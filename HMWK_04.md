# Hmk_04 template: Data frames and data wrangling

**Load packages**

``` r
library(tidyverse)
library(nycflights13)
```

# Question 1: filtering

Make a plot of air time as a function of distance (air time on the y
axis, distance on the x axis) for all flights that meet the following
criteria:

- originate from LaGuardia airport (“LGA”)
- departed on the 16th of the month
- have a flight distance of less than 2000

``` r
flights|>
  filter(origin == "LGA",
         day == "16",
         distance <2000)|>
  ggplot(mapping = aes(x = distance, y = air_time)) + 
  geom_point(na.rm = TRUE) + 
  labs(title = "Organized Flights",
       x = "Distance",
       y = "Air Time")
```



![](HMWK_04_files/figure-commonmark/unnamed-chunk-2-1.png)
# Question 2: dealing with NAs

Make a data frame of all of the rows of `flights` that have values for
*both* `arr_time` and `dep_time` - that is, neither of those values are
`NA`.

``` r
filtered.flights <- flights |>
  filter(!is.na(arr_time),
         !is.na(dep_time))
```

## filtering NAs

`ggplot()` will automatically remove NA values from the plot, as you may
have seen in question 1, but it emits a warning message about that. Of
course you could silence the warning message using [chunk
options](https://bookdown.org/yihui/rmarkdown-cookbook/chunk-options.html),
but how could you prevent them from appearing in the first place?

*We can mitigate NA responses by using the function ‘(na.rm =TRUE)’
within a mapping function such as ‘geom_point’, etc.*

# Question 3: adding columns

Create a data frame of average flight speeds, based on `air_time` and
`distance`. Make either a histogram or a density plot of the data. If
you like, you may break the data out (e.g. by airline, or some other
variable) in a way that you think makes sense.

``` r
avg.speeds <- flights |>
  mutate(speed = distance/(air_time/60)) |>
  group_by(carrier) |>
  summarise(avg.speeds = mean(speed, na.rm =TRUE))

avg.speeds |>
  ggplot(mapping = aes(x =carrier,
                       y = avg.speeds, 
                       fill = carrier)) + 
  geom_col(na.rm = TRUE) + 
  labs(title = "Average Speed by Airline",
       x = "Airline",
       y = "Average Flight Speed (mph)")
```

![](HMWK_04_files/figure-commonmark/unnamed-chunk-4-1.png)
