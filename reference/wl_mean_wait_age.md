# Calculate Mean Age of Waiting for Patients on Waiting List

This function calculates the time waiting for patients who are still on
the waiting list as of a specified end date. Recall that the age of
waiting is the mean time waited so far. (Opposed to the mean sojourn
time, which is the time waiting until removal from the list.)

## Usage

``` r
wl_mean_wait_age(waiting_list, referral_index, removal_index, end_date)
```

## Arguments

- waiting_list:

  A data frame containing patient waiting list data.

- referral_index:

  The column name or index for the referral date in `waiting_list`.

- removal_index:

  The column name or index for the removal date in `waiting_list`.

- end_date:

  The date (as Date or numeric) up to which to calculate ages.

## Value

The mean age (numeric) for patients still waiting as of `end_date`.
