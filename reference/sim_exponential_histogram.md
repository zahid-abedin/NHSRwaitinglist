# Create a simulated histogram in the format used by the package

Create a simulated histogram in the format used by the package

## Usage

``` r
sim_exponential_histogram(
  num_intervals = 52,
  end_date = Sys.Date(),
  rate = 0.1,
  queue_size = 1000,
  time_interval = "weeks",
  random = FALSE
)
```

## Arguments

- num_intervals:

  integer. Number of time intervals to create.

- end_date:

  Date. The date that the waiting list should end.

- rate:

  numeric. The rate defining the exponential.

- queue_size:

  numeric. Total queue size used to scale the histogram.

- time_interval:

  Character or numeric. Controls the spacing between rows.

- random:

  logical. If TRUE, draws interval counts from an exponential process.

## Value

A data.frame of a simulated waiting list in histogram format.
