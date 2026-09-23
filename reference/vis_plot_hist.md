# Plot Actual vs Geometric Target Waiting List Histogram

This function plots the actual waiting list histogram against a
geometric target distribution.

## Usage

``` r
vis_plot_hist(wl_hist, target = 18, percentage = 92)
```

## Arguments

- wl_hist:

  Data frame containing waiting list history in histogram format.

- target:

  Numeric. Target number of weeks for the geometric distribution (e.g.,
  18).

- percentage:

  Numeric. Percentile to highlight (e.g., 92).

## Value

A ggplot object showing the actual and target distributions.
