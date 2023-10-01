# HMWK_05

# Data import

## Q1:

- I created a directory, within my main class directory, called `data`.
- Once I downloaded the example file from Ch 9 and saved it inside the
  `data` directory, I used `read_csv()` to read the file to an R data
  frame and ensure the following:
  - Column names are *syntactic*, (without spaces).
  - N/A values are represented with the R value `NA`, not the character
    “N/A”.
  - Data types (character vs factor vs numeric) are appropriate.

``` r
library(tidyverse, janitor)

students <- read_csv("data/students.csv", 
                     na = c("N/A", ""))
glimpse(students)
```

    Rows: 6
    Columns: 5
    $ `Student ID`   <dbl> 1, 2, 3, 4, 5, 6
    $ `Full Name`    <chr> "Sunil Huffmann", "Barclay Lynn", "Jayendra Lyne", "Leo…
    $ favourite.food <chr> "Strawberry yoghurt", "French fries", NA, "Anchovies", …
    $ mealPlan       <chr> "Lunch only", "Lunch only", "Breakfast and lunch", "Lun…
    $ AGE            <chr> "4", "5", "7", NA, "five", "6"

``` r
students |>
  janitor::clean_names() |>
  mutate(
    meal_plan = factor(meal_plan),
    age = parse_number(if_else(age == "five", "5", age)))
```

    # A tibble: 6 × 5
      student_id full_name        favourite_food     meal_plan             age
           <dbl> <chr>            <chr>              <fct>               <dbl>
    1          1 Sunil Huffmann   Strawberry yoghurt Lunch only              4
    2          2 Barclay Lynn     French fries       Lunch only              5
    3          3 Jayendra Lyne    <NA>               Breakfast and lunch     7
    4          4 Leon Rossini     Anchovies          Lunch only             NA
    5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch     5
    6          6 Güvenç Attila    Ice cream          Lunch only              6

Above, I used ’na = c(“N/A”, ” “)’ to make sure all the N/A values were
interpreted the same way. Then, I used glimpse to show me what the data
looks like. Here, I noticed that the column names needed to be
reformatted, and that there were inconsistent descriptions used in the
age column. So I knew that I needed to remove the spaces in the column
names and make sure all age values were numerical values (1, 2, 3, etc).
I used janitor (clean_names) to remove all spaces within column names,
made meal_plan a factor because it is categorical, and then used age =
parse_number (if_else) etc. to change”five” to “5” in the age column.

## Q2:

Find (or make) a data file of your own, in text format. Read it into a
well-formatted data frame.

``` r
library(tidyverse, janitor)
mushrooms <- read_csv("data/mushrooms.csv")
mushrooms |>
  janitor::clean_names()|>
  mutate(
    mushroom_species = factor(mushroom_species))
```

    # A tibble: 5 × 4
      mushroom_id mushroom_species       mushroom_cap_color mushroom_height_cm
            <dbl> <fct>                  <chr>                           <dbl>
    1           1 Amanita muscaria       red                                30
    2           2 Lactarius indigo       blue                                8
    3           3 Calvatia gigantea      white                              70
    4           4 Laccaria amethystina   purple                             10
    5           5 Hypomyces lactifluorum orange                             20

``` r
mushrooms |>
  ggplot(mapping = aes(x = `Mushroom species`,
                       y = `Mushroom Height cm`, 
                       fill = `Mushroom species`)) + 
  geom_col(na.rm = TRUE) + 
  labs(title = "Mushroom Species Height and Cap Color",
       x = "Species",
       y = "Height (cm)") +
  scale_fill_manual(values = c("Amanita muscaria" = "red",
                               "Calvatia gigantea" = "white", 
                               "Hypomyces lactifluorum" = "orange",
                               "Laccaria amethystina" = "purple",
                               "Lactarius indigo" = "blue")) + 
  scale_x_discrete(labels = NULL)
```

![](HMWK_05_files/figure-commonmark/unnamed-chunk-2-1.png)

``` r
        ggsave(filename = "mushrooms.png", device = "png")
```

Here above, you can a small dataset I created, detailing mushroom
species name, color, and height. There were no N/A values in this data
so I did not have to do anything concerning them. I did however use
janitor (clean_names) to remove any space within the column names, then
made sure mushroom species was read as a factor because I want it to be
interpreted as categorical.

(I also wanted to practice making plots so I did that as well.)

# Tidying

I downloaded the data set available from
<https://tiny.utk.edu/MICR_575_hmk_05>, and saved it to my `data`
folder.

Why is the file formatted so inconveniently? I have no idea, but I do
know that this is about an average level of inconvenient formatting for
real data sets you will find in the wild.

\_I think this file was formatted this way because it was interpreted
from a survey-style score sheet.\_

## Q3a:

Next, use `read_csv()` to read the data into a data frame. Note that
you’ll need to make use of some of the optional arguments. Use
`?read_csv` to see what they are.

``` r
colloquium <- read_csv("data/colloquium_assessment.csv",
                       col_names = c("Start_Time", "End_Time", "Status", "Progress", "Duration", "Finished", "Recorded_Date", "Response_ID", "Last_Name", "First_Name", "Email", "External_Reference", "Latitude", "Longitude", "Distribution_Channel", "User_Language", "Q4", "Q5", "Q6", "Q7", "Q8", "Q9", "Q10", "Q11", "Judge_Comments"),
                       skip = 5)|>
  filter(!is.na(Start_Time))
```

    Rows: 32 Columns: 25
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr  (7): Start_Time, End_Time, Recorded_Date, Response_ID, Distribution_Cha...
    dbl (14): Status, Progress, Duration, Finished, Latitude, Longitude, Q4, Q5,...
    lgl  (4): Last_Name, First_Name, Email, External_Reference

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
new_colloquium <- colloquium|>
  pivot_longer(cols = starts_with("Q"), 
               names_to = "Question", 
               values_to = "Response") |>
  mutate(Question = parse_number(Question))
View(new_colloquium)
```

Once the data was read into a data frame, I renamed the columns so that
they would be more appropriate to represent the cell values. Then I used
‘skip = 5’ to remove the first 5 rows of each column because these cells
did not contain any meaingful data required for use here. Once I did
this, I used ‘filter’ to remove any rows that did not list a start time
(so I could see the actual students’ responses). I used `pivot_longer()`
to make the data frame longer, in a way that makes sense by condensing
the ‘Q’ columns into one column, and calling this column ‘Question’ and
the values will be considered responses. I realized I needed to remove
the ‘Q’ from each number in the ‘Question’ column in order to be able to
fine the average in the next question so I included that here as well. I
used the ‘view()’ function to see the data again so that I could make
sure it visually made sense to me after modifying it.

## Q3b:

I used ‘new_colloquium’ to specify the data frame I am still wokring
from. I used the filter function and \>= (greater than or equal to) to
specify which of the questions I want to use for the mean. Then I used
the summarise function and the mean argument to calculate the student’s
average score for questions 7-10 only.

``` r
new_colloquium |>
  filter(Question >= 7) |>
  summarise(mean(Response))
```

    # A tibble: 1 × 1
      `mean(Response)`
                 <dbl>
    1             4.53

The mean response for Questions 7 through 10 was “4.525”.
