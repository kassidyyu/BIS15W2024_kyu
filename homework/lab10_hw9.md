---
title: "Homework 9"
author: "Kassidy Yu"
date: "2024-02-19"
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
library(here)
library(naniar)
```

For this homework, we will take a departure from biological data and use data about California colleges. These data are a subset of the national college scorecard (https://collegescorecard.ed.gov/data/). Load the `ca_college_data.csv` as a new object called `colleges`.

```r
colleges <- read_csv("data/ca_college_data.csv")
```

```
## Rows: 341 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): INSTNM, CITY, STABBR, ZIP
## dbl (6): ADM_RATE, SAT_AVG, PCIP26, COSTT4_A, C150_4_POOLED, PFTFTUG1_EF
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

The variables are a bit hard to decipher, here is a key:  

INSTNM: Institution name  
CITY: California city  
STABBR: Location state  
ZIP: Zip code  
ADM_RATE: Admission rate  
SAT_AVG: SAT average score  
PCIP26: Percentage of degrees awarded in Biological And Biomedical Sciences  
COSTT4_A: Annual cost of attendance  
C150_4_POOLED: 4-year completion rate  
PFTFTUG1_EF: Percentage of undergraduate students who are first-time, full-time degree/certificate-seeking undergraduate students  

1. Use your preferred function(s) to have a look at the data and get an idea of its structure. Make sure you summarize NA's and determine whether or not the data are tidy. You may also consider dealing with any naming issues.

```r
glimpse(colleges)
```

```
## Rows: 341
## Columns: 10
## $ INSTNM        <chr> "Grossmont College", "College of the Sequoias", "College…
## $ CITY          <chr> "El Cajon", "Visalia", "San Mateo", "Ventura", "Oxnard",…
## $ STABBR        <chr> "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "C…
## $ ZIP           <chr> "92020-1799", "93277-2214", "94402-3784", "93003-3872", …
## $ ADM_RATE      <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ SAT_AVG       <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ PCIP26        <dbl> 0.0016, 0.0066, 0.0038, 0.0035, 0.0085, 0.0151, 0.0000, …
## $ COSTT4_A      <dbl> 7956, 8109, 8278, 8407, 8516, 8577, 8580, 9181, 9281, 93…
## $ C150_4_POOLED <dbl> NA, NA, NA, NA, NA, NA, 0.2334, NA, NA, NA, NA, 0.1704, …
## $ PFTFTUG1_EF   <dbl> 0.3546, 0.5413, 0.3567, 0.3824, 0.2753, 0.4286, 0.2307, …
```

```r
names(colleges)
```

```
##  [1] "INSTNM"        "CITY"          "STABBR"        "ZIP"          
##  [5] "ADM_RATE"      "SAT_AVG"       "PCIP26"        "COSTT4_A"     
##  [9] "C150_4_POOLED" "PFTFTUG1_EF"
```


```r
colleges <- colleges %>%
  rename(institution = "INSTNM", 
           city = "CITY", 
           state = "STABBR", 
           zip = "ZIP", 
           admission_rate = "ADM_RATE", 
           sat_avg = "SAT_AVG",
           pct_bio_degrees = "PCIP26",
           annual_cost = "COSTT4_A",
           completion_rate = "C150_4_POOLED", 
           pct_first_time_undergrad = "PFTFTUG1_EF")
```

2. Which cities in California have the highest number of colleges?

```r
colleges %>%
  group_by(city) %>%
  count() %>%
  arrange(-n)
```

```
## # A tibble: 161 × 2
## # Groups:   city [161]
##    city              n
##    <chr>         <int>
##  1 Los Angeles      24
##  2 San Diego        18
##  3 San Francisco    15
##  4 Sacramento       10
##  5 Berkeley          9
##  6 Oakland           9
##  7 Claremont         7
##  8 Pasadena          6
##  9 Fresno            5
## 10 Irvine            5
## # ℹ 151 more rows
```

3. Based on your answer to #2, make a plot that shows the number of colleges in the top 10 cities.

```r
colleges %>%
  group_by(city) %>%
  summarize(num_colleges = n()) %>%
  top_n(10, num_colleges) %>%
  ggplot(aes(x = city, y = num_colleges))+
  geom_col()+
  coord_flip()
```

![](lab10_hw9_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

4. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest average cost? Where is it located?\
Claremont, CA has the highest average cost.

```r
colleges %>%
  group_by(city) %>%
  summarize(mean_cost = mean(annual_cost, na.rm=T)) %>%
  arrange(-mean_cost)
```

```
## # A tibble: 161 × 2
##    city                mean_cost
##    <chr>                   <dbl>
##  1 Claremont               66498
##  2 Malibu                  66152
##  3 Valencia                64686
##  4 Orange                  64501
##  5 Redlands                61542
##  6 Moraga                  61095
##  7 Atherton                56035
##  8 Thousand Oaks           54373
##  9 Rancho Palos Verdes     50758
## 10 La Verne                50603
## # ℹ 151 more rows
```

```r
colleges %>%
  filter(city == "Claremont")
```

```
## # A tibble: 7 × 10
##   institution           city  state zip   admission_rate sat_avg pct_bio_degrees
##   <chr>                 <chr> <chr> <chr>          <dbl>   <dbl>           <dbl>
## 1 Pomona College        Clar… CA    9171…         0.0944    1442          0.171 
## 2 Pitzer College        Clar… CA    9171…         0.137       NA          0.0888
## 3 Scripps College       Clar… CA    9171…         0.299     1353          0.152 
## 4 Claremont McKenna Co… Clar… CA    9171…         0.0944    1413          0.0681
## 5 Harvey Mudd College   Clar… CA    91711         0.129     1496          0.0674
## 6 Claremont Graduate U… Clar… CA    9171…        NA           NA         NA     
## 7 Claremont School of … Clar… CA    9171…        NA           NA         NA     
## # ℹ 3 more variables: annual_cost <dbl>, completion_rate <dbl>,
## #   pct_first_time_undergrad <dbl>
```

5. Based on your answer to #4, make a plot that compares the cost of the individual colleges in the most expensive city. Bonus! Add UC Davis here to see how it compares :>).

```r
colleges %>%
  filter(city == "Claremont" | city == "Davis") %>%
  ggplot(aes(x = institution, y = annual_cost))+
  geom_col()+
  coord_flip()
```

```
## Warning: Removed 2 rows containing missing values (`position_stack()`).
```

![](lab10_hw9_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

6. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What do you think this means?\
There seems to be a negative, linear association between variables. That is, the lower admission rates correspond to a higher completion rate.

```r
colleges %>%
  ggplot(aes(x = admission_rate, y = completion_rate))+
  geom_point()+
  geom_smooth(method=lm, se=T)
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

```
## Warning: Removed 251 rows containing non-finite values (`stat_smooth()`).
```

```
## Warning: Removed 251 rows containing missing values (`geom_point()`).
```

![](lab10_hw9_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

7. Is there a relationship between cost and four-year completion rate? (You don't need to do the stats, just produce a plot). What do you think this means?\
There seems to be a positive linear association between cost and completion rate - higher cost corresponds to a higher completion rate.

```r
colleges %>%
  ggplot(aes(x = annual_cost, y = completion_rate))+
  geom_point()+
  geom_smooth(method=lm, se=T)
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

```
## Warning: Removed 225 rows containing non-finite values (`stat_smooth()`).
```

```
## Warning: Removed 225 rows containing missing values (`geom_point()`).
```

![](lab10_hw9_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

8. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Make a new data frame that is restricted to UC institutions. You can remove `Hastings College of Law` and `UC San Francisco` as we are only interested in undergraduate institutions.

Remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

```r
univ_calif_final <- colleges %>%
  separate(institution, into = c("UNIV", "CAMPUS"), sep = "-") %>%
  filter(UNIV == "University of California", city != "San Francisco")
```

```
## Warning: Expected 2 pieces. Additional pieces discarded in 9 rows [140, 145, 165, 173,
## 177, 292, 298, 299, 300].
```

```
## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 264 rows [1, 2, 3, 4, 5,
## 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...].
```

9. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final %>%
  filter(admission_rate == min(admission_rate) | admission_rate == max(admission_rate))
```

```
## # A tibble: 2 × 11
##   UNIV           CAMPUS city  state zip   admission_rate sat_avg pct_bio_degrees
##   <chr>          <chr>  <chr> <chr> <chr>          <dbl>   <dbl>           <dbl>
## 1 University of… River… Rive… CA    92521          0.663    1078           0.149
## 2 University of… Berke… Berk… CA    94720          0.169    1422           0.105
## # ℹ 3 more variables: annual_cost <dbl>, completion_rate <dbl>,
## #   pct_first_time_undergrad <dbl>
```

```r
univ_calif_final %>%
  filter(admission_rate == min(admission_rate) | admission_rate == max(admission_rate)) %>%
  ggplot(aes(x=CAMPUS, y= admission_rate))+
  geom_col()
```

![](lab10_hw9_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

10. If you wanted to get a degree in biological or biomedical sciences, which campus confers the majority of these degrees? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final %>%
  select(CAMPUS, pct_bio_degrees) %>%
  arrange(-pct_bio_degrees)
```

```
## # A tibble: 8 × 2
##   CAMPUS        pct_bio_degrees
##   <chr>                   <dbl>
## 1 San Diego               0.216
## 2 Davis                   0.198
## 3 Santa Cruz              0.193
## 4 Los Angeles             0.155
## 5 Riverside               0.149
## 6 Santa Barbara           0.108
## 7 Irvine                  0.107
## 8 Berkeley                0.105
```

```r
univ_calif_final %>%
  ggplot(aes(x = CAMPUS, y = pct_bio_degrees))+
  geom_col()+
  coord_flip()
```

![](lab10_hw9_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
