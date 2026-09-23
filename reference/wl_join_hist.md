# Join Two Waiting Lists in Histogram Format

Combines two waiting lists provided in histogram format into a single
histogram. Optionally, preserves common category columns between the two
dataframes.

## Usage

``` r
wl_join_hist(wl_hist_1, wl_hist_2, categories = FALSE)
```

## Arguments

- wl_hist_1:

  data.frame. A waiting list in histogram format.

- wl_hist_2:

  data.frame. A waiting list in histogram format.

- categories:

  logical. If TRUE, retains only the common category columns when
  aggregating. Default is FALSE.

## Value

data.frame. A combined waiting list in histogram format.

## Details

This function ensures both input dataframes are in the correct histogram
format, aggregates them (optionally by common categories), and returns
the combined result in histogram format.

## See also

[`format_histogram`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/format_histogram.md),
[`aggregate_histogram`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/aggregate_histogram.md)
