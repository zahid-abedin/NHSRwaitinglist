# Aggregate a histogram to remove categorical variables and sum to a single waiting list

(arrival_since, arrival_before) are the start and end dates of the
arrival time interval. The function aggregates the histogram by summing
the counts. (The arrival_since and arrival_before columns are not
aggregated.)

## Usage

``` r
aggregate_histogram(histogram, group_columns = NULL)
```

## Arguments

- histogram:

  a waiting list in histogram format, possibly including counts for
  different categorical variables

- group_columns:

  Optional character vector of additional column names to group by (in
  addition to arrival_since and arrival_before). Default is NULL.

## Value

a dataframe summarised to a single aggregated waiting list
