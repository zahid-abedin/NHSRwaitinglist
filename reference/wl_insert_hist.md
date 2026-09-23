# Insert some patients into a waiting list in histogram format

Acting on a histogram this is identical to
[`wl_join_hist()`](https://nhs-r-community.github.io/NHSRwaitinglist/reference/wl_join_hist.md).

## Usage

``` r
wl_insert_hist(waiting_list, additions)
```

## Arguments

- waiting_list:

  dataframe. A waiting list in histogram format

- additions:

  dataframe. A dataframe of waiting list additions in histogram format

## Value

data.frame. A combined waiting list.
