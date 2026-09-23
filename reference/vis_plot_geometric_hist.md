# Plot a Waiting-list Histogram Against a Fitted Geometric Distribution

Plot a Waiting-list Histogram Against a Fitted Geometric Distribution

## Usage

``` r
vis_plot_geometric_hist(
  fit,
  title = "Waiting List Histogram",
  subtitle = "Observed counts vs fitted geometric distribution",
  caption = NULL
)
```

## Arguments

- fit:

  A result from
  [`wl_fit_geometric_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_fit_geometric_hist.md)
  or a histogram data frame that can be passed to
  [`wl_fit_geometric_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_fit_geometric_hist.md).

- title:

  Plot title.

- subtitle:

  Plot subtitle.

- caption:

  Plot caption.

## Value

A ggplot object.
