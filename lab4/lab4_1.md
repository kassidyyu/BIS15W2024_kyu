---
title: "Transforming data 1: Dplyr `select()`"
date: "2024-01-23"
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

*At the end of this exercise, you will be able to:*\
1. Use summary functions to assess the structure of a data frame.\
2. Us the select function of `dplyr` to build data frames restricted to variable of interest.\
3. Use the `rename()` function to provide new, consistent names to variables in data frames.

## Load the tidyverse

For the remainder of the quarter, we will work within the `tidyverse`. At the start of each lab, the library needs to be loaded as shown below.


```r
library("tidyverse")
```

## Load the data

These data are from: Gaeta J., G. Sass, S. Carpenter. 2012. Biocomplexity at North Temperate Lakes LTER: Coordinated Field Studies: Large Mouth Bass Growth 2006. Environmental Data Initiative. [link](https://portal.edirepository.org/nis/mapbrowse?scope=knb-lter-ntl&identifier=267)


```r
fish <- readr::read_csv("data/Gaeta_etal_CLC_data.csv")
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

## Data Structure

Once data have been uploaded, let's get an idea of its structure, contents, and dimensions.


```r
str(fish)
```

```
## spc_tbl_ [4,033 × 6] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ lakeid         : chr [1:4033] "AL" "AL" "AL" "AL" ...
##  $ fish_id        : num [1:4033] 299 299 299 300 300 300 300 301 301 301 ...
##  $ annnumber      : chr [1:4033] "EDGE" "2" "1" "EDGE" ...
##  $ length         : num [1:4033] 167 167 167 175 175 175 175 194 194 194 ...
##  $ radii_length_mm: num [1:4033] 2.7 2.04 1.31 3.02 2.67 ...
##  $ scalelength    : num [1:4033] 2.7 2.7 2.7 3.02 3.02 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   lakeid = col_character(),
##   ..   fish_id = col_double(),
##   ..   annnumber = col_character(),
##   ..   length = col_double(),
##   ..   radii_length_mm = col_double(),
##   ..   scalelength = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

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

## Tidyverse

The [tidyverse](www.tidyverse.org) is an "opinionated" collection of packages that make workflow in R easier. The packages operate more intuitively than base R commands and share a common organizational philosophy.\
![Data Science Workflow in the Tidyverse.](tidy-1.png)

## dplyr

The first package that we will use that is part of the tidyverse is `dplyr`. `dplyr` is used to transform data frames by extracting, rearranging, and summarizing data such that they are focused on a question of interest. This is very helpful, especially when wrangling large data, and makes dplyr one of most frequently used packages in the tidyverse. The two functions we will use most are `select()` and `filter()`.

## `select()`

Select allows you to pull out columns of interest from a dataframe. To do this, just add the names of the columns to the `select()` command. The order in which you add them, will determine the order in which they appear in the output.


```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```

We are only interested in lakeid and scalelength.


```r
select(fish, "lakeid", "scalelength")
```

```
## # A tibble: 4,033 × 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # ℹ 4,023 more rows
```

To add a range of columns use `start_col:end_col`.


```r
select(fish, fish_id:length)
```

```
## # A tibble: 4,033 × 3
##    fish_id annnumber length
##      <dbl> <chr>      <dbl>
##  1     299 EDGE         167
##  2     299 2            167
##  3     299 1            167
##  4     300 EDGE         175
##  5     300 3            175
##  6     300 2            175
##  7     300 1            175
##  8     301 EDGE         194
##  9     301 3            194
## 10     301 2            194
## # ℹ 4,023 more rows
```

The - operator is useful in select. It allows us to select everything except the specified variables.


```r
select(fish, -fish_id, -annnumber, -length, -radii_length_mm)
```

```
## # A tibble: 4,033 × 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # ℹ 4,023 more rows
```

For very large data frames with lots of variables, `select()` utilizes lots of different operators to make things easier. Let's say we are only interested in the variables that deal with length.


```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```


```r
select(fish, contains("length"))
```

```
## # A tibble: 4,033 × 3
##    length radii_length_mm scalelength
##     <dbl>           <dbl>       <dbl>
##  1    167            2.70        2.70
##  2    167            2.04        2.70
##  3    167            1.31        2.70
##  4    175            3.02        3.02
##  5    175            2.67        3.02
##  6    175            2.14        3.02
##  7    175            1.23        3.02
##  8    194            3.34        3.34
##  9    194            2.97        3.34
## 10    194            2.29        3.34
## # ℹ 4,023 more rows
```

When columns are sequentially named, `starts_with()` makes selecting columns easier.


```r
select(fish, starts_with("radii"))
```

```
## # A tibble: 4,033 × 1
##    radii_length_mm
##              <dbl>
##  1            2.70
##  2            2.04
##  3            1.31
##  4            3.02
##  5            2.67
##  6            2.14
##  7            1.23
##  8            3.34
##  9            2.97
## 10            2.29
## # ℹ 4,023 more rows
```

Options to select columns based on a specific criteria include:\
1. ends_with() = Select columns that end with a character string\
2. contains() = Select columns that contain a character string\
3. matches() = Select columns that match a regular expression\
4. one_of() = Select columns names that are from a group of names


```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```


```r
select(fish, ends_with("id"))
```

```
## # A tibble: 4,033 × 2
##    lakeid fish_id
##    <chr>    <dbl>
##  1 AL         299
##  2 AL         299
##  3 AL         299
##  4 AL         300
##  5 AL         300
##  6 AL         300
##  7 AL         300
##  8 AL         301
##  9 AL         301
## 10 AL         301
## # ℹ 4,023 more rows
```


```r
select(fish, contains("fish"))
```

```
## # A tibble: 4,033 × 1
##    fish_id
##      <dbl>
##  1     299
##  2     299
##  3     299
##  4     300
##  5     300
##  6     300
##  7     300
##  8     301
##  9     301
## 10     301
## # ℹ 4,023 more rows
```

We won't cover regular expressions [regex](https://en.wikipedia.org/wiki/Regular_expression) in this class, but the following code is helpful when you know that a column contains a letter (in this case "a") followed by a subsequent string (in this case "er").


```r
select(fish, matches("a.+er"))
```

```
## # A tibble: 4,033 × 1
##    annnumber
##    <chr>    
##  1 EDGE     
##  2 2        
##  3 1        
##  4 EDGE     
##  5 3        
##  6 2        
##  7 1        
##  8 EDGE     
##  9 3        
## 10 2        
## # ℹ 4,023 more rows
```

You can also select columns based on the class of data.


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


```r
select_if(fish, is.numeric)
```

```
## # A tibble: 4,033 × 4
##    fish_id length radii_length_mm scalelength
##      <dbl>  <dbl>           <dbl>       <dbl>
##  1     299    167            2.70        2.70
##  2     299    167            2.04        2.70
##  3     299    167            1.31        2.70
##  4     300    175            3.02        3.02
##  5     300    175            2.67        3.02
##  6     300    175            2.14        3.02
##  7     300    175            1.23        3.02
##  8     301    194            3.34        3.34
##  9     301    194            2.97        3.34
## 10     301    194            2.29        3.34
## # ℹ 4,023 more rows
```

To select all columns that are *not* a class of data, you need to add a `~`.


```r
select_if(fish, ~!is.numeric(.))
```

```
## # A tibble: 4,033 × 2
##    lakeid annnumber
##    <chr>  <chr>    
##  1 AL     EDGE     
##  2 AL     2        
##  3 AL     1        
##  4 AL     EDGE     
##  5 AL     3        
##  6 AL     2        
##  7 AL     1        
##  8 AL     EDGE     
##  9 AL     3        
## 10 AL     2        
## # ℹ 4,023 more rows
```

## Practice

For this exercise we will use life history data `mammal_lifehistories_v2.csv` for mammals. The [data](http://esapubs.org/archive/ecol/E084/093/) are from:\
**S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.**

Load the data.


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

1.  Use one or more of your favorite functions to assess the structure of the data.\


```r
glimpse(mammals)
```

```
## Rows: 1,440
## Columns: 13
## $ order          <chr> "Artiodactyla", "Artiodactyla", "Artiodactyla", "Artiod…
## $ family         <chr> "Antilocapridae", "Bovidae", "Bovidae", "Bovidae", "Bov…
## $ Genus          <chr> "Antilocapra", "Addax", "Aepyceros", "Alcelaphus", "Amm…
## $ species        <chr> "americana", "nasomaculatus", "melampus", "buselaphus",…
## $ mass           <dbl> 45375.0, 182375.0, 41480.0, 150000.0, 28500.0, 55500.0,…
## $ gestation      <dbl> 8.13, 9.39, 6.35, 7.90, 6.80, 5.08, 5.72, 5.50, 8.93, 9…
## $ newborn        <dbl> 3246.36, 5480.00, 5093.00, 10166.67, -999.00, 3810.00, …
## $ weaning        <dbl> 3.00, 6.50, 5.63, 6.50, -999.00, 4.00, 4.04, 2.13, 10.7…
## $ `wean mass`    <dbl> 8900, -999, 15900, -999, -999, -999, -999, -999, 157500…
## $ AFR            <dbl> 13.53, 27.27, 16.66, 23.02, -999.00, 14.89, 10.23, 20.1…
## $ `max. life`    <dbl> 142, 308, 213, 240, -999, 251, 228, 255, 300, 324, 300,…
## $ `litter size`  <dbl> 1.85, 1.00, 1.00, 1.00, 1.00, 1.37, 1.00, 1.00, 1.00, 1…
## $ `litters/year` <dbl> 1.00, 0.99, 0.95, -999.00, -999.00, 2.00, -999.00, 1.89…
```

2.  Are there any NAs? Are you sure? Try taking an average of `max. life` as a test.\
    There doesn't seem to be NAs.


```r
mean(mammals$`max. life`)
```

```
## [1] -490.2556
```


```r
mean(mammals$`max. life`, na.rm=T)
```

```
## [1] -490.2556
```

3.  What are the names of the columns in the `mammals` data?


```r
colnames(mammals)
```

```
##  [1] "order"        "family"       "Genus"        "species"      "mass"        
##  [6] "gestation"    "newborn"      "weaning"      "wean mass"    "AFR"         
## [11] "max. life"    "litter size"  "litters/year"
```

4.  Rename any columns that have capitol letters or punctuation issues.\


```r
colnames(mammals)[3] <- "genus"
colnames(mammals)[9] <- "wean_mass"
colnames(mammals)[11] <- "max_life"
colnames(mammals)[12] <- "litter_size"
colnames(mammals)[13] <- "litters_per_year"
colnames(mammals)
```

```
##  [1] "order"            "family"           "genus"            "species"         
##  [5] "mass"             "gestation"        "newborn"          "weaning"         
##  [9] "wean_mass"        "AFR"              "max_life"         "litter_size"     
## [13] "litters_per_year"
```

5.  We are only interested in the variables `genus`, `species`, and `mass`. Use `select()` to build a new dataframe `mass` focused on these variables.


```r
mass <- select(mammals, "genus", "species", "mass")
mass
```

```
## # A tibble: 1,440 × 3
##    genus       species          mass
##    <chr>       <chr>           <dbl>
##  1 Antilocapra americana      45375 
##  2 Addax       nasomaculatus 182375 
##  3 Aepyceros   melampus       41480 
##  4 Alcelaphus  buselaphus    150000 
##  5 Ammodorcas  clarkei        28500 
##  6 Ammotragus  lervia         55500 
##  7 Antidorcas  marsupialis    30000 
##  8 Antilope    cervicapra     37500 
##  9 Bison       bison         497667.
## 10 Bison       bonasus       500000 
## # ℹ 1,430 more rows
```

6.  What if we only wanted to exclude `order` and `family`? Use the `-` operator to make the code efficient.


```r
select(mammals, -order, -family)
```

```
## # A tibble: 1,440 × 11
##    genus      species   mass gestation newborn weaning wean_mass    AFR max_life
##    <chr>      <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl>  <dbl>    <dbl>
##  1 Antilocap… americ… 4.54e4      8.13   3246.    3         8900   13.5      142
##  2 Addax      nasoma… 1.82e5      9.39   5480     6.5       -999   27.3      308
##  3 Aepyceros  melamp… 4.15e4      6.35   5093     5.63     15900   16.7      213
##  4 Alcelaphus busela… 1.5 e5      7.9   10167.    6.5       -999   23.0      240
##  5 Ammodorcas clarkei 2.85e4      6.8    -999  -999         -999 -999       -999
##  6 Ammotragus lervia  5.55e4      5.08   3810     4         -999   14.9      251
##  7 Antidorcas marsup… 3   e4      5.72   3910     4.04      -999   10.2      228
##  8 Antilope   cervic… 3.75e4      5.5    3846     2.13      -999   20.1      255
##  9 Bison      bison   4.98e5      8.93  20000    10.7     157500   29.4      300
## 10 Bison      bonasus 5   e5      9.14  23000.    6.6       -999   30.0      324
## # ℹ 1,430 more rows
## # ℹ 2 more variables: litter_size <dbl>, litters_per_year <dbl>
```

7.  Select the columns that include "mass" as part of the name.\


```r
select(mammals, contains("mass"))
```

```
## # A tibble: 1,440 × 2
##       mass wean_mass
##      <dbl>     <dbl>
##  1  45375       8900
##  2 182375       -999
##  3  41480      15900
##  4 150000       -999
##  5  28500       -999
##  6  55500       -999
##  7  30000       -999
##  8  37500       -999
##  9 497667.    157500
## 10 500000       -999
## # ℹ 1,430 more rows
```

8.  Select all of the columns that are of class `character`.\


```r
select_if(mammals, is.character)
```

```
## # A tibble: 1,440 × 4
##    order        family         genus       species      
##    <chr>        <chr>          <chr>       <chr>        
##  1 Artiodactyla Antilocapridae Antilocapra americana    
##  2 Artiodactyla Bovidae        Addax       nasomaculatus
##  3 Artiodactyla Bovidae        Aepyceros   melampus     
##  4 Artiodactyla Bovidae        Alcelaphus  buselaphus   
##  5 Artiodactyla Bovidae        Ammodorcas  clarkei      
##  6 Artiodactyla Bovidae        Ammotragus  lervia       
##  7 Artiodactyla Bovidae        Antidorcas  marsupialis  
##  8 Artiodactyla Bovidae        Antilope    cervicapra   
##  9 Artiodactyla Bovidae        Bison       bison        
## 10 Artiodactyla Bovidae        Bison       bonasus      
## # ℹ 1,430 more rows
```

## Other

Here are two examples of code that are super helpful to have in your bag of tricks.

Imported data frames often have a mix of lower and uppercase column names. Use `toupper()` or `tolower()` to fix this issue. I always try to use lowercase to keep things consistent.


```r
toupper(colnames(mammals))
```

```
##  [1] "ORDER"            "FAMILY"           "GENUS"            "SPECIES"         
##  [5] "MASS"             "GESTATION"        "NEWBORN"          "WEANING"         
##  [9] "WEAN_MASS"        "AFR"              "MAX_LIFE"         "LITTER_SIZE"     
## [13] "LITTERS_PER_YEAR"
```


```r
tolower(colnames(mammals))
```

```
##  [1] "order"            "family"           "genus"            "species"         
##  [5] "mass"             "gestation"        "newborn"          "weaning"         
##  [9] "wean_mass"        "afr"              "max_life"         "litter_size"     
## [13] "litters_per_year"
```

When naming columns, blank spaces are often added (don't do this, please). Here is a trick to remove these.


```r
#select_all(mammals, ~str_replace(., " ", "_"))
```

## That's it! Let's take a break and then move on to part 2!

--\>[Home](https://jmledford3115.github.io/datascibiol/)
