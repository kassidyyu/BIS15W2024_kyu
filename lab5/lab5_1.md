---
title: "Transforming data 2: `filter()` con't"
date: "2024-01-30"
output:
  html_document: 
    theme: spacelab
    toc: true
    toc_float: true
    keep_md: true
  pdf_document:
    toc: yes
---

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Use `filter()` to extract variables of interest.  
2. Use `filter()` to extract variables of interest under multiple conditions.  

## Load the tidyverse

```r
library("tidyverse")
```

## Load the data
For this lab, we will use the following two datasets:  

1. Gaeta J., G. Sass, S. Carpenter. 2012. Biocomplexity at North Temperate Lakes LTER: Coordinated Field Studies: Large Mouth Bass Growth 2006. Environmental Data Initiative.   [link](https://portal.edirepository.org/nis/mapbrowse?scope=knb-lter-ntl&identifier=267)  

```r
fish <- read_csv("data/Gaeta_etal_CLC_data.csv")
```

```
## Rows: 4033 Columns: 6
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): lakeid, annnumber
## dbl (4): fish_id, length, radii_length_mm, scalelength
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.   [link](http://esapubs.org/archive/ecol/E084/093/)  

```r
mammals <- read_csv("data/mammal_lifehistories_v2.csv")
```

```
## Rows: 1440 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): order, family, Genus, species
## dbl (9): mass, gestation, newborn, weaning, wean mass, AFR, max. life, litte...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Let's rename some of the `mammals` variables.  

```r
mammals <- rename(mammals, genus="Genus", wean_mass="wean mass", max_life= "max. life", litter_size="litter size", litters_per_year="litters/year")
```

Do you remember the easier way?

```r
colnames(mammals)[3] <- "genus"
```

## Review
In the warm-up, we practiced extracting observations of interest. For example, we can pull out all of the fish from a specific lake.  

```r
glimpse(fish)
```

```
## Rows: 4,033
## Columns: 6
## $ lakeid          <chr> "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", …
## $ fish_id         <dbl> 299, 299, 299, 300, 300, 300, 300, 301, 301, 301, 301,…
## $ annnumber       <chr> "EDGE", "2", "1", "EDGE", "3", "2", "1", "EDGE", "3", …
## $ length          <dbl> 167, 167, 167, 175, 175, 175, 175, 194, 194, 194, 194,…
## $ radii_length_mm <dbl> 2.697443, 2.037518, 1.311795, 3.015477, 2.670733, 2.13…
## $ scalelength     <dbl> 2.697443, 2.697443, 2.697443, 3.015477, 3.015477, 3.01…
```

Let's convert `lakeid` to a factor and have a look at which states are represented in the data.  

```r
fish$lakeid <- as.factor(fish$lakeid)
levels(fish$lakeid)
```

```
##  [1] "AL"  "AR"  "BO"  "BR"  "CR"  "DY"  "FD"  "JN"  "LC"  "LJ"  "LR"  "LSG"
## [13] "MN"  "RD"  "UB"  "WS"
```

Now we can pull out any state of interest.  

```r
filter(fish, lakeid == "AL")
```

```
## # A tibble: 383 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <fct>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # ℹ 373 more rows
```

## Using `filter()` on multiple conditions
You can also use `filter()` to extract data based on multiple conditions. Below we extract only the fish that have lakeid "AL" and length >350.

```r
filter(fish, lakeid == "AL" & length > 350)
```

```
## # A tibble: 314 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <fct>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         307 EDGE         353            7.55        7.55
##  2 AL         307 13           353            7.28        7.55
##  3 AL         307 12           353            6.98        7.55
##  4 AL         307 11           353            6.73        7.55
##  5 AL         307 10           353            6.48        7.55
##  6 AL         307 9            353            6.22        7.55
##  7 AL         307 8            353            5.92        7.55
##  8 AL         307 7            353            5.44        7.55
##  9 AL         307 6            353            5.06        7.55
## 10 AL         307 5            353            4.37        7.55
## # ℹ 304 more rows
```

Notice that the `|` operator generates a different result.

```r
filter(fish, lakeid == "AL" | length > 350)
```

```
## # A tibble: 948 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <fct>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # ℹ 938 more rows
```

Rules:  
+ `filter(condition1, condition2)` will return rows where both conditions are met.  
+ `filter(condition1, !condition2)` will return all rows where condition one is true but condition 2 is not.  
+ `filter(condition1 | condition2)` will return rows where condition 1 or condition 2 is met.  
+ `filter(xor(condition1, condition2)` will return all rows where only one of the conditions is met, and not when both conditions are met.  

In this case, we filter out the fish with a length over 400 and a scale length over 11 or a radii length over 8.

```r
filter(fish, length > 400, (scalelength > 11 | radii_length_mm > 8))
```

```
## # A tibble: 23 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <fct>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         324 EDGE         406            8.21        8.21
##  2 AL         327 EDGE         413            8.33        8.33
##  3 AL         327 15           413            8.11        8.33
##  4 AL         328 EDGE         420            8.71        8.71
##  5 AL         328 16           420            8.41        8.71
##  6 AL         328 15           420            8.14        8.71
##  7 WS         180 EDGE         403           11.0        11.0 
##  8 WS         180 16           403           10.6        11.0 
##  9 WS         180 15           403           10.3        11.0 
## 10 WS         180 14           403            9.93       11.0 
## # ℹ 13 more rows
```

## Practice  
1.  From the `mammals` data, filter all members of the family Bovidae with a mass greater than 450000.

```r
filter(mammals, family == "Bovidae", mass > 450000)
```

```
## # A tibble: 7 × 13
##   order    family genus species   mass gestation newborn weaning wean_mass   AFR
##   <chr>    <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl> <dbl>
## 1 Artioda… Bovid… Bison bison   4.98e5      8.93  20000    10.7     157500  29.4
## 2 Artioda… Bovid… Bison bonasus 5   e5      9.14  23000.    6.6       -999  30.0
## 3 Artioda… Bovid… Bos   fronta… 8   e5      9.02  23033.    4.5       -999  24.2
## 4 Artioda… Bovid… Bos   javani… 6.67e5      9.83   -999     9.5       -999  25.5
## 5 Artioda… Bovid… Buba… bubalis 9.5 e5     10.5   37500     7.5       -999  19.9
## 6 Artioda… Bovid… Sync… caffer  5.05e5     11.0   42862.    9.18    166000  47.9
## 7 Artioda… Bovid… Taur… derbia… 6.80e5      8.67   -999  -999         -999  36.4
## # ℹ 3 more variables: max_life <dbl>, litter_size <dbl>, litters_per_year <dbl>
```

2. From the `mammals` data, build a data frame that compares `mass`, `gestation`, and `newborn` among the primate genera `Lophocebus`, `Erythrocebus`, and `Macaca`. Among these genera, which species has the smallest `newborn` mass?

```r
filtered_genera <- filter(mammals, genus == "Lophocebus" | genus == "Erythrocebus" | genus == "Macaca")
primate_frame <- select(filtered_genera, genus, mass, gestation, newborn)
primate_frame
```

```
## # A tibble: 15 × 4
##    genus          mass gestation newborn
##    <chr>         <dbl>     <dbl>   <dbl>
##  1 Erythrocebus  5883.      5.56    546.
##  2 Lophocebus    6726.      5.97    462.
##  3 Macaca       10037.      5.67    533.
##  4 Macaca        8858.      5.72    505.
##  5 Macaca        5575       5.43    390.
##  6 Macaca        9753.      5.49    450 
##  7 Macaca        7308.      6       486.
##  8 Macaca        6212.      5.78    458.
##  9 Macaca        3495    -999       446 
## 10 Macaca        4875       5.94    418 
## 11 Macaca        5413.      5.47    476.
## 12 Macaca        6317.      5.4     401 
## 13 Macaca        6133.      5.71    476.
## 14 Macaca        3456.      5.49    408.
## 15 Macaca        3735       5.43    391.
```


```r
min(primate_frame$newborn)
```

```
## [1] 389.5
```

```r
filter(mammals, newborn == min(primate_frame$newborn))
```

```
## # A tibble: 1 × 13
##   order    family  genus species  mass gestation newborn weaning wean_mass   AFR
##   <chr>    <chr>   <chr> <chr>   <dbl>     <dbl>   <dbl>   <dbl>     <dbl> <dbl>
## 1 Primates Cercop… Maca… maura    5575      5.43    390.    16.7      -999    60
## # ℹ 3 more variables: max_life <dbl>, litter_size <dbl>, litters_per_year <dbl>
```

## That's it! Let's take a break and then move on to part 2!  

-->[Home](https://jmledford3115.github.io/datascibiol/)  
