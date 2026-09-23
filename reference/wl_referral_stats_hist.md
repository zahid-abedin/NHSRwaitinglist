# Calculate some stats about referrals from histogram data

Calculate referral statistics from a histogram containing multiple
report dates.

## Usage

``` r
wl_referral_stats_hist(
  wl_hist,
  start_date = NULL,
  end_date = NULL,
  time_interval = "weeks"
)
```

## Arguments

- wl_hist:

  data.frame. A histogram with columns: arrival_since, arrival_before,
  n, and report_date.

- start_date:

  Date or character (in format 'YYYY-MM-DD'); The start date to
  calculate from.

- end_date:

  Date or character (in format 'YYYY-MM-DD'); The end date to calculate
  to.

- time_interval:

  Character or numeric. Passed through to format_histogram() when
  deriving the histogram end date.

## Value

A data.frame with the following summary statistics on referrals/demand:

- demand_weekly:

  Numeric. Mean number of additions to the waiting list per week.

- demand_daily:

  Numeric. Mean number of additions to the waiting list per day.

- demand_cov:

  Numeric. Coefficient of variation in the time between additions to the
  waiting list.

- demand_count:

  Numeric. Total demand over the full time period.

## Examples

``` r
referrals <- as.Date(c("2024-01-01", "2024-01-04", "2024-01-10", "2024-01-16"))
removals <- as.Date(c("2024-01-08", NA, NA, NA))
hist_waiting_list <- data.frame(
  arrival_since = as.Date(c("2024-01-01", "2024-01-08")),
  arrival_before = as.Date(c("2024-01-07", "2024-01-14")),
  n = c(100, 120),
  report_date = as.Date(c("2024-01-31", "2024-02-29"))
)
referral_stats <- wl_referral_stats_hist(hist_waiting_list)
```
