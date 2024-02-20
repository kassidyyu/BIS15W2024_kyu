---
title: "Lab 7 Homework"
author: "Kassidy Yu"
date: "2024-02-20"
output:
  html_document: 
    theme: spacelab
    keep_md: true
---



## Instructions

Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!

## Load the libraries


```r
library(tidyverse)
library(janitor)
library(skimr)
```

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.


```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1.  Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?\


```r
glimpse(fisheries)
```

```
## Rows: 17,692
## Columns: 71
## $ Country                   <chr> "Albania", "Albania", "Albania", "Albania", …
## $ `Common name`             <chr> "Angelsharks, sand devils nei", "Atlantic bo…
## $ `ISSCAAP group#`          <dbl> 38, 36, 37, 45, 32, 37, 33, 45, 38, 57, 33, …
## $ `ISSCAAP taxonomic group` <chr> "Sharks, rays, chimaeras", "Tunas, bonitos, …
## $ `ASFIS species#`          <chr> "10903XXXXX", "1750100101", "17710001XX", "2…
## $ `ASFIS species name`      <chr> "Squatinidae", "Sarda sarda", "Sphyraena spp…
## $ `FAO major fishing area`  <dbl> 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, …
## $ Measure                   <chr> "Quantity (tonnes)", "Quantity (tonnes)", "Q…
## $ `1950`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1951`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1952`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1953`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1954`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1955`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1956`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1957`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1958`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1959`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1960`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1961`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1962`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1963`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1964`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1965`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1966`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1967`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1968`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1969`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1970`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1971`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1972`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1973`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1974`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1975`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1976`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1977`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1978`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1979`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1980`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1981`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1982`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ `1983`                    <chr> NA, NA, NA, NA, NA, NA, "559", NA, NA, NA, N…
## $ `1984`                    <chr> NA, NA, NA, NA, NA, NA, "392", NA, NA, NA, N…
## $ `1985`                    <chr> NA, NA, NA, NA, NA, NA, "406", NA, NA, NA, N…
## $ `1986`                    <chr> NA, NA, NA, NA, NA, NA, "499", NA, NA, NA, N…
## $ `1987`                    <chr> NA, NA, NA, NA, NA, NA, "564", NA, NA, NA, N…
## $ `1988`                    <chr> NA, NA, NA, NA, NA, NA, "724", NA, NA, NA, N…
## $ `1989`                    <chr> NA, NA, NA, NA, NA, NA, "583", NA, NA, NA, N…
## $ `1990`                    <chr> NA, NA, NA, NA, NA, NA, "754", NA, NA, NA, N…
## $ `1991`                    <chr> NA, NA, NA, NA, NA, NA, "283", NA, NA, NA, N…
## $ `1992`                    <chr> NA, NA, NA, NA, NA, NA, "196", NA, NA, NA, N…
## $ `1993`                    <chr> NA, NA, NA, NA, NA, NA, "150 F", NA, NA, NA,…
## $ `1994`                    <chr> NA, NA, NA, NA, NA, NA, "100 F", NA, NA, NA,…
## $ `1995`                    <chr> "0 0", "1", NA, "0 0", "0 0", NA, "52", "30"…
## $ `1996`                    <chr> "53", "2", NA, "3", "2", NA, "104", "8", NA,…
## $ `1997`                    <chr> "20", "0 0", NA, "0 0", "0 0", NA, "65", "4"…
## $ `1998`                    <chr> "31", "12", NA, NA, NA, NA, "220", "18", NA,…
## $ `1999`                    <chr> "30", "30", NA, NA, NA, NA, "220", "18", NA,…
## $ `2000`                    <chr> "30", "25", "2", NA, NA, NA, "220", "20", NA…
## $ `2001`                    <chr> "16", "30", NA, NA, NA, NA, "120", "23", NA,…
## $ `2002`                    <chr> "79", "24", NA, "34", "6", NA, "150", "84", …
## $ `2003`                    <chr> "1", "4", NA, "22", NA, NA, "84", "178", NA,…
## $ `2004`                    <chr> "4", "2", "2", "15", "1", "2", "76", "285", …
## $ `2005`                    <chr> "68", "23", "4", "12", "5", "6", "68", "150"…
## $ `2006`                    <chr> "55", "30", "7", "18", "8", "9", "86", "102"…
## $ `2007`                    <chr> "12", "19", NA, NA, NA, NA, "132", "18", NA,…
## $ `2008`                    <chr> "23", "27", NA, NA, NA, NA, "132", "23", NA,…
## $ `2009`                    <chr> "14", "21", NA, NA, NA, NA, "154", "20", NA,…
## $ `2010`                    <chr> "78", "23", "7", NA, NA, NA, "80", "228", NA…
## $ `2011`                    <chr> "12", "12", NA, NA, NA, NA, "88", "9", NA, "…
## $ `2012`                    <chr> "5", "5", NA, NA, NA, NA, "129", "290", NA, …
```

2.  Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor.


```r
fisheries <- clean_names(fisheries)
```


```r
fisheries$country <- as.factor(fisheries$country)
fisheries$isscaap_group_number <- as.factor(fisheries$isscaap_group_number) 
fisheries$asfis_species_number <- as.factor(fisheries$asfis_species_number) 
fisheries$fao_major_fishing_area <- as.factor(fisheries$fao_major_fishing_area)
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!


```r
fisheries_tidy <- fisheries %>% 
  pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
               names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
  mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
  mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)

#Ledford added for us to clean the data! just run and be happy we don't need to write
```

3.  How many countries are represented in the data? Provide a count and list their names.


```r
fisheries_tidy %>%
  distinct(country)
```

```
## # A tibble: 203 × 1
##    country            
##    <fct>              
##  1 Albania            
##  2 Algeria            
##  3 American Samoa     
##  4 Angola             
##  5 Anguilla           
##  6 Antigua and Barbuda
##  7 Argentina          
##  8 Aruba              
##  9 Australia          
## 10 Bahamas            
## # ℹ 193 more rows
```

```r
n_distinct(fisheries_tidy$country)
```

```
## [1] 203
```

4.  Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.


```r
fisheries_focus <- fisheries_tidy %>%
  select(country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch)
```

5.  Based on the asfis_species_number, how many distinct fish species were caught as part of these data?


```r
n_distinct(fisheries_focus$asfis_species_number)
```

```
## [1] 1551
```

6.  Which country had the largest overall catch in the year 2000?


```r
fisheries_focus %>%
  filter(catch == max(catch, na.rm=T))
```

```
## # A tibble: 1 × 6
##   country isscaap_taxonomic_group  asfis_species_name asfis_species_number  year
##   <fct>   <chr>                    <chr>              <fct>                <dbl>
## 1 Peru    Herrings, sardines, anc… Engraulis ringens  1210600208            1970
## # ℹ 1 more variable: catch <dbl>
```

7.  Which country caught the most sardines (*Sardina pilchardus*) between the years 1990-2000?


```r
fisheries_focus %>%
  filter(asfis_species_name == "Sardina pilchardus", between(year, 1990, 2000)) %>%
  count(country) %>%
  arrange(desc(n))
```

```
## # A tibble: 37 × 2
##    country            n
##    <fct>          <int>
##  1 Spain             33
##  2 France            25
##  3 Morocco           22
##  4 Portugal          22
##  5 Netherlands       15
##  6 Germany           14
##  7 United Kingdom    12
##  8 Albania           11
##  9 Algeria           11
## 10 Denmark           11
## # ℹ 27 more rows
```

8.  Which five countries caught the most cephalopods between 2008-2012?


```r
fisheries_focus %>%
  filter(isscaap_taxonomic_group == "Squids, cuttlefishes, octopuses", between(year, 2008, 2012)) %>%
  count(country) %>%
  arrange(desc(n))
```

```
## # A tibble: 122 × 2
##    country                      n
##    <fct>                    <int>
##  1 Spain                      116
##  2 Korea, Republic of          85
##  3 Portugal                    64
##  4 France                      59
##  5 United States of America    56
##  6 Australia                   51
##  7 Italy                       41
##  8 Greece                      40
##  9 Thailand                    40
## 10 China                       38
## # ℹ 112 more rows
```

9.  Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)


```r
fisheries_focus %>%
  filter(asfis_species_name != "Osteichthyes", between(year, 2008, 2012)) %>%
  count(asfis_species_name) %>%
  arrange(desc(n))
```

```
## # A tibble: 1,471 × 2
##    asfis_species_name     n
##    <chr>              <int>
##  1 Thunnus albacares   1006
##  2 Thunnus obesus       923
##  3 Xiphias gladius      837
##  4 Elasmobranchii       761
##  5 Katsuwonus pelamis   761
##  6 Thunnus alalunga     755
##  7 Makaira nigricans    584
##  8 Mugilidae            482
##  9 Rajiformes           427
## 10 Scombroidei          417
## # ℹ 1,461 more rows
```

10. Use the data to do at least one analysis of your choice.

```r
fisheries_focus %>%
  group_by(country) %>%
  filter(asfis_species_name == "Osteichthyes") %>%
  count(year) %>%
  arrange(desc(n))
```

```
## # A tibble: 10,908 × 3
## # Groups:   country [201]
##    country  year     n
##    <fct>   <dbl> <int>
##  1 Japan    1974    17
##  2 Japan    1976    17
##  3 Japan    1989    17
##  4 Japan    1971    16
##  5 Japan    1972    16
##  6 Japan    1973    16
##  7 Japan    1977    16
##  8 Japan    1978    16
##  9 Japan    1980    16
## 10 Japan    1981    16
## # ℹ 10,898 more rows
```

## Push your final code to GitHub!

Please be sure that you check the `keep md` file in the knit preferences.
