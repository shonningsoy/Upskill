# STG_TC_ITEM Pipeline Documentation

> A complete guide to the data pipeline that feeds `PROD.STAGING.STG_TC_ITEM` — from raw JSON files in Google Cloud Storage all the way to the final item-level table in Snowflake.

---

## Pipeline Overview

This pipeline ingests **consumer session messages** from TOMRA's reverse vending machines (RVMs) across three regions (AU, EU, US), parses the nested JSON payloads, and flattens individual **item-level** records (each bottle, can, or container processed by a machine) into a queryable table.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        EXTERNAL (Google Cloud)                          │
│                                                                         │
│   RVM Cloud Backend → JSON files → GCS Buckets (AU / EU / US)          │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │  Pub/Sub notification
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     LAYER 1-4: INGESTION (Snowflake)                    │
│                                                                         │
│   External Stages → Snowpipes → Raw Tables (GCS_TC_OCMESSAGES_xx)     │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │  Streams (CDC)
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     LAYER 5-7: PARSING & DEDUP                         │
│                                                                         │
│   Streams → Tasks (every 3h) → STG_TC_CONSUMER_SESSION_3              │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │  Stream (CDC)
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     LAYER 8-10: ITEM FLATTENING                        │
│                                                                         │
│   Stream → Task (every 3h +40min) → STG_TC_ITEM                       │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Layer 1: External Source — GCS Buckets

### What

JSON files land in three regional Google Cloud Storage buckets:

| Region    | GCS Bucket URL                                                    |
|-----------|-------------------------------------------------------------------|
| Australia | `gcs://snowpipe-enriched-oc-messages-au-prod-analytics-prod-4ee9` |
| Europe    | `gcs://snowpipe-enriched-oc-messages-eu-prod-analytics-prod-4ee9` |
| US        | `gcs://snowpipe-enriched-oc-messages-us-prod-analytics-prod-4ee9` |

### Why regional separation?

- Data residency / compliance — keeping data close to its origin
- Fault isolation — a problem in one region doesn't block others
- The upstream backend (TOMRA's operational cloud) produces messages regionally

### What the files look like

Each JSON file contains one or more messages with this general structure:

```json
{
  "installationAggregate": { ... },
  "messageType": "consumer_session",
  "serialNumber": "NO123456",
  "ocMessage": {
    "messageId": "abc-123",
    "messageHash": "...",
    "installationId": "INST-001",
    "date": "2025-01-15T10:30:00Z",
    "session_start": "2025-01-15T10:30:00Z",
    "session_end": "2025-01-15T10:32:15Z",
    "items": [
      {
        "crate_items": [
          { "type": "bottle", "weight": 25, "value": 100, "properties": { "bc": "5901234123457", ... } }
        ]
      }
    ]
  }
}
```

### Retention

The stage is a **live window** into the GCS bucket — Snowflake stores nothing here. Whether historical files are available depends entirely on the bucket's lifecycle policy in GCP. Once Snowpipe loads a file, the data is safe in Snowflake regardless of what happens to the file in GCS.

---

## Layer 2: External Stages

### What

External stages are named Snowflake objects that point to the GCS bucket locations:

| Stage                                    | Points To                              | Storage Integration                              |
|------------------------------------------|----------------------------------------|--------------------------------------------------|
| `PROD.RAW.GCS_STAGE_TC_OCMESSAGES_AU`   | `gcs://snowpipe-enriched-...au-...`    | `GCS_STORAGEINTEGRATION_TC_OCMESSAGES_AU_PROD`   |
| `PROD.RAW.GCS_STAGE_TC_OCMESSAGES_EU`   | `gcs://snowpipe-enriched-...eu-...`    | `GCS_STORAGEINTEGRATION_TC_OCMESSAGES_EU_PROD`   |
| `PROD.RAW.GCS_STAGE_TC_OCMESSAGES_US`   | `gcs://snowpipe-enriched-...us-...`    | `GCS_STORAGEINTEGRATION_TC_OCMESSAGES_US_PROD`   |

### Why

- Stages provide an **abstraction layer** — the Snowpipe and COPY INTO commands reference the stage name, not the raw GCS URL
- The **storage integration** handles authentication via a GCP service account, avoiding embedded credentials
- If the bucket URL ever changes, only the stage definition needs updating — downstream objects remain untouched

---

## Layer 3: Snowpipes (Auto-Ingest)

### What

Snowpipes are always-on ingestion services that automatically load files as they arrive:

| Pipe                                   | Loads Into                          |
|----------------------------------------|-------------------------------------|
| `PROD.RAW.GCS_PIPE_TC_OCMESSAGES_AU`  | `PROD.RAW.GCS_TC_OCMESSAGES_AU`    |
| `PROD.RAW.GCS_PIPE_TC_OCMESSAGES_EU`  | `PROD.RAW.GCS_TC_OCMESSAGES_EU`    |
| `PROD.RAW.GCS_PIPE_TC_OCMESSAGES_US`  | `PROD.RAW.GCS_TC_OCMESSAGES_US`    |

### How it triggers

```
New JSON file in GCS bucket
       │
       ▼
GCS sends Pub/Sub notification
       │
       ▼
Snowflake notification integration receives event
       │
       ▼
Snowpipe wakes up and runs COPY INTO
       │
       ▼
Data lands in raw table (typically within seconds to minutes)
```

### What the pipe does (simplified)

```sql
COPY INTO GCS_TC_OCMESSAGES_EU (
    installationAggregate,   -- full aggregate payload (VARIANT)
    messageType,             -- e.g. 'consumer_session'
    ocMessage,               -- the main message payload (VARIANT)
    serialNumber,            -- machine serial number
    messageId,               -- unique message ID
    messageHash,             -- upstream-provided hash
    ocmessageHash,           -- computed dedup hash (SHA2_HEX)
    file_name,               -- METADATA$FILENAME
    file_row_number,         -- row position within the file
    file_content_key,        -- unique file content identifier
    file_last_modified,      -- when file was last modified in GCS
    start_scan_time,         -- when Snowpipe started scanning
    LOAD_TIME,               -- CURRENT_TIMESTAMP()
    SOURCE                   -- literal: 'GCS_TC_OCMESSAGES_EU'
)
FROM (
    SELECT ...
    FROM @GCS_STAGE_TC_OCMESSAGES_EU
)
FILE_FORMAT = (TYPE = 'JSON');
```

### Why Snowpipe?

- **Near real-time** — no waiting for a batch schedule; files load within seconds of arrival
- **Serverless** — no warehouse needed; pay only for compute used
- **Exactly-once loading** — Snowpipe tracks file metadata and won't reload the same file twice
- **The `ocmessageHash`** is computed at this stage via `SHA2_HEX(date || installationId || session_end || session_start || serialNumber || user_id || items)` to enable downstream deduplication

---

## Layer 4: Raw Tables

### What

These are the "landing zone" tables where data arrives in its most raw form:

| Table                              | Clustering Key | Purpose                  |
|------------------------------------|----------------|--------------------------|
| `PROD.RAW.GCS_TC_OCMESSAGES_AU`   | (none shown)   | Raw AU messages          |
| `PROD.RAW.GCS_TC_OCMESSAGES_EU`   | (none shown)   | Raw EU messages          |
| `PROD.RAW.GCS_TC_OCMESSAGES_US`   | (none shown)   | Raw US messages          |

### Why keep raw?

- **Auditability** — you can always trace back to the original payload
- **Reprocessing** — if downstream logic changes, you can replay from raw
- **Debugging** — when something looks wrong downstream, the raw table is the source of truth
- The `ocMessage` column is stored as VARIANT (semi-structured), preserving the full nested JSON without schema enforcement

---

## Layer 5: Streams (Change Data Capture on Raw Tables)

### What

Streams are Snowflake's built-in CDC (Change Data Capture) mechanism. They sit on top of a table and track what's new:

| Stream                                                 | Source Table                        |
|--------------------------------------------------------|-------------------------------------|
| `PROD.STAGING.ST_TC_OCMESSAGES_CONSUMER_SESSION_3_AU` | `PROD.RAW.GCS_TC_OCMESSAGES_AU`    |
| `PROD.STAGING.ST_TC_OCMESSAGES_CONSUMER_SESSION_3_EU` | `PROD.RAW.GCS_TC_OCMESSAGES_EU`    |
| `PROD.STAGING.ST_TC_OCMESSAGES_CONSUMER_SESSION_3_US` | `PROD.RAW.GCS_TC_OCMESSAGES_US`    |

All are **DELTA** type streams (only tracking inserts, which is all that happens on these tables).

### How a stream works

```
Raw Table:      [row1] [row2] [row3] [row4] [row5] [row6] [row7]
                                      ▲                     ▲
Stream offset (last consumed) ────────┘                     │
                                                            │
New rows visible to stream: [row5] [row6] [row7] ──────────┘
```

- The stream maintains a **pointer** (offset) into the table
- When you query the stream, you only see rows **added since the last successful consume**
- Once the downstream task commits its transaction, the offset advances automatically
- If the task fails, the offset stays put — rows reappear on the next attempt (exactly-once semantics)

### Why streams instead of watermark queries?

- **No custom tracking logic needed** — no WHERE clauses like `WHERE load_time > last_processed_time`
- **Atomic offset advancement** — tied to transaction commit, so no risk of skipping or double-processing
- **Efficient** — Snowflake uses internal metadata (micro-partition versioning) to identify changes without scanning the entire table

---

## Layer 6: Tasks (Raw → Intermediate Staging)

### What

Three scheduled tasks parse the raw JSON and insert clean, tabular data into an intermediate table:

| Task                                                    | Schedule                | State   |
|---------------------------------------------------------|-------------------------|---------|
| `PROD.STAGING.TA_TC_OCMESSAGES_CONSUMER_SESSION_3_AU`  | `CRON 0 1/3 * * * UTC`  | started |
| `PROD.STAGING.TA_TC_OCMESSAGES_CONSUMER_SESSION_3_EU`  | `CRON 0 1/3 * * * UTC`  | started |
| `PROD.STAGING.TA_TC_OCMESSAGES_CONSUMER_SESSION_3_US`  | `CRON 0 1/3 * * * UTC`  | started |

**Schedule:** Every 3 hours at minute 0 (01:00, 04:00, 07:00, 10:00, 13:00, 16:00, 19:00, 22:00 UTC).

### What the task does (conceptual)

```sql
INSERT INTO PROD.STAGING.STG_TC_CONSUMER_SESSION_3 (
    INSTALLATION_AGGREGATE, MESSAGE_TYPE, OC_MESSAGE,
    MESSAGE_ID, MESSAGE_HASH, OC_MESSAGE_HASH,
    LOAD_TIME, PROCESSED_TIME, SOURCE, ENVIRONMENT,
    -- Parsed session fields:
    INSTALLATION_ID, SERIAL_NUMBER, DATE, SESSION_START, SESSION_END, ...
    -- Enrichment fields (location, organization):
    LOCATION_COUNTRYCODE, LOCATION_CITY, ORGANIZATIONUNIT_NAME, ...
)
SELECT
    DEDUP.INSTALLATIONAGGREGATE,
    DEDUP.MESSAGETYPE,
    ...
    PARSE_JSON(DEDUP.OCMESSAGE):installationId::VARCHAR AS installation_id,
    PARSE_JSON(DEDUP.OCMESSAGE):date::TIMESTAMP_NTZ AS date,
    PARSE_JSON(DEDUP.OCMESSAGE):session_start::TIMESTAMP_NTZ AS session_start,
    ...
    -- Enrichment from installationAggregate:
    DEDUP.INSTALLATIONAGGREGATE:location.countryCode::VARCHAR AS location_countryCode,
    DEDUP.INSTALLATIONAGGREGATE:location.city::VARCHAR AS location_city,
    ...
FROM (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY OC_MESSAGE_HASH ORDER BY LOAD_TIME DESC) AS rn
    FROM ST_TC_OCMESSAGES_CONSUMER_SESSION_3_EU  -- reads from stream
) DEDUP
WHERE DEDUP.rn = 1  -- deduplication
```

### Key transformations

1. **JSON parsing** — Extracts structured fields from the nested `ocMessage` and `installationAggregate` VARIANT columns
2. **Deduplication** — Uses `QUALIFY ROW_NUMBER() OVER (PARTITION BY OC_MESSAGE_HASH ORDER BY LOAD_TIME DESC) = 1` to keep only the latest version of each message (in case of duplicates from upstream)
3. **Enrichment** — Flattens location metadata (country, city, timezone, currency) and organization hierarchy (organization unit, parent, path) from the `installationAggregate`

### Why separate tasks per region?

- **Fault isolation** — if the AU task fails, EU and US still run
- **Parallelism** — all three tasks run at the same time on the same schedule
- **Clarity** — each task's SOURCE column identifies where the data came from

---

## Layer 7: Intermediate Staging Table

### What

| Table                                    | Clustering Key                   |
|------------------------------------------|----------------------------------|
| `PROD.STAGING.STG_TC_CONSUMER_SESSION_3` | `(OC_MESSAGE_HASH, MESSAGE_ID)`  |

This table contains **one row per consumer session** — a complete visit by a customer to a reverse vending machine.

### Why this intermediate step?

The final target (`STG_TC_ITEM`) needs **one row per item**. But each session contains multiple items (nested arrays). This intermediate table:

1. Provides a **clean, deduplicated, parsed** view of sessions before flattening
2. Serves as the **source for the item-level stream** — so the item task only processes new sessions
3. Can be queried independently for **session-level analytics** (without the explosion of item rows)

### Column overview

- **Message identifiers:** `MESSAGE_ID`, `MESSAGE_HASH`, `OC_MESSAGE_HASH`
- **Session data:** `DATE`, `SESSION_START`, `SESSION_END`, `USER_ID`, `VALUE`
- **Machine identity:** `INSTALLATION_ID`, `SERIAL_NUMBER`
- **Location metadata:** `LOCATION_COUNTRYCODE`, `LOCATION_CITY`, `LOCATION_TIMEZONE`, `LOCATION_CURRENCY`, etc.
- **Organization hierarchy:** `ORGANIZATIONUNIT_ID`, `ORGANIZATIONUNIT_NAME`, `ORGANIZATIONUNIT_ORGANIZATIONNAME_PATH`
- **Raw JSON preserved:** `ITEMS_JSON`, `PROPERTIES_JSON` (for downstream flattening)

---

## Layer 8: Stream (CDC on Intermediate Table)

### What

| Stream                                    | Source Table                              |
|-------------------------------------------|-------------------------------------------|
| `PROD.STAGING.ST_TC_OCMESSAGES_ITEM`     | `PROD.STAGING.STG_TC_CONSUMER_SESSION_3`  |

### Why another stream?

Same principle as Layer 5 — this stream ensures the item-flattening task only processes **newly arrived sessions**, not the entire table. Without it, every run would need to re-scan and de-duplicate against the full item table.

---

## Layer 9: Task (Item Flattening)

### What

| Task                                       | Schedule                    | State   |
|--------------------------------------------|-----------------------------|---------|
| `PROD.STAGING.TA_TC_OCMESSAGES_ITEM`      | `CRON 40 1/3 * * * UTC`    | started |

**Schedule:** Every 3 hours at minute 40 (01:40, 04:40, 07:40, etc.) — deliberately offset 40 minutes after the upstream tasks to ensure fresh data is available.

**Timeout:** 5,400,000 ms (90 minutes) — this is a heavy flattening operation.

### What it does

This task takes each session and "explodes" it into individual item rows using **LATERAL FLATTEN**:

```sql
INSERT INTO PROD.STAGING.STG_TC_ITEM (...)

-- Branch 1: Items from the stream (new sessions)
SELECT
    s.MESSAGE_ID,
    s.MESSAGE_HASH,
    ...
    f2.value:type::STRING AS TYPE,
    f2.value:weight::INT AS WEIGHT,
    f2.value:value::INT AS VALUE,
    f2.value:properties.bc::STRING AS PROPERTIES_BC,   -- barcode
    f2.value:properties.mat::STRING AS PROPERTIES_MAT, -- material
    f2.value:properties.col::STRING AS PROPERTIES_COL, -- color
    ...
FROM ST_TC_OCMESSAGES_ITEM s,                          -- stream
    LATERAL FLATTEN(input => oc_Message:items) f,       -- first level: item groups
    LATERAL FLATTEN(input => f.value:crate_items) f2    -- second level: individual items

UNION ALL

-- Branch 2: Additional item extraction from session-level properties
SELECT ...
FROM STG_TC_CONSUMER_SESSION_3
    LATERAL FLATTEN(...) f
WHERE ...
```

### Why nested LATERAL FLATTEN?

The JSON structure has items nested two levels deep:

```json
{
  "items": [                          ← LATERAL FLATTEN #1 (item groups / crates)
    {
      "crate_items": [                ← LATERAL FLATTEN #2 (individual items)
        { "type": "bottle", "weight": 25, "value": 100, "properties": {...} },
        { "type": "can", "weight": 15, "value": 100, "properties": {...} }
      ]
    }
  ]
}
```

Each individual item becomes its own row in the final table.

### Why UNION ALL with two branches?

The task handles two different message formats/sources in a single pass:
1. New sessions from the stream (via the nested `oc_Message:items → crate_items` path)
2. Sessions with a different item structure at the top level

This ensures backwards compatibility and handling of varying message schemas.

---

## Layer 10: Final Target — STG_TC_ITEM

### What

| Table                          | Clustering Key    | Columns |
|--------------------------------|-------------------|---------|
| `PROD.STAGING.STG_TC_ITEM`    | `PROCESSED_TIME`  | 87      |

### What it contains

**One row per individual item** (bottle, can, container) processed by a TOMRA reverse vending machine.

### Key column groups

| Group              | Example Columns                                             | Description                          |
|--------------------|-------------------------------------------------------------|--------------------------------------|
| Identifiers        | `MESSAGE_ID`, `MESSAGE_HASH`, `OC_MESSAGE_HASH`            | Traceability back to source message  |
| Timing             | `DATE`, `SESSION_START`, `SESSION_END`, `PROCESSED_TIME`    | When the item was processed          |
| Machine            | `INSTALLATION_ID`, `SERIAL_NUMBER`                          | Which machine handled it             |
| Item properties    | `TYPE`, `WEIGHT`, `VALUE`, `PROPERTIES_BC` (barcode)        | What the item is                     |
| Material/Shape     | `PROPERTIES_MAT`, `PROPERTIES_COL`, `PROPERTIES_S/B/G`     | Material, color, size                |
| Location           | `LOCATION_COUNTRYCODE`, `LOCATION_CITY`, `LOCATION_REGION`  | Where the machine is                 |
| Organization       | `ORGANIZATIONUNIT_NAME`, `ORGANIZATIONUNIT_ORGANIZATIONNAME_PATH` | Who operates the machine       |
| Metadata           | `LOAD_TIME`, `PROCESSED_AT`, `RECEIVED_AT`, `SOURCE`        | Pipeline tracking                    |

### Why clustered on PROCESSED_TIME?

Most queries against this table filter by time ("items in the last week", "daily volumes"). Clustering on `PROCESSED_TIME` ensures these queries scan fewer micro-partitions, resulting in faster and cheaper queries.

---

## Timing & Orchestration

```
Hour:  00:00   01:00        01:40        04:00        04:40   ...
       │       │            │            │            │
       │       ▼            │            ▼            │
       │  Tasks run:        │       Tasks run:        │
       │  - Parse AU        │       - Parse AU        │
       │  - Parse EU        │       - Parse EU        │
       │  - Parse US        │       - Parse US        │
       │       │            ▼            │            ▼
       │       │       Item task:        │       Item task:
       │       │       - Flatten         │       - Flatten
       │       │                         │
       ▼       ▼            ▼            ▼            ▼
  Snowpipe runs continuously (event-driven, near real-time)
```

The 40-minute offset between Layer 6 tasks and the Layer 9 task gives the parsing tasks time to complete before the item task reads from the stream. If parsing takes longer than expected, the item task simply sees fewer new rows (and catches up on the next cycle).

---

## Error Handling & Resilience

| Mechanism              | What it protects against                                    |
|------------------------|-------------------------------------------------------------|
| Snowpipe file tracking | Won't reload an already-ingested file                       |
| SHA2 hash (`ocmessageHash`) | Detects and deduplicates duplicate messages from upstream |
| Stream offsets         | Only advance on commit — failed tasks replay automatically  |
| Regional separation    | One region's failure doesn't block others                   |
| 90-minute timeout      | Prevents runaway item flattening from blocking resources    |
| Clustering keys        | Keeps query performance predictable as tables grow          |

---

## Summary: Why This Architecture?

| Design Choice                    | Reason                                                                    |
|----------------------------------|---------------------------------------------------------------------------|
| Snowpipe (not batch COPY)        | Near real-time ingestion without managing schedules for file arrival       |
| Raw tables with VARIANT          | Preserves full payload; schema changes upstream don't break ingestion      |
| Streams (not watermarks)         | Atomic, exactly-once processing without custom tracking logic              |
| Intermediate session table       | Clean separation between session-level and item-level concerns             |
| LATERAL FLATTEN for items        | Native Snowflake way to unnest JSON arrays into rows                       |
| Regional tasks                   | Parallel processing, fault isolation, clear data lineage                   |
| Scheduled tasks (not continuous) | Cost control — 3-hour batches balance latency vs. compute cost             |
| 40-minute offset                 | Ensures upstream tasks finish before downstream reads                      |

---

## Quick Reference: All Objects

```
PROD.RAW
├── GCS_STAGE_TC_OCMESSAGES_AU (external stage)
├── GCS_STAGE_TC_OCMESSAGES_EU (external stage)
├── GCS_STAGE_TC_OCMESSAGES_US (external stage)
├── GCS_PIPE_TC_OCMESSAGES_AU  (snowpipe)
├── GCS_PIPE_TC_OCMESSAGES_EU  (snowpipe)
├── GCS_PIPE_TC_OCMESSAGES_US  (snowpipe)
├── GCS_TC_OCMESSAGES_AU       (raw table)
├── GCS_TC_OCMESSAGES_EU       (raw table)
└── GCS_TC_OCMESSAGES_US       (raw table)

PROD.STAGING
├── ST_TC_OCMESSAGES_CONSUMER_SESSION_3_AU  (stream on raw AU)
├── ST_TC_OCMESSAGES_CONSUMER_SESSION_3_EU  (stream on raw EU)
├── ST_TC_OCMESSAGES_CONSUMER_SESSION_3_US  (stream on raw US)
├── TA_TC_OCMESSAGES_CONSUMER_SESSION_3_AU  (task: parse AU)
├── TA_TC_OCMESSAGES_CONSUMER_SESSION_3_EU  (task: parse EU)
├── TA_TC_OCMESSAGES_CONSUMER_SESSION_3_US  (task: parse US)
├── STG_TC_CONSUMER_SESSION_3               (intermediate table)
├── ST_TC_OCMESSAGES_ITEM                   (stream on intermediate)
├── TA_TC_OCMESSAGES_ITEM                   (task: flatten items)
└── STG_TC_ITEM                             (final target table)
```
