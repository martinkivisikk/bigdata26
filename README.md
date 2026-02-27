# bigdata26

## Project structure

- Provide the input data (`work/data/inbox/`) before running the pipeline.

```bash
work/data/inbox/
    *.parquet
```

## Transformations

### Cleaning Rules

| # | Rule |
|---|------|
| 1 | Drop rows where any critical field is null: `pickup_ts`, `dropoff_ts`, `PULocationID`, `DOLocationID`, `total_amount` |
| 2 | `dropoff_ts` must be strictly greater than `pickup_ts` |
| 3 | `passenger_count` must be in range 1–8, otherwise set to `NULL` |
| 4 | `trip_distance` must be > 0 |
| 5 | Monetary fields must be ≥ 0, otherwise set to `NULL` |

### Deduplication Rules

Rows are deduplicated on the following business key, which uniquely identifies a taxi trip event:
```python
(VendorID, pickup_ts, dropoff_ts, PULocationID, DOLocationID)
```