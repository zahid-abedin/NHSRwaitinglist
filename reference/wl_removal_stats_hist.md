# Calculate some stats about removals from histogram data

Calculate removal statistics from a histogram containing multiple report
dates. Compares consecutive snapshots to estimate removal rates over
time.

## Usage

``` r
wl_removal_stats_hist(wl_hist, start_date = NULL, end_date = NULL)
```

## Arguments

- wl_hist:

  data.frame. A histogram with columns: arrival_since, arrival_before,
  n, and report_date. Must contain at least 2 unique report_date values.

- start_date:

  Date or character (in format 'YYYY-MM-DD'); The start date to
  calculate from. If NULL, uses the earliest report_date in the
  histogram.

- end_date:

  Date or character (in format 'YYYY-MM-DD'); The end date to calculate
  to. If NULL, uses the latest report_date in the histogram.

## Value

A data.frame with the following summary statistics on removals/capacity:

- capacity_weekly:

  Numeric. Mean number of removals from the waiting list per week,
  averaged across all consecutive time periods.

- capacity_daily:

  Numeric. Mean number of removals from the waiting list per day,
  averaged across all consecutive time periods.

- capacity_cov:

  Numeric. Coefficient of variation in the time between removals from
  the waiting list (currently fixed at 1).

- removal_count:

  Numeric. Total number of removals from the waiting list over the full
  time period.

## Examples

``` r
if (FALSE) { # \dontrun{
# Histogram with multiple report dates
wl_hist <- data.frame(
  arrival_since = as.Date(c("2024-01-01", "2024-01-08")),
  arrival_before = as.Date(c("2024-01-07", "2024-01-14")),
  n = c(100, 120),
  report_date = as.Date(c("2024-01-31", "2024-02-29"))
)
removal_stats <- wl_removal_stats_hist(wl_hist)
} # }
```
