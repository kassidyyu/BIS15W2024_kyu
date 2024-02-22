---
title: "Homework 10"
author: "Kassidy Yu"
date: "2024-02-22"
output:
  html_document: 
    theme: spacelab
    keep_md: true
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(naniar)
```


```r
options(scipen=999)
```

## Desert Ecology
For this assignment, we are going to use a modified data set on [desert ecology](http://esapubs.org/archive/ecol/E090/118/). The data are from: S. K. Morgan Ernest, Thomas J. Valone, and James H. Brown. 2009. Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA. Ecology 90:1708.

```r
deserts <- read_csv("data/surveys_complete.csv") %>% clean_names()
```

```
## Rows: 34786 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (6): species_id, sex, genus, species, taxa, plot_type
## dbl (7): record_id, month, day, year, plot_id, hindfoot_length, weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?  

```r
glimpse(deserts)
```

```
## Rows: 34,786
## Columns: 13
## $ record_id       <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,…
## $ month           <dbl> 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, …
## $ day             <dbl> 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16…
## $ year            <dbl> 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, …
## $ plot_id         <dbl> 2, 3, 2, 7, 3, 1, 2, 1, 1, 6, 5, 7, 3, 8, 6, 4, 3, 2, …
## $ species_id      <chr> "NL", "NL", "DM", "DM", "DM", "PF", "PE", "DM", "DM", …
## $ sex             <chr> "M", "M", "F", "M", "M", "M", "F", "M", "F", "F", "F",…
## $ hindfoot_length <dbl> 32, 33, 37, 36, 35, 14, NA, 37, 34, 20, 53, 38, 35, NA…
## $ weight          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ genus           <chr> "Neotoma", "Neotoma", "Dipodomys", "Dipodomys", "Dipod…
## $ species         <chr> "albigula", "albigula", "merriami", "merriami", "merri…
## $ taxa            <chr> "Rodent", "Rodent", "Rodent", "Rodent", "Rodent", "Rod…
## $ plot_type       <chr> "Control", "Long-term Krat Exclosure", "Control", "Rod…
```

```r
#the data seem tidy, but some species are labeled "sp.", seemingly indicating an unknown species
```

```r
deserts <- deserts %>%
  mutate(species = ifelse(species == "sp.", NA, species))
```

2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?

```r
deserts %>%
  summarize(num_genera = n_distinct(genus), num_species = n_distinct(species), num_total = n())
```

```
## # A tibble: 1 × 3
##   num_genera num_species num_total
##        <int>       <int>     <int>
## 1         26          40     34786
```

```r
deserts %>%
  group_by(species) %>%
  summarize(count = n()) %>%
  filter(count == max(count) | count == min(count)) %>%
  arrange(-count)
```

```
## # A tibble: 7 × 2
##   species      count
##   <chr>        <int>
## 1 merriami     10596
## 2 clarki           1
## 3 scutalatus       1
## 4 tereticaudus     1
## 5 tigris           1
## 6 uniparens        1
## 7 viridis          1
```

```r
#most: merriami
#least: clarki, scutalatus, tereticaudus, tigris, uniparens, viridis
```


3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.

```r
deserts %>%
  tabyl(taxa)
```

```
##     taxa     n      percent
##     Bird   450 0.0129362387
##   Rabbit    75 0.0021560398
##  Reptile    14 0.0004024608
##   Rodent 34247 0.9845052607
```

```r
deserts %>%
  ggplot(aes(x=taxa))+
  geom_bar()+
  labs(title = "Proportions of Taxa", x = "Taxa", y = "Number of Observations")+
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5))
```

![](lab11_hw10_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`

```r
deserts %>%
  ggplot(aes(x=taxa, fill = plot_type))+
  geom_bar()+
  labs(title = "Proportions of Plot Type Samples Within Taxa", x = "Taxa", y = "Number of Observations", fill = "Plot Type")+
  theme(plot.title = element_text(hjust = 0.5))
```

![](lab11_hw10_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.

```r
deserts %>%
  group_by(species) %>%
  select(species, weight) %>%
  filter(!is.na(weight), !is.na(species)) %>%
  ggplot(aes(x=species, y=weight))+
  geom_boxplot()+
  coord_flip()+
  labs(title = "Weight Distribution by Species", x = "Species", y = "Weight")
```

![](lab11_hw10_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

6. Add another layer to your answer from #5 using `geom_point` to get an idea of how many measurements were taken for each species.

```r
deserts %>%
  group_by(species) %>%
  select(species, weight) %>%
  filter(!is.na(weight), !is.na(species)) %>%
  ggplot(aes(x=species, y=weight))+
  geom_boxplot()+
  geom_point()+
  coord_flip()+
  labs(title = "Weight Distribution by Species", x = "Species", y = "Weight")
```

![](lab11_hw10_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?

```r
deserts %>%
  group_by(year) %>%
  filter(species == "merriami") %>%
  summarize(counts = n()) %>%
  ggplot(aes(x = year, y = counts))+
  geom_col()+
  labs(title = "Observations of Dipodomys merrami Over the Years",
       x = "Year", y = "Number of Observations")+
  theme(plot.title = element_text(hjust = 0.5))
```

![](lab11_hw10_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.

```r
deserts %>%
  filter(!is.na(weight), !is.na(hindfoot_length)) %>%
  ggplot(aes(x = weight, y = hindfoot_length, alpha(0.25)))+
  geom_jitter(size = 0.25)+
  labs(title = "Weight vs Hindfoot Length", x = "Weight", y = "Hindfoot Length")+
  theme(plot.title = element_text(hjust=0.5))
```

![](lab11_hw10_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.

```r
deserts %>%
  group_by(species) %>%
  filter(!is.na(species), !is.na(weight)) %>%
  summarize(mean_weight = mean(weight)) %>%
  arrange(-mean_weight)
```

```
## # A tibble: 21 × 2
##    species      mean_weight
##    <chr>              <dbl>
##  1 albigula           159. 
##  2 spectabilis        120. 
##  3 spilosoma           93.5
##  4 hispidus            65.6
##  5 fulviventer         58.9
##  6 ochrognathus        55.4
##  7 ordii               48.9
##  8 merriami            43.2
##  9 baileyi             31.7
## 10 leucogaster         31.6
## # ℹ 11 more rows
```


```r
deserts <- deserts %>%
  mutate(weight_to_hindfoot_length = weight/hindfoot_length)
```


```r
deserts %>%
  filter(species == "albigula" | species == "spectabilis", sex != "NA") %>%
  ggplot(aes(x = species, y = weight_to_hindfoot_length, fill = sex))+
  geom_boxplot()+
  labs(title = "Heaviest Species' Weight to Hindfoot Length Ratio by Sex", x = "Species", y = "Ratio of Weight to Hindfoot Length", fill = "Sex")
```

```
## Warning: Removed 567 rows containing non-finite values (`stat_boxplot()`).
```

![](lab11_hw10_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.

```r
deserts %>%
  filter(!is.na(genus), !is.na(hindfoot_length), !is.na(sex))%>%
  group_by(genus) %>%
  ggplot(aes(x = genus, y = hindfoot_length, fill = sex))+
  geom_col(position = "dodge")+
  coord_flip()+
  labs(title = "Hindfoot Length Distributions Across Genera by Sex", x = "Genus", y = "Hindfoot Length", fill = "Sex")
```

![](lab11_hw10_files/figure-html/unnamed-chunk-18-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
