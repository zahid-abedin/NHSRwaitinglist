# Calculate the date and waiting time corresponding to a given percentile in waiting list history

Calculate the date and waiting time corresponding to a given percentile
in waiting list history

## Usage

``` r
wl_percentile_hist(wl_hist, percentage = 92)
```

## Arguments

- wl_hist:

  A data frame containing waiting list history with columns: `n` (number
  of patients), `arrival_since`, `arrival_before`, and `report_date`.

- percentage:

  Numeric value indicating the percentile to calculate. Must be between
  0 and 100.

## Value

A list with the percentile date and weeks to percentile. The list
includes both the preferred names `date_percentile` and
`weeks_percentile`, and the legacy aliases `date` and `weeks` for
backward compatibility.

## Examples

``` r
# get_percentile_date(wl_hist, percentage = 92)
```
