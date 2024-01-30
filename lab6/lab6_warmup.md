---
title: "Lab6Warmup"
output: 
  html_document: 
    keep_md: yes
date: "2024-01-30"
---



## Load the libraries

```r
library("tidyverse")
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

## Load the bison data

```r
bison <- read_csv("data/bison.csv")
```

```
## Rows: 8325 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (3): data_code, animal_code, animal_sex
## dbl (5): rec_year, rec_month, rec_day, animal_weight, animal_yob
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Have a look

```r
glimpse(bison)
```

```
## Rows: 8,325
## Columns: 8
## $ data_code     <chr> "CBH01", "CBH01", "CBH01", "CBH01", "CBH01", "CBH01", "C…
## $ rec_year      <dbl> 1994, 1994, 1994, 1994, 1994, 1994, 1994, 1994, 1994, 19…
## $ rec_month     <dbl> 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, …
## $ rec_day       <dbl> 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8,…
## $ animal_code   <chr> "813", "834", "B-301", "B-402", "B-403", "B-502", "B-503…
## $ animal_sex    <chr> "F", "F", "F", "F", "F", "F", "F", "F", "F", "F", "F", "…
## $ animal_weight <dbl> 890, 1074, 1060, 989, 1062, 978, 1068, 1024, 978, 1188, …
## $ animal_yob    <dbl> 1981, 1983, 1983, 1984, 1984, 1985, 1985, 1985, 1986, 19…
```
## Pull out code, sex, weight, year of birth & store the dataframe as a new object

```r
bison_new <- bison %>%
  select(animal_code, animal_sex, animal_weight, animal_yob)
```


## Animals born between 1980-1990

```r
bison_new %>%
  filter(animal_yob >= 1980 & animal_yob <= 1990)
```

```
## # A tibble: 435 × 4
##    animal_code animal_sex animal_weight animal_yob
##    <chr>       <chr>              <dbl>      <dbl>
##  1 813         F                    890       1981
##  2 834         F                   1074       1983
##  3 B-301       F                   1060       1983
##  4 B-402       F                    989       1984
##  5 B-403       F                   1062       1984
##  6 B-502       F                    978       1985
##  7 B-503       F                   1068       1985
##  8 B-504       F                   1024       1985
##  9 B-601       F                    978       1986
## 10 B-602       F                   1188       1986
## # ℹ 425 more rows
```

```r
bison_new %>%
  filter(between(animal_yob, 1980, 1990))
```

```
## # A tibble: 435 × 4
##    animal_code animal_sex animal_weight animal_yob
##    <chr>       <chr>              <dbl>      <dbl>
##  1 813         F                    890       1981
##  2 834         F                   1074       1983
##  3 B-301       F                   1060       1983
##  4 B-402       F                    989       1984
##  5 B-403       F                   1062       1984
##  6 B-502       F                    978       1985
##  7 B-503       F                   1068       1985
##  8 B-504       F                   1024       1985
##  9 B-601       F                    978       1986
## 10 B-602       F                   1188       1986
## # ℹ 425 more rows
```

## How many male and female bison are represented between 1980-1990?

```r
males <- bison_new %>% #first pull out the males
  filter(animal_yob >= 1980 & animal_yob <= 1990) %>%
  filter(animal_sex == "M")
males
```

```
## # A tibble: 21 × 4
##    animal_code animal_sex animal_weight animal_yob
##    <chr>       <chr>              <dbl>      <dbl>
##  1 108         M                   1728       1987
##  2 888         M                   1726       1988
##  3 88Q         M                   1712       1988
##  4 892         M                   1306       1989
##  5 89F         M                   1682       1989
##  6 89N         M                   1594       1989
##  7 904         M                   1552       1990
##  8 905         M                   1572       1990
##  9 908         M                   1538       1990
## 10 90L         M                   1422       1990
## # ℹ 11 more rows
```

```r
table(males$animal_sex)
```

```
## 
##  M 
## 21
```


```r
females <- bison_new %>% #first pull out the males
  filter(animal_yob >= 1980 & animal_yob <= 1990) %>%
  filter(animal_sex == "F")
females
```

```
## # A tibble: 414 × 4
##    animal_code animal_sex animal_weight animal_yob
##    <chr>       <chr>              <dbl>      <dbl>
##  1 813         F                    890       1981
##  2 834         F                   1074       1983
##  3 B-301       F                   1060       1983
##  4 B-402       F                    989       1984
##  5 B-403       F                   1062       1984
##  6 B-502       F                    978       1985
##  7 B-503       F                   1068       1985
##  8 B-504       F                   1024       1985
##  9 B-601       F                    978       1986
## 10 B-602       F                   1188       1986
## # ℹ 404 more rows
```

```r
table(females$animal_sex)
```

```
## 
##   F 
## 414
```

## Mean of males

```r
mean(males$animal_weight)
```

```
## [1] 1543.333
```

## Mean of females

```r
mean(females$animal_weight)
```

```
## [1] 1017.314
```
