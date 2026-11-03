# goit-rdb-hw-03

Homework 2 (Topic 3): loading, cleaning, data-quality audit and EDA of NYC Yellow Taxi trips in PostgreSQL.

## Dataset

- **Source:** [NYC TLC Trip Record Data](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page), Yellow Taxi, March 2024 (`yellow_tripdata_2024-03.parquet`, 3,582,628 rows), and the zone lookup `taxi_zone_lookup.csv`.
- **Sample:** 100,000 random rows (`pandas.sample(n=100_000, random_state=42)`), 19 columns, sorted by pickup time and saved to [data/hw3_taxi_sample.csv](data/hw3_taxi_sample.csv). The sample is deterministic, so a re-run produces the same file.
- **After cleaning:** `hw3_taxi_staging` has 100,000 rows and `hw3_taxi_trips` has 99,994. 6 trips with a drop-off earlier than the pickup were dropped.

## Running

Open [hw03_nyc_taxi_load_eda.ipynb](hw03_nyc_taxi_load_eda.ipynb) in Google Colab and run Restart & Run All. PostgreSQL 16 starts through `pgserver`. If `data/hw3_taxi_sample.csv` is missing, the notebook downloads the Parquet file from the official TLC CDN and builds the sample itself.

Locally: `uv sync`, then the same notebook on the `.venv` kernel.

## What is inside

1. `COPY FROM STDIN` into staging tables with `TEXT` columns.
2. The typed table `hw3_taxi_trips`:
   - `TIMESTAMPTZ` through `AT TIME ZONE 'America/New_York'`;
   - `DATE` for the pickup date;
   - `NUMERIC(10,2)` for amounts;
   - `BOOLEAN` for `store_and_fwd`;
   - 7 `CHECK`s and a foreign key to the zone lookup.
3. Audit:
   - row counts in staging and clean;
   - duplicates by business key, including "receipt + reversal" pairs;
   - null rate of seven columns and the markers `''`, `99`, `0`, `Unknown`, `N/A`.
4. Rate-code distribution, distance and amount statistics, a domain outlier check.
5. Ten queries in the format business-question / SQL / interpretation.
6. Reflection.
