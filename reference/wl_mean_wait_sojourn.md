# Calculate Mean Sojourn Time Between Referral and Removal

This function calculates the mean sojourn time (number of days between
referral and removal dates) in a waiting list data frame. It ignores
rows with missing referral or removal values.

## Usage

``` r
wl_mean_wait_sojourn(waiting_list)
```

## Arguments

- waiting_list:

  A data frame containing referral and removal date columns.

## Value

A numeric value representing the mean sojourn time in days. Returns
`NA_real_` if there are no complete referral/removal pairs.

## Examples

``` r
waiting_list <- data.frame(
  referral = as.Date(c("2024-01-01", "2024-01-08")),
  removal = as.Date(c("2024-01-05", NA))
)
wl_mean_wait_sojourn(waiting_list)
#> [1] 4
```
