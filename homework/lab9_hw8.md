---
title: "Homework 8"
author: "Kassidy Yu"
date: "2024-02-13"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches <- read_csv("data/sydneybeaches.csv")
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
glimpse(sydneybeaches)
```

```
## Rows: 3,690
## Columns: 8
## $ BeachId                   <dbl> 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, …
## $ Region                    <chr> "Sydney City Ocean Beaches", "Sydney City Oc…
## $ Council                   <chr> "Randwick Council", "Randwick Council", "Ran…
## $ Site                      <chr> "Clovelly Beach", "Clovelly Beach", "Clovell…
## $ Longitude                 <dbl> 151.2675, 151.2675, 151.2675, 151.2675, 151.…
## $ Latitude                  <dbl> -33.91449, -33.91449, -33.91449, -33.91449, …
## $ Date                      <chr> "02/01/2013", "06/01/2013", "12/01/2013", "1…
## $ `Enterococci (cfu/100ml)` <dbl> 19, 3, 2, 13, 8, 7, 11, 97, 3, 0, 6, 0, 1, 8…
```

```r
summary(sydneybeaches)
```

```
##     BeachId         Region            Council              Site          
##  Min.   :22.00   Length:3690        Length:3690        Length:3690       
##  1st Qu.:24.00   Class :character   Class :character   Class :character  
##  Median :26.00   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :25.87                                                           
##  3rd Qu.:27.40                                                           
##  Max.   :29.00                                                           
##                                                                          
##    Longitude        Latitude          Date           Enterococci (cfu/100ml)
##  Min.   :151.3   Min.   :-33.98   Length:3690        Min.   :   0.00        
##  1st Qu.:151.3   1st Qu.:-33.95   Class :character   1st Qu.:   1.00        
##  Median :151.3   Median :-33.92   Mode  :character   Median :   5.00        
##  Mean   :151.3   Mean   :-33.93                      Mean   :  33.92        
##  3rd Qu.:151.3   3rd Qu.:-33.90                      3rd Qu.:  17.00        
##  Max.   :151.3   Max.   :-33.89                      Max.   :4900.00        
##                                                      NA's   :29
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

```
## here() starts at /Users/kcyu/Desktop/BIS15W2024_kyu
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("homework", "data", "sydneybeaches.csv")) %>% clean_names()
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?
Yes, each column is a variable and each row is the observations of the variables. They are in long format.

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`

```r
sydneybeaches_long <- sydneybeaches %>%
  select(site, date, enterococci_cfu_100ml)
```

4. Pivot the data such that the dates are column names and each beach only appears once (wide format). Name the object `sydneybeaches_wide`

```r
sydneybeaches_wide <- sydneybeaches %>%
  pivot_wider(names_from = "date",
              values_from = "enterococci_cfu_100ml")
```

5. Pivot the data back so that the dates are data and not column names.

```r
sydneybeaches_relong <- sydneybeaches_wide %>%
  pivot_longer(-c(beach_id:latitude),
               names_to = "date",
               values_to = "enterococci_cfu_100ml")
```

6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.

```r
sydneybeaches_long_datesep <- sydneybeaches_long %>%
  separate(date, into = c("day", "month", "year"), sep = "/")
```

7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

```r
avg_by_year_beach <- sydneybeaches_long_datesep %>%
  group_by(site, year) %>%
  summarize(mean = mean(enterococci_cfu_100ml, na.rm=T), .groups = 'keep')
```

8. Make the output from question 7 easier to read by pivoting it to wide format.

```r
avg_by_year_beach %>%
  pivot_wider(names_from = site,
              values_from = mean)
```

```
## # A tibble: 6 × 12
## # Groups:   year [6]
##   year  `Bondi Beach` `Bronte Beach` `Clovelly Beach` `Coogee Beach`
##   <chr>         <dbl>          <dbl>            <dbl>          <dbl>
## 1 2013           32.2           26.8             9.28           39.7
## 2 2014           11.1           17.5            13.8            52.6
## 3 2015           14.3           23.6             8.82           40.3
## 4 2016           19.4           61.3            11.3            59.5
## 5 2017           13.2           16.8             7.93           20.7
## 6 2018           22.9           43.4            10.6            21.6
## # ℹ 7 more variables: `Gordons Bay (East)` <dbl>, `Little Bay Beach` <dbl>,
## #   `Malabar Beach` <dbl>, `Maroubra Beach` <dbl>,
## #   `South Maroubra Beach` <dbl>, `South Maroubra Rockpool` <dbl>,
## #   `Tamarama Beach` <dbl>
```

9. What was the most polluted beach in 2013?\
Little Bay Beach was the most polluted in 2013

```r
avg_by_year_beach %>%
  filter(year == 2013) %>%
  arrange(-mean)
```

```
## # A tibble: 11 × 3
## # Groups:   site, year [11]
##    site                    year    mean
##    <chr>                   <chr>  <dbl>
##  1 Little Bay Beach        2013  122.  
##  2 Malabar Beach           2013  101.  
##  3 South Maroubra Rockpool 2013   96.4 
##  4 Maroubra Beach          2013   47.1 
##  5 Coogee Beach            2013   39.7 
##  6 South Maroubra Beach    2013   39.3 
##  7 Bondi Beach             2013   32.2 
##  8 Tamarama Beach          2013   29.7 
##  9 Bronte Beach            2013   26.8 
## 10 Gordons Bay (East)      2013   24.8 
## 11 Clovelly Beach          2013    9.28
```

10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   