# Fit a Geometric Distribution to a Waiting-list Histogram

Fits a discrete geometric distribution to histogram counts by weeks
waited. If an `open_ended` column is present, rows marked `TRUE` are
treated as right-censored tail bins.

## Usage

``` r
wl_fit_geometric_hist(wl_hist)
```

## Arguments

- wl_hist:

  A data frame with `n` and either `weeks_waiting` or date columns from
  which weeks waited can be inferred.

## Value

A list with fitted-bin data and a one-row summary table.
