# Calculate Mean Waiting Age from Waiting List History

Calculates the mean waiting age for a histogram snapshot: the average
time waited so far by patients who are still on the waiting list at the
report date.

## Usage

``` r
wl_mean_wait_age_hist(wl_hist)
```

## Arguments

- wl_hist:

  A data frame containing waiting list history with columns:

  arrival_since

  :   Date, start of the arrival interval

  arrival_before

  :   Date, end of the arrival interval

  report_date

  :   Date, snapshot date of the histogram

  n

  :   Numeric, count of cases

## Value

Numeric value of the mean waiting age in days.
