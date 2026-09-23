# Changelog

## NHSRwaitinglist 0.1.3

### New Features

- **Histogram functionality now documented and exported**: The package
  has long included functions for analysing aggregated/binned waiting
  list data (histogram format), but these were previously internal-only.
  These functions are now fully documented and exported:

  - [`wl_stats_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_stats_hist.md) -
    Comprehensive waiting list statistics from histogram data
  - [`wl_referral_stats_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_referral_stats_hist.md) -
    Demand/referral statistics
  - [`wl_removal_stats_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_removal_stats_hist.md) -
    Capacity/removal statistics
  - [`wl_queue_size_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_queue_size_hist.md) -
    Queue size calculation
  - [`wl_mean_wait_age_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_mean_wait_age_hist.md) -
    Mean waiting time
  - [`wl_percentile_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_percentile_hist.md) -
    Percentile calculations
  - [`wl_join_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_join_hist.md)
    and
    [`wl_insert_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_insert_hist.md) -
    Histogram manipulation
  - [`format_histogram()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/format_histogram.md)
    and
    [`aggregate_histogram()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/aggregate_histogram.md) -
    Data preparation utilities

- **Improved histogram API**:
  [`wl_stats_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_stats_hist.md)
  and
  [`wl_removal_stats_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_removal_stats_hist.md)
  now accept a single histogram dataframe with multiple `report_date`
  values, rather than requiring two separate histogram parameters. This
  significantly improves usability and allows analysis across multiple
  time points.

- **New vignette**: Added comprehensive [histogram analysis
  vignette](https://nhs-r-community.github.io/NHSRwaitinglist/articles/histogram_analysis.html)
  demonstrating:

  - When and why to use histogram format
  - Data structure requirements
  - All histogram analysis functions
  - Working with NHS national statistics
  - Comparison with patient-level analysis

- Added a plotting function
  [`plot_rtt_journey()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/plot_rtt_journey.md)
  and quarto report generating function
  [`rep_rtt_improvement()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/rep_rtt_improvement.md)
  to conveniently generate plots to help tell RTT improvement stories
  within acute trusts.

### Bug Fixes

- Fixed hardcoded `target_wait` value in
  [`wl_stats_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_stats_hist.md)
  (was always 18, now uses function parameter)
- Removed duplicate file `wl_referral_stats_hist-E-LOSX4GYQ6L4.R`

## NHSRwaitinglist 0.1.2

CRAN release: 2025-07-15

- Bug fix release, required for changes in date handling in R.
- More input checks added
- CRAN installation instructions added.

## NHSRwaitinglist 0.1.1

CRAN release: 2025-04-29

- Bug fixes and formatting updated in some help files
- Added an additional vignette on using WL simulation to answer
  questions around managing waiting lists
- Arguments harmonised across several functions, and column
  specifications given more details. Thanks
  [@davidfoord1](https://github.com/davidfoord1)
- Better column indexing in wl_insert and others. Thanks
  [@davidfoord1](https://github.com/davidfoord1)
- More details added to DESCRIPTION

## NHSRwaitinglist 0.1

- Initial CRAN submission.
