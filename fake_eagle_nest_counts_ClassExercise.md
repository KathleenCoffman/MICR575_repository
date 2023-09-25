# Eagles data exercise

## Tidy data class exercise using Eagles data

- Use `read_csv()` to read the data into a data frame.

``` r
getwd()
```

    [1] "C:/Users/owner/OneDrive - University of Tennessee/Documents/MICR575 repro data/Project.folder"

``` r
library(ggplot2, tidyr)
eagles_data <- read.csv("fake_eagle_nest_counts.csv")
```

- Would you describe this as a “tidy” data frame? Why or why not?

  - This data frame isn’t too terribly untidy, but tidiness can be
    enhanced by using the pivot_longer function.

- If not, “pivot” it to be a tidy data frame. Remember to use the
  names_to and values_to arguments to generate a new data frame that has
  logical column names.

  ``` r
  library(tidyr)
  tidy_eagles_data <- eagles_data |>
    pivot_longer(cols = -year, 
                 names_to = "region", 
                 values_to = "quantity")

  # Rename the columns
  colnames(tidy_eagles_data) <- c("Year", "Region", "Nests")

  # Print data frame
  print(tidy_eagles_data)
  ```

      # A tibble: 54 × 3
          Year Region    Nests
         <int> <chr>     <int>
       1  2006 pacific    1636
       2  2006 southwest    68
       3  2006 rocky.mtn   241
       4  2007 pacific    2474
       5  2007 southwest   148
       6  2007 rocky.mtn   277
       7  2008 pacific    3772
       8  2008 southwest   131
       9  2008 rocky.mtn   384
      10  2009 pacific    3179
      # ℹ 44 more rows

- Make a scatter plot of the resulting data, showing the number of
  eagles’ nests in each region by year. Add a linear trend line using
  `geom_smooth(method="lm")`. 

``` r
library(ggplot2)
custom_colors <- c("pacific" = "purple", "southwest" = "pink", "rocky.mtn" = "magenta")
ggplot(tidy_eagles_data, aes(x = Year, y = Nests, color = Region)) +
  geom_point() +
  geom_smooth(method = "lm") +
  labs(x = "Year", y = "Number of Eagle Nests", title = "Eagle Nests by Region Over Time") + 
  scale_color_manual(values = custom_colors)
```

    `geom_smooth()` using formula = 'y ~ x'

![](fake_eagle_nest_counts_ClassExercise_files/figure-commonmark/unnamed-chunk-3-1.png)

``` r
                       ggsave(filename = "eagles_plot.png", device = "png")
```

    Saving 7 x 5 in image
    `geom_smooth()` using formula = 'y ~ x'
