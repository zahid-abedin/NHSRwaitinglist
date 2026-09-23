# Format Histogram Data for Waiting List Metrics

This function formats a histogram data frame by ensuring the presence
and correct formatting of `arrival_since` and `arrival_before` columns.
If `arrival_before` does not exist, it is generated for each group as
the previous `arrival_since` date (or the end date for the first row in
each group).

## Usage

``` r
format_histogram(
  histogram,
  group_columns = NULL,
  end_date = NULL,
  time_interval = "weeks"
)
```

## Arguments

- histogram:

  A data frame containing at least an `arrival_since` column (as
  character or Date).

- group_columns:

  A character vector specifying the column(s) to group by when
  generating `arrival_before`.

- end_date:

  Optional. The reference date to use for the first `arrival_before` in
  each group. Defaults to the system date.

- time_interval:

  Optional. Controls how the default `end_date` is derived when it is
  not supplied. Use `weeks` (default), `months`, `NULL`, or a numeric
  offset.

## Value

A data frame with `arrival_since` and `arrival_before` columns as Dates,
and all original columns. The columns are reordered so that
`arrival_since` and `arrival_before` appear first.

## Details

- If the `arrival_before` column exists, it is converted to Date.

- If not, for each group (as defined by `group_columns`),
  `arrival_before` is set to the previous `arrival_since` (or `end_date`
  for the first row), minus one day.

## Examples

``` r
if (FALSE) { # \dontrun{
df <- data.frame(
  group = c("A", "A", "B"),
  arrival_since = c("2023-01-01", "2023-02-01", "2023-01-15")
)
format_histogram(df, group_columns = "group")
} # }
```
