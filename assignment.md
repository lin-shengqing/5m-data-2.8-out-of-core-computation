# Assignment

## Brief

Write the Python codes for the following questions.

## Instructions

Paste the answer as Python in the answer code section below each question.

### Question 1

Question: Join the `metadata_pl` and `ratings_pl` DataFrames in Polars, then calculate the average rating by `genre` and by `original_language` (separately).

Answer:

```python
import polars as pl
# set n_rows to 1000000 to prevent out of memory when join
metadata_pl = pl.read_csv("data/movies_metadata.csv", infer_schema_length=100000, n_rows=1000000)
ratings_pl = pl.read_csv("data/ratings.csv", infer_schema_length=100000, n_rows=1000000)

# filter out id column that are str, so that id will be int64
metadata_pl = (
    metadata_pl
    .filter(pl.col("id").str.contains(r"^\d+$"))
    .with_columns(
        pl.col("id").cast(pl.Int64)
    )
)

movie_pl = metadata_pl.join(ratings_pl, left_on="id", right_on='movieId')

movie_pl.group_by("genres").agg(
    pl.mean("rating").alias("avg_rating")
)

movie_pl.group_by("original_language").agg(
    pl.mean("rating").alias("avg_rating")
)
```

### Question 2

Question: Calculate the average total amount for vendors with at least 5 trips from the NYC Taxi Trip Data.

Answer:

```python
import polars as pl

taxi_pl = pl.read_csv("data/taxi_trip_data.csv")

taxi_results = taxi_pl.group_by("vendor_id").agg(
    pl.count("total_amount").alias("trips_count"),
    pl.mean("total_amount").alias("avg_amount")
).filter(pl.col('trips_count') >= 5)
taxi_results
```

## Submission

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
