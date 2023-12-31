# HMWK_03

**Creating and naming variables**  
1. Create a variable called x and use it to store the result of the
calculation (3\*(4+2).  
2. Then multiply the product by pi value.

------------------------------------------------------------------------

``` r
(3*(4+2))  
```

    [1] 18

------------------------------------------------------------------------

------------------------------------------------------------------------

``` r
x <- 18  
```

------------------------------------------------------------------------

``` r
x* pi
```

    [1] 56.54867

3\. Use the getwd() function to show your current working directory. Is
that a good working directory, and what program do you think set it that
way?

``` r
getwd() 
```

    [1] "C:/Users/owner/OneDrive - University of Tennessee/Documents/MICR575 repro data/Project.folder"

*This is a good working directory because it is in a specific file
location that is easily recognizable to me, and the file location name
corresponds to the class I am completing this homework assignment for.*

*The program that set the working directory this way would be RStudio.*

------------------------------------------------------------------------

**Vectors**  
1. Use the c() function to create a vector of numbers.

------------------------------------------------------------------------

``` r
plant_height <- c(6.0, 8.0, 10.0)  
```

------------------------------------------------------------------------

2.  Use the c() function to create a vector of characters.

------------------------------------------------------------------------

``` r
plant_name <- c("kale", "cilantro", "broccoli")  
```

------------------------------------------------------------------------

3.  Use the : implicit function to create a vector of integers 1 to 10.

------------------------------------------------------------------------

``` r
plant_index <- 1:10  
```

------------------------------------------------------------------------

4.  Explain why the following code returns what it does. Also address
    whether you think this was a good decision on the part of the
    designers of R?

------------------------------------------------------------------------

``` r, message = FALSE
v1 <- 1:3  
v2 <- c(1:4)  
v1 + v2  
```

    Warning in v1 + v2: longer object length is not a multiple of shorter object
    length

    [1] 2 4 6 5

------------------------------------------------------------------------

*The response saying longer object length is not a multiple of shorter
object length occurs when the vector lengths added are not the same.
This creates a lot of inconsistency here and room for error. It also
means that each has a different number of elements.*

*Vector recycling is okay if your vectors are the same length because
it’s designed to perform the operation according the positions of values
within each vector. But if your vectors aren’t the same length, it can
lead to errors in your data because it will circle back to the first
value in the shortest vector (which may not be originally intended by
the user).*

5.  Explain the following code and what it does c(1, 5, 9) +3

------------------------------------------------------------------------

``` r
c(1, 5, 9) + 3  
```

    [1]  4  8 12

------------------------------------------------------------------------

*This code takes each individual number in parentheses and produces 3
corresponding outcomes where each individual number, (1, 5, and 9) is
added with 3 to produce appropriate corresponding outcomes (4, 8, 12).
This is probably a better option in adding numbers compared to adding v1
and v2 in the previous question. Although, the behaviors in the previous
question and this one have the same goal, using the c function provides
a clearer, more concise answer.*

6.  Remove (delete) every variable in your workspace

------------------------------------------------------------------------

``` r
ls()  
```

    [1] "has_annotations" "plant_height"    "plant_index"     "plant_name"     
    [5] "v1"              "v2"              "x"              

``` r
rm(list = ls())  
```

------------------------------------------------------------------------

**Graphics**

1.  Load the tidyverse package.

------------------------------------------------------------------------

``` r
library(tidyverse)
```

------------------------------------------------------------------------

2.  Recreate the visualization of body_mass_g to flipper_length_mm, from
    the penguins data set, that is shown in question 8 of section 2.2.5
    of R4DS.

------------------------------------------------------------------------

``` r
library(ggthemes, ggplot)
  install.packages("palmerpenguins", repos = "http://cran.us.r-project.org") 
```

    package 'palmerpenguins' successfully unpacked and MD5 sums checked

    The downloaded binary packages are in
        C:\Users\owner\AppData\Local\Temp\RtmpcbKFKV\downloaded_packages

``` r
library(palmerpenguins)  
```

    # A tibble: 344 × 8
       species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
       <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
     1 Adelie  Torgersen           39.1          18.7               181        3750
     2 Adelie  Torgersen           39.5          17.4               186        3800
     3 Adelie  Torgersen           40.3          18                 195        3250
     4 Adelie  Torgersen           NA            NA                  NA          NA
     5 Adelie  Torgersen           36.7          19.3               193        3450
     6 Adelie  Torgersen           39.3          20.6               190        3650
     7 Adelie  Torgersen           38.9          17.8               181        3625
     8 Adelie  Torgersen           39.2          19.6               195        4675
     9 Adelie  Torgersen           34.1          18.1               193        3475
    10 Adelie  Torgersen           42            20.2               190        4250
    # ℹ 334 more rows
    # ℹ 2 more variables: sex <fct>, year <int>

``` r
view(penguins) 
ggplot(data = penguins,
  mapping = aes(x = flipper_length_mm, y = body_mass_g)) + 
  geom_point(aes(color = bill_depth_mm)) + 
  geom_smooth(method = "lm") 
```

------------------------------------------------------------------------
![](hmk_03_plot.png)

3.  Explain why each aesthetic is mapped at the level that it is (i.e.,
    at the global level, in the ggplot() function call, or at the geom
    level, in the geom_XXX() function call). Noting that a lot of
    different options will work, but some options are clearly better
    than others.

*We map body mass and flipper length globally because it tells you what
you’re comparing, i.e. the “plot objects”. Then, we can add layers to
add in the actual datapoints, and use the aes argument within geom_point
to further different the data visually. (i.e., using color and shape to
specify species type)*

------------------------------------------------------------------------
 

