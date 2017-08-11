Tidyverse
========================================================
author: Blake Seers and Kevin Chang
date: 11 August 2017
autosize: false

What is the tidyverse?
========================================================

The tidyverse (<http://www.tidyverse.org/>) is a collection of **R** packages that are all based on one simple,
underlying philosophy to data science:

## Solve a complex problem by **combining simple pieces** that have a **consisitent structure**.

The collection of tidyverse packages:
========================================================

- share the same data representation and programming styles, i.e. API design. 
- work in harmony

<div align="center">
<img src="tidyversePackage.PNG" width=500>

The collection of tidyverse packages:
========================================================

These can all be installed and loaded easily in **R**:


```r
install.packages("tidyverse")
library(tidyverse)
```



Remember: These packages work together to solve a complex problem by **combining simple pieces** that have a **consisitent structure**.

But how?

Simple pieces...
========================================================

- Functions:
    + do one thing well: return a *value* or have a *side-effect*. 
    + can be understood in isolation with minimal context: *type-stable*
- Always comment your code:
    + why did you write this piece of code --- not how it works.


Side-effect
========================================================

- changing the state of the world.
- Examples: 
      + `<-`
      + `print()`
      + `read.csv`


Simple pieces...
========================================================

- Functions:
    + do one thing well: return a *value* or have a *side-effect*. 
    + can be understood in isolation with minimal context: *type-stable*
- Always comment your code:
    + why did you write this piece of code --- not how it works.


`sapply()` and `[` functions are type-unstable
=======================================================
- `sapply()` can return vector, matrix or list depends on the dimension of the ouputs. 
- `[` can return vector or data frame. 


...Combining...
========================================================

- With *assignment*, *composition* or the *pipe* (`%>%`) operator


Assignment 
==========================================================

```r
a1 <- group_by(flights, year, month, day)
a2 <- select(a1, arr_delay, dep_delay)
a3 <- summarise(a2,
  arr = mean(arr_delay, na.rm = TRUE),
  dep = mean(dep_delay, na.rm = TRUE))
a4 <- filter(a3, arr > 30 | dep > 30)
```

Composition 
==========================================================

```r
filter(
  summarise(
    select(
      group_by(flights, year, month, day),
      arr_delay, dep_delay
    ),
    arr = mean(arr_delay, na.rm = TRUE),
    dep = mean(dep_delay, na.rm = TRUE)
  ),
  arr > 30 | dep > 30
)
```

Piping 
==========================================================

```r
flights %>%
  group_by(year, month, day) %>%
  select(arr_delay, dep_delay) %>%
  summarise(
    arr = mean(arr_delay, na.rm = TRUE),
    dep = mean(dep_delay, na.rm = TRUE)
  ) %>%
  filter(arr > 30 | dep > 30)
```

...Combining...
========================================================
- With assignment, composition or the pipe (`%>%`) operator
- Function fit best into a pipe when 
    + The first argument is the "data".
    + The data is the same type across different functions.
- Pipe operator is not useful when
    + The intermediate values are need to be stored.
    + The intermediate step takes long time to compute.
    + Relationship between functions is not linear. 


...Consistent structure.
========================================================

- Tidy tibbles instead of the conventional `data.frame()`
    + Each dataset goes in a tibble.
    + Each column represents a variable.
    + Each row represents an observation.
- Columns are `list`s that can store richer data structure, i.e. `list-cols`.


Tibble
==========================================================

```r
library(nycflights13)
flights
```

```
# A tibble: 336,776 x 19
    year month   day dep_time
   <int> <int> <int>    <int>
 1  2013     1     1      517
 2  2013     1     1      533
 3  2013     1     1      542
 4  2013     1     1      544
 5  2013     1     1      554
 6  2013     1     1      554
 7  2013     1     1      555
 8  2013     1     1      557
 9  2013     1     1      557
10  2013     1     1      558
# ... with 336,766 more rows, and 15
#   more variables:
#   sched_dep_time <int>,
#   dep_delay <dbl>, arr_time <int>,
#   sched_arr_time <int>,
#   arr_delay <dbl>, carrier <chr>,
#   flight <int>, tailnum <chr>,
#   origin <chr>, dest <chr>,
#   air_time <dbl>, distance <dbl>,
#   hour <dbl>, minute <dbl>,
#   time_hour <dttm>
```


Summary
========================================================
To solve a complex problem by **combining simple pieces** that have a **consisitent structure**.

Four basic principles to a tidy API:

  1.  Reuse existing data structures.
  2.  Compose simple functions with the *pipe*.
  3.  Embrace *functional programming*.
  4.  Design for humans.


Workflow of a typical data science project
=======================================================

<div align="center">
<img src="dataScience.PNG" width=1000>

Data import
=======================================================

- `readr`: for CSV files
- `haven`: for SPSS, SAS and Stata files
- `readxl`: for MS Excel files

Note that character variables in the dataset remain as a character variables. The  `read.csv()` function automatically converts these variables into a factor variable.

Data tidying
=======================================================
- `tidyr`: data tidying 
    + `gather()` function: convert the dataset from wide format to long format.
    + `spread()` function: convert the dataset from long format to wide format.
-  `tibble`: new type of data frames.

Data transformation 
=======================================================
- `dplyr`: data manipulation
- `forcats`: for factors
- `stringr`: for strings
- `lubridate`: for date/times

Data visualization
=======================================================
- `ggplot2`

<img src="tidyverse-figure/unnamed-chunk-7-1.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" style="display: block; margin: auto;" />

Example
=======================================================

## Trans-Tasman Breast Cancer Study

- Recent client.
- Interested in understanding the differences in breast cancer survival times for patients in Waitemata and Brisbane hospitals.

Example --- Data import
=======================================================


```r
# Read in breast cancer data
ttbcs_tbl = read_csv("CSV/ttbcs.csv")
ttbcs_tbl
```


```
# A tibble: 1,767 x 21
   patient_code birth_date core_biopsy
          <chr>      <chr>       <chr>
 1      B003920 1931-07-08  2013-03-20
 2      B019064 1930-06-03  2011-04-18
 3      B041025 1934-05-16  2013-07-31
 4      B049838 1935-01-08  2011-06-20
 5      B055154 1938-11-23  2008-07-07
 6      B057829 1923-04-19  2009-08-03
 7      B060717 1929-01-29  2008-07-01
 8      B061836 1940-07-24  2009-12-08
 9      B073978 1939-11-10  2010-06-11
10      B089272 1943-09-10  2011-08-08
# ... with 1,757 more rows, and 18 more
#   variables: death_date <dttm>,
#   ethnicity <chr>, censored <int>,
#   dataset <chr>, t_stage <chr>,
#   n_stage <chr>, m_stage <chr>,
#   overall_tmn_stage <chr>,
#   presented <chr>,
#   surgery_type <chr>, size <dbl>,
#   radiation <chr>,
#   chemotherapy <chr>,
#   surv_time <dbl>, age <dbl>,
#   age_group <chr>, indigeneity <chr>,
#   overall_stage <chr>
```

Example --- Data transformation
=======================================================


```r
# Include a variable denoting the survival time
ttbcs_tbl %<>% 
  mutate(surv_time = as.numeric(death_date - core_biopsy) / 30.5)
```

Example --- Visualise
=======================================================

<div align="center">
<img src="data_vis.png" width=1000>

Example --- Visualise
=======================================================

<div align="center">
<img src="model_vis.png" width=1000>

Example --- Communicate
=======================================================

R markdown allows for a reproducible report that contains all of your well-organised code so you can easily come back to an old project.

<div align="center">
<img src="rmd_screenshot.png" width=1000>

Example --- Communicate
=======================================================

<div align="center">
<img src="word_screenshot.png" width=1000>


Some Resources
=======================================================

- **R for Data Science** by Hadley Wickham and Garrett Grolemund. <http://r4ds.had.co.nz/>



