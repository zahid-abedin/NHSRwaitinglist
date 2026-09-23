# Analysing Waiting Lists with Histogram Data

``` r

library(NHSRwaitinglist)
library(dplyr)
#> 
#> Attaching package: 'dplyr'
#> The following objects are masked from 'package:stats':
#> 
#>     filter, lag
#> The following objects are masked from 'package:base':
#> 
#>     intersect, setdiff, setequal, union
library(ggplot2)

# set up a theme for our charts
theme_set(
  theme_minimal() +
    theme(
      plot.subtitle = element_text(face = "italic")
    )
)
```

## Introduction

The NHSRwaitinglist package provides two complementary approaches for
analysing waiting lists:

1.  **Patient-level data**: Individual records with referral and removal
    dates
2.  **Histogram data**: Aggregated counts by waiting time intervals

This vignette focuses on the **histogram approach**, which is
particularly valuable for:

- **Processing NHS national statistics**: Published data is aggregated,
  not patient-level
- **Privacy-preserving analysis**: No individual patient records
  required
- **Large dataset performance**: Efficient computation with
  time-bucketed summaries
- **Historical data**: Working with pre-aggregated reported statistics

### When to Use Histogram Functions

Use histogram functions when your data is:

- Aggregated into time bins (weeks, months) with patient counts
- Published in summary form (e.g., NHS England RTT statistics)
- Too large for patient-level processing
- Subject to privacy constraints

### Histogram Data Structure

A histogram in this package is a data frame with these essential
columns:

- `arrival_since` (Date): Start of the arrival time interval
- `arrival_before` (Date): End of the arrival time interval  
- `n` (Numeric): Count of patients in that interval
- `report_date` (Date): Reference date of the waiting list snapshot

Additional categorical columns (e.g., specialty, priority) are optional.

## Creating Histogram Data

### Example: Converting Weekly Bins

``` r

# Example: Waiting list snapshot from 31 Jan 2024
# Bins represent weeks waiting
wl_hist_jan <- data.frame(
  arrival_since = as.Date(c("2024-01-24", "2024-01-17", "2024-01-10", "2024-01-03")),
  arrival_before = as.Date(c("2024-01-30", "2024-01-23", "2024-01-16", "2024-01-09")),
  n = c(45, 38, 52, 61), # Patients waiting 0-1, 1-2, 2-3, 3-4 weeks
  report_date = as.Date("2024-01-31")
)

print(wl_hist_jan)
#>   arrival_since arrival_before  n report_date
#> 1    2024-01-24     2024-01-30 45  2024-01-31
#> 2    2024-01-17     2024-01-23 38  2024-01-31
#> 3    2024-01-10     2024-01-16 52  2024-01-31
#> 4    2024-01-03     2024-01-09 61  2024-01-31
```

The first row shows 45 patients who arrived between 24-30 Jan (waiting
\< 1 week on 31 Jan).

### Multiple Time Points

For removal statistics, we need multiple snapshots:

``` r

# Add February snapshot
wl_hist_feb <- data.frame(
  arrival_since = as.Date(c("2024-02-21", "2024-02-14", "2024-02-07", "2024-01-31")),
  arrival_before = as.Date(c("2024-02-27", "2024-02-20", "2024-02-13", "2024-02-06")),
  n = c(42, 35, 48, 55),
  report_date = as.Date("2024-02-29")
)

# Combine into single histogram
wl_hist <- rbind(wl_hist_jan, wl_hist_feb)

# View unique report dates
unique(wl_hist$report_date)
#> [1] "2024-01-31" "2024-02-29"
```

### Using format_histogram()

The
[`format_histogram()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/format_histogram.md)
function ensures your data has the correct structure:

``` r

# If you only have arrival_since, format_histogram adds arrival_before
hist_simple <- data.frame(
  arrival_since = as.Date(c("2024-01-24", "2024-01-17", "2024-01-10")),
  n = c(45, 38, 52),
  report_date = as.Date("2024-01-31")
)

hist_formatted <- format_histogram(hist_simple, end_date = "2024-01-31")
print(hist_formatted)
#>   arrival_since arrival_before  n report_date
#> 1    2024-01-24     2024-01-30 45  2024-01-31
#> 2    2024-01-17     2024-01-23 38  2024-01-31
#> 3    2024-01-10     2024-01-16 52  2024-01-31
```

## Core Analysis Functions

### Queue Size

Calculate the total number of patients waiting:

``` r

# Queue size at end of January
queue_jan <- wl_queue_size_hist(wl_hist_jan)
cat("Queue size (31 Jan):", queue_jan, "patients\n")
#> Queue size (31 Jan): 196 patients

# Queue size at end of February
queue_feb <- wl_queue_size_hist(wl_hist_feb)
cat("Queue size (29 Feb):", queue_feb, "patients\n")
#> Queue size (29 Feb): 180 patients
```

### Referral Statistics (Demand)

Calculate demand statistics from the most recent snapshot:

``` r

referral_stats <- wl_referral_stats_hist(wl_hist_feb)
print(referral_stats)
#>   demand_weekly demand_daily demand_cov demand_count
#> 1            42            6          1           42
```

This shows: - `demand_weekly`: Average weekly referrals -
`demand_daily`: Average daily referrals - `demand_cov`: Coefficient of
variation (uncertainty in arrival pattern) - `demand_count`: Total
referrals over the period

### Removal Statistics (Capacity)

Calculate removal/capacity statistics by comparing multiple snapshots:

``` r

# Uses both Jan and Feb snapshots to estimate removals
removal_stats <- wl_removal_stats_hist(wl_hist)
print(removal_stats)
#>   capacity_weekly capacity_daily capacity_cov removal_count
#> 1        47.31034       6.758621            1           196
```

This shows: - `capacity_weekly`: Average weekly removals (treatments) -
`capacity_daily`: Average daily removals - `capacity_cov`: Coefficient
of variation - `removal_count`: Total removals between snapshots

The function compares consecutive snapshots to detect where counts
decreased (indicating removals).

### Mean Waiting Time

Calculate average waiting time from a snapshot:

``` r

mean_wait <- wl_mean_wait_age_hist(wl_hist_feb)
cat("Mean waiting time:", round(mean_wait, 1), "weeks\n")
#> Mean waiting time: 17 weeks
```

### Percentile Waiting Times

Find specific percentiles of the waiting time distribution:

``` r

# Median (50th percentile)
p50 <- wl_percentile_hist(wl_hist_feb, percentage = 50)
cat("Median wait:", round(p50$weeks, 1), "weeks\n")
#> Median wait: 3 weeks

# 95th percentile
p95 <- wl_percentile_hist(wl_hist_feb, percentage = 95)
cat("95th percentile wait:", round(p95$weeks, 1), "weeks\n")
#> 95th percentile wait: 3 weeks
```

## Comprehensive Statistics with wl_stats_hist()

The
[`wl_stats_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_stats_hist.md)
function provides a complete analysis in one call:

``` r

# Calculate all key statistics
stats <- wl_stats_hist(wl_hist, target_wait = 18)
print(stats)
#>   mean_demand mean_capacity      load load_too_big count_demand queue_size
#> 1          42      47.31034 0.8877551        FALSE           42        180
#>   target_queue_size queue_too_big mean_wait_age mean_wait cv_arrival cv_removal
#> 1               300         FALSE      17.01111  17.01111          1          1
#>   target_capacity relief_capacity pressure
#> 1        42.22222              NA 1.890123
```

This returns a data frame with:

- **Demand/Capacity metrics**: Mean demand, mean capacity, load
  (demand/capacity ratio)
- **Queue metrics**: Current size, target size, whether queue is too big
- **Wait time metrics**: Mean wait, pressure indicator
- **Planning metrics**: Target capacity, relief capacity (if needed)
- **Variability**: Coefficients of variation for arrivals and removals

#### Understanding the Results

``` r

cat("Current queue:", stats$queue_size, "patients\n")
#> Current queue: 180 patients
cat("Target queue:", round(stats$target_queue_size), "patients\n")
#> Target queue: 300 patients
cat("Queue too big?", stats$queue_too_big, "\n\n")
#> Queue too big? FALSE

cat("Mean demand:", round(stats$mean_demand, 1), "patients/week\n")
#> Mean demand: 42 patients/week
cat("Mean capacity:", round(stats$mean_capacity, 1), "patients/week\n")
#> Mean capacity: 47.3 patients/week
cat("Load (demand/capacity):", round(stats$load, 2), "\n")
#> Load (demand/capacity): 0.89
cat("Load too big?", stats$load_too_big, "\n\n")
#> Load too big? FALSE

cat("Mean waiting time:", round(stats$mean_wait_age, 1), "weeks\n")
#> Mean waiting time: 17 weeks
cat("Target wait:", 18, "weeks\n")
#> Target wait: 18 weeks
cat("Pressure (2 × mean / target):", round(stats$pressure, 2), "\n")
#> Pressure (2 × mean / target): 1.89
```

**Key indicators:**

- `load >= 1.0`: System is unstable (demand exceeds capacity)
- `queue_too_big = TRUE`: Queue exceeds twice the target size
- `pressure > 1.0`: Unlikely to meet waiting time targets

## Working with Multiple Snapshots

### Time Series Analysis

``` r

# Create a longer time series (3 months)
wl_hist_mar <- data.frame(
  arrival_since = as.Date(c("2024-03-20", "2024-03-13", "2024-03-06", "2024-02-28")),
  arrival_before = as.Date(c("2024-03-26", "2024-03-19", "2024-03-12", "2024-03-05")),
  n = c(48, 41, 50, 58),
  report_date = as.Date("2024-03-31")
)

wl_hist_full <- rbind(wl_hist_jan, wl_hist_feb, wl_hist_mar)

# Calculate stats using all three time points
stats_full <- wl_stats_hist(wl_hist_full, target_wait = 18)

# Removal stats averaged across Jan→Feb and Feb→Mar
removal_stats_full <- wl_removal_stats_hist(wl_hist_full)
print(removal_stats_full)
#>   capacity_weekly capacity_daily capacity_cov removal_count
#> 1        43.86667       6.266667            1           376
```

### Filtering Date Ranges

Specify which time period to analyze:

``` r

# Only analyze Feb→Mar period
stats_feb_mar <- wl_stats_hist(
  wl_hist_full,
  target_wait = 18,
  start_date = "2024-02-01",
  end_date = "2024-03-31"
)

cat("Capacity (Feb-Mar only):", round(stats_feb_mar$mean_capacity, 1), "\n")
#> Capacity (Feb-Mar only): 40.6

# Compare to full period
cat("Capacity (Jan-Mar):", round(stats_full$mean_capacity, 1), "\n")
#> Capacity (Jan-Mar): 43.9
```

## Working with Categorical Variables

### Multiple Specialties

``` r

# Histogram with multiple specialties
wl_hist_specialty <- data.frame(
  specialty = rep(c("Cardiology", "Orthopedics"), each = 3),
  arrival_since = rep(as.Date(c("2024-01-24", "2024-01-17", "2024-01-10")), 2),
  arrival_before = rep(as.Date(c("2024-01-30", "2024-01-23", "2024-01-16")), 2),
  n = c(45, 38, 52, 30, 42, 28),
  report_date = as.Date("2024-01-31")
)

print(wl_hist_specialty)
#>     specialty arrival_since arrival_before  n report_date
#> 1  Cardiology    2024-01-24     2024-01-30 45  2024-01-31
#> 2  Cardiology    2024-01-17     2024-01-23 38  2024-01-31
#> 3  Cardiology    2024-01-10     2024-01-16 52  2024-01-31
#> 4 Orthopedics    2024-01-24     2024-01-30 30  2024-01-31
#> 5 Orthopedics    2024-01-17     2024-01-23 42  2024-01-31
#> 6 Orthopedics    2024-01-10     2024-01-16 28  2024-01-31
```

### Aggregating Across Categories

Remove categorical splits to get overall statistics:

``` r

# Aggregate across specialties
wl_hist_total <- aggregate_histogram(wl_hist_specialty)
print(wl_hist_total)
#> # A tibble: 3 × 3
#>   arrival_since arrival_before     n
#>   <date>        <date>         <dbl>
#> 1 2024-01-10    2024-01-16        80
#> 2 2024-01-17    2024-01-23        80
#> 3 2024-01-24    2024-01-30        75

# Calculate overall queue size
total_queue <- wl_queue_size_hist(wl_hist_total)
cat("Total queue (all specialties):", total_queue, "\n")
#> Total queue (all specialties): 235
```

### Keeping Categories

Analyze by specialty:

``` r

# Keep specialty grouping
wl_hist_by_specialty <- aggregate_histogram(
  wl_hist_specialty,
  group_columns = "specialty"
)

print(wl_hist_by_specialty)
#> # A tibble: 6 × 4
#>   arrival_since arrival_before specialty       n
#>   <date>        <date>         <chr>       <dbl>
#> 1 2024-01-10    2024-01-16     Cardiology     52
#> 2 2024-01-10    2024-01-16     Orthopedics    28
#> 3 2024-01-17    2024-01-23     Cardiology     38
#> 4 2024-01-17    2024-01-23     Orthopedics    42
#> 5 2024-01-24    2024-01-30     Cardiology     45
#> 6 2024-01-24    2024-01-30     Orthopedics    30
```

## Practical Example: NHS National Data Pattern

This example shows a typical workflow for NHS published statistics:

``` r

# Simulate NHS RTT data structure (weekly bins)
create_nhs_snapshot <- function(report_date, base_count = 100) {
  # NHS typically reports in 1-week bins up to 52+ weeks
  weeks <- c(1, 2, 3, 4, 6, 8, 10, 12, 15, 18, 21, 26, 52)

  data.frame(
    arrival_since = as.Date(report_date) - (weeks * 7 + 6),
    arrival_before = as.Date(report_date) - (weeks * 7),
    n = round(base_count * exp(-weeks / 20)), # Exponential decay
    report_date = as.Date(report_date)
  )
}

# Create quarterly snapshots
nhs_q4_2023 <- create_nhs_snapshot("2023-12-31", base_count = 120)
nhs_q1_2024 <- create_nhs_snapshot("2024-03-31", base_count = 110)
nhs_q2_2024 <- create_nhs_snapshot("2024-06-30", base_count = 105)

nhs_hist <- rbind(nhs_q4_2023, nhs_q1_2024, nhs_q2_2024)

# Analyze NHS data
nhs_stats <- wl_stats_hist(nhs_hist, target_wait = 18)

cat("NHS Example Analysis\n")
#> NHS Example Analysis
cat("====================\n")
#> ====================
cat("Queue size (Jun 2024):", nhs_stats$queue_size, "\n")
#> Queue size (Jun 2024): 808
cat("Target queue size:", round(nhs_stats$target_queue_size), "\n")
#> Target queue size: 714
cat("Mean waiting time:", round(nhs_stats$mean_wait_age, 1), "weeks\n")
#> Mean waiting time: 61.9 weeks
cat("Meeting 18-week target?", nhs_stats$pressure <= 1.0, "\n")
#> Meeting 18-week target? FALSE
```

### Trade-offs

**Patient-level approach:** - ✓ More precise calculations - ✓ Can track
individual patient journeys - ✗ Requires detailed data - ✗ Privacy
concerns - ✗ Slower with large datasets

**Histogram approach:** - ✓ Works with aggregate published data - ✓
Privacy-preserving - ✓ Efficient for large datasets - ✗ Less precision
(binned data) - ✗ Cannot track individuals - ✗ Requires multiple
snapshots for removals

## Summary

### Key Functions

| Function | Purpose | Input |
|----|----|----|
| [`format_histogram()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/format_histogram.md) | Prepare/validate data | Histogram with arrival_since |
| [`aggregate_histogram()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/aggregate_histogram.md) | Collapse categories | Histogram with categorical variables |
| [`wl_queue_size_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_queue_size_hist.md) | Current queue size | Single snapshot |
| [`wl_referral_stats_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_referral_stats_hist.md) | Demand statistics | Single snapshot |
| [`wl_removal_stats_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_removal_stats_hist.md) | Capacity statistics | Multiple snapshots |
| [`wl_mean_wait_age_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_mean_wait_age_hist.md) | Mean waiting time | Single snapshot |
| [`wl_percentile_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_percentile_hist.md) | Percentile waits | Single snapshot |
| [`wl_stats_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_stats_hist.md) | Complete analysis | Multiple snapshots |

### Best Practices

1.  **Always include report_date**: Essential for multi-snapshot
    analysis
2.  **Use consistent time bins**: Weekly or monthly bins work best
3.  **Multiple snapshots for accuracy**: More snapshots = better
    capacity estimates
4.  **Validate your data**: Use
    [`format_histogram()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/format_histogram.md)
    to ensure structure
5.  **Check for missing dates**: Ensure consecutive snapshots for
    reliable trends

### Next Steps

- Follow the [histogram
  walkthrough](https://nhs-r-community.github.io/NHSRwaitinglist/articles/histogram_walkthrough.md)
  for a worked end-to-end example
- Explore the [simulation
  vignette](https://nhs-r-community.github.io/NHSRwaitinglist/articles/waiting_list_sim.md)
  for forward projections
- See [example
  walkthrough](https://nhs-r-community.github.io/NHSRwaitinglist/articles/example_walkthrough.md)
  for patient-level analysis
- Read about [queuing theory
  foundations](https://nhs-r-community.github.io/NHSRwaitinglist/articles/three_example_waiting_lists.md)

## References

This package implements methods described in:

Fong et al. (2022). “Queueing Theory and Machine Learning Applied to
National Scale Medical Waiting Lists.” *medRxiv*.
<doi:10.1101/2022.08.23.22279117>
