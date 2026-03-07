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

## Custom Scenario

In addition to the enriched trip dataset, the pipeline produces an aggregated view of taxi flows between boroughs.

The dataset `borough_flows.parquet` summarizes trips by:

- `pickup_borough`
- `dropoff_borough`
- `pickup_year_month`

For each borough pair and month the pipeline computes:

- `trip_count` – total number of trips
- `avg_trip_distance` – average trip distance

Example observation: the dominant flow in the dataset is **Manhattan → Manhattan** with more than 2.8 million trips which indicates that most yellow taxi trips occur within Manhattan rather than between boroughs.