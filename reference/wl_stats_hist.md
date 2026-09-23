# Calculate waiting list statistics from histogram data

A summary of all the key stats associated with a waiting list using
histogram format data with multiple report dates.

## Usage

``` r
wl_stats_hist(wl_hist, target_wait = 4, start_date = NULL, end_date = NULL)
```

## Arguments

- wl_hist:

  data.frame. A histogram with columns: arrival_since, arrival_before,
  n, and report_date. Must contain at least 2 unique report_date values.

- target_wait:

  numeric. The required waiting time in weeks.

- start_date:

  Date or character (in format 'YYYY-MM-DD'); The start date for
  calculating removal statistics. If NULL, uses earliest report_date.

- end_date:

  Date or character (in format 'YYYY-MM-DD'); The end date for
  calculating removal statistics. If NULL, uses latest report_date.

## Value

A data.frame of key waiting list summary statistics based on queueing
theory:

- mean_demand:

  Numeric. Mean number of additions to the waiting list per week.

- mean_capacity:

  Numeric. Mean number of removals from the waiting list per week.

- load:

  Numeric. Ratio between demand and capacity.

- load_too_big:

  Logical. Whether the load is greater than or equal to 1, indicating
  whether the waiting list is unstable and expected to grow.

- count_demand:

  Numeric. Total demand (i.e., number of referrals) over the full time
  period.

- queue_size:

  Numeric. Number of patients on the waiting list at the end of the time
  period.

- target_queue_size:

  Numeric. The recommended size of the waiting list to achieve
  approximately 98.2% of patients being treated within their target wait
  time. This is based on Little’s Law, assuming the system is in
  equilibrium, with the average waiting time set to one-quarter of the
  `target_wait`.

- queue_too_big:

  Logical. Whether `queue_size` is more than twice the
  `target_queue_size`. A value of `TRUE` indicates the queue is at risk
  of missing its targets.

- mean_wait_age:

  Numeric. Mean waiting age in days.

- mean_wait:

  Numeric. Legacy alias for `mean_wait_age`.

- cv_arrival:

  Numeric. Coefficient of variation in the time between additions to the
  waiting list.

- cv_removal:

  Numeric. Coefficient of variation in the time between removals from
  the waiting list.

- target_capacity:

  Numeric. The weekly treatment capacity required to maintain the
  waiting list at its target equilibrium, assuming the target queue size
  has been reached.

- relief_capacity:

  Numeric. The temporary weekly capacity required to reduce the waiting
  list to its `target_queue_size` within 26 weeks, assuming current
  demand remains steady. Calculated only if `queue_too_big` is `TRUE`;
  otherwise returns `NA`.

- pressure:

  Numeric. A measure of pressure on the system, defined as
  `2 × mean_wait_age / target_wait`. Values greater than 1 suggest the
  system is unlikely to meet its waiting time targets.

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
waiting_list_stats <- wl_stats_hist(wl_hist, target_wait = 18)
} # }
```
