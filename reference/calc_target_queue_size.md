# Target Queue Size

Uses Little's Law to calculate the target queue size to achieve a target
waiting time as a function of observed demand, target wait and a
variability factor used in the target mean waiting time calculation.

Target Queue Size = Demand \* Target Mean Wait, where Target Mean Wait =
Target Wait / factor.

The average wait should sit somewhere between `target_wait/6` and
`target_wait/4` depending on the chosen factor. The factor defaults to
4.

Only applicable when Capacity \> Demand.

## Usage

``` r
calc_target_queue_size(demand, target_wait, factor = 4)
```

## Arguments

- demand:

  Numeric value of rate of demand in same units as target wait e.g. if
  target wait is weeks, then demand in units of patients/week.

- target_wait:

  Numeric value of number of weeks that has been set as the target
  within which the patient should be seen.

- factor:

  Numeric factor used in average wait calculation

  - to get a quarter of the target use factor=4

  - to get one sixth of the target use factor = 6 etc. Defaults to 4.

## Value

Numeric target queue length.

## Examples

``` r
# If demand is 30 patients per week and the target wait is 52 weeks, then the
# for a quarter-target mean wait, target queue size = 30 * 52/4 = 390.

calc_target_queue_size(30, 52, 4)
#> [1] 390
```
