---
title: "Lab 4 Homework"
author: "Kassidy Yu"
date: "2024-01-23"
output:
  html_document: 
    theme: spacelab
    keep_md: true
---



## Instructions

Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!

## Load the tidyverse


```r
library(tidyverse)
```

## Data

For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.

**Database of vertebrate home range sizes.**\
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. <http://dx.doi.org/10.1086/682070>.\
Data: <http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1>

**1. Load the data into a new object called `homerange`.**


```r
homerange <- read_csv("data/Tamburelloetal_HomeRangeDatabase.csv")
```

```
## Rows: 569 Columns: 24
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (16): taxon, common.name, class, order, family, genus, species, primarym...
## dbl  (8): mean.mass.g, log10.mass, mean.hra.m2, log10.hra, dimension, preyma...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**


```r
dim(homerange)
```

```
## [1] 569  24
```


```r
str(homerange)
```

```
## spc_tbl_ [569 × 24] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ taxon                     : chr [1:569] "lake fishes" "river fishes" "river fishes" "river fishes" ...
##  $ common.name               : chr [1:569] "american eel" "blacktail redhorse" "central stoneroller" "rosyside dace" ...
##  $ class                     : chr [1:569] "actinopterygii" "actinopterygii" "actinopterygii" "actinopterygii" ...
##  $ order                     : chr [1:569] "anguilliformes" "cypriniformes" "cypriniformes" "cypriniformes" ...
##  $ family                    : chr [1:569] "anguillidae" "catostomidae" "cyprinidae" "cyprinidae" ...
##  $ genus                     : chr [1:569] "anguilla" "moxostoma" "campostoma" "clinostomus" ...
##  $ species                   : chr [1:569] "rostrata" "poecilura" "anomalum" "funduloides" ...
##  $ primarymethod             : chr [1:569] "telemetry" "mark-recapture" "mark-recapture" "mark-recapture" ...
##  $ N                         : chr [1:569] "16" NA "20" "26" ...
##  $ mean.mass.g               : num [1:569] 887 562 34 4 4 ...
##  $ log10.mass                : num [1:569] 2.948 2.75 1.531 0.602 0.602 ...
##  $ alternative.mass.reference: chr [1:569] NA NA NA NA ...
##  $ mean.hra.m2               : num [1:569] 282750 282.1 116.1 125.5 87.1 ...
##  $ log10.hra                 : num [1:569] 5.45 2.45 2.06 2.1 1.94 ...
##  $ hra.reference             : chr [1:569] "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 ...
##  $ realm                     : chr [1:569] "aquatic" "aquatic" "aquatic" "aquatic" ...
##  $ thermoregulation          : chr [1:569] "ectotherm" "ectotherm" "ectotherm" "ectotherm" ...
##  $ locomotion                : chr [1:569] "swimming" "swimming" "swimming" "swimming" ...
##  $ trophic.guild             : chr [1:569] "carnivore" "carnivore" "carnivore" "carnivore" ...
##  $ dimension                 : num [1:569] 3 2 2 2 2 2 2 2 2 2 ...
##  $ preymass                  : num [1:569] NA NA NA NA NA NA 1.39 NA NA NA ...
##  $ log10.preymass            : num [1:569] NA NA NA NA NA ...
##  $ PPMR                      : num [1:569] NA NA NA NA NA NA 530 NA NA NA ...
##  $ prey.size.reference       : chr [1:569] NA NA NA NA ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   taxon = col_character(),
##   ..   common.name = col_character(),
##   ..   class = col_character(),
##   ..   order = col_character(),
##   ..   family = col_character(),
##   ..   genus = col_character(),
##   ..   species = col_character(),
##   ..   primarymethod = col_character(),
##   ..   N = col_character(),
##   ..   mean.mass.g = col_double(),
##   ..   log10.mass = col_double(),
##   ..   alternative.mass.reference = col_character(),
##   ..   mean.hra.m2 = col_double(),
##   ..   log10.hra = col_double(),
##   ..   hra.reference = col_character(),
##   ..   realm = col_character(),
##   ..   thermoregulation = col_character(),
##   ..   locomotion = col_character(),
##   ..   trophic.guild = col_character(),
##   ..   dimension = col_double(),
##   ..   preymass = col_double(),
##   ..   log10.preymass = col_double(),
##   ..   PPMR = col_double(),
##   ..   prey.size.reference = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**


```r
homerange$taxon <- as.factor(homerange$taxon)
homerange$order <- as.factor(homerange$order)
```


```r
levels(homerange$taxon)
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```


```r
levels(homerange$order)
```

```
##  [1] "accipitriformes"       "afrosoricida"          "anguilliformes"       
##  [4] "anseriformes"          "apterygiformes"        "artiodactyla"         
##  [7] "caprimulgiformes"      "carnivora"             "charadriiformes"      
## [10] "columbidormes"         "columbiformes"         "coraciiformes"        
## [13] "cuculiformes"          "cypriniformes"         "dasyuromorpha"        
## [16] "dasyuromorpia"         "didelphimorphia"       "diprodontia"          
## [19] "diprotodontia"         "erinaceomorpha"        "esociformes"          
## [22] "falconiformes"         "gadiformes"            "galliformes"          
## [25] "gruiformes"            "lagomorpha"            "macroscelidea"        
## [28] "monotrematae"          "passeriformes"         "pelecaniformes"       
## [31] "peramelemorphia"       "perciformes"           "perissodactyla"       
## [34] "piciformes"            "pilosa"                "proboscidea"          
## [37] "psittaciformes"        "rheiformes"            "roden"                
## [40] "rodentia"              "salmoniformes"         "scorpaeniformes"      
## [43] "siluriformes"          "soricomorpha"          "squamata"             
## [46] "strigiformes"          "struthioniformes"      "syngnathiformes"      
## [49] "testudines"            "tinamiformes"          "tetraodontiformes\xa0"
```

**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**


```r
taxa <- select(homerange, taxon:species)
```

**5. The variable `taxon` identifies the common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**


```r
table(homerange$taxon)
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```

**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**


```r
table(homerange$trophic.guild)
```

```
## 
## carnivore herbivore 
##       342       227
```

**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**


```r
carnivores <- filter(homerange, trophic.guild == "carnivore")
herbivores <- filter(homerange, trophic.guild == "herbivore")
```

**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.** Carnivores.


```r
mean(carnivores$mean.hra.m2, na.rm=T)
```

```
## [1] 13039918
```


```r
mean(herbivores$mean.hra.m2, na.rm=T)
```

```
## [1] 34137012
```

**9. Make a new dataframe `owls` that is limited to the mean mass, log10 mass, family, genus, and species of owls in the database. Which is the smallest owl? What is its common name? Do a little bit of searching online to see what you can learn about this species and provide a link below**


```r
owls <- filter(homerange, order == "strigiformes")
owls <- select(owls, family, genus, species, mean.mass.g, log10.mass)
owls
```

```
## # A tibble: 9 × 5
##   family    genus      species     mean.mass.g log10.mass
##   <chr>     <chr>      <chr>             <dbl>      <dbl>
## 1 strigidae aegolius   funereus          119         2.08
## 2 strigidae asio       otus              252         2.40
## 3 strigidae athene     noctua            156.        2.19
## 4 strigidae bubo       bubo             2191         3.34
## 5 strigidae bubo       virginianus      1510         3.18
## 6 strigidae glaucidium passerinum         61.3       1.79
## 7 strigidae nyctea     scandiaca        1920         3.28
## 8 strigidae strix      aluco             519         2.72
## 9 tytonidae tyto       alba              285         2.45
```


```r
filter(owls, mean.mass.g == min(owls$mean.mass.g))
```

```
## # A tibble: 1 × 5
##   family    genus      species    mean.mass.g log10.mass
##   <chr>     <chr>      <chr>            <dbl>      <dbl>
## 1 strigidae glaucidium passerinum        61.3       1.79
```

[Eurasian pygmy owl](<https://planetofbirds.com/strigiformes-strigidae-eurasian-pygmy-owl-glaucidium-passerinum>)

**10. As measured by the data, which bird species has the largest homerange? Show all of your work, please. Look this species up online and tell me about it!**.


```r
birds <- filter(homerange, taxon == "birds")
filter(birds, mean.hra.m2 == max(birds$mean.hra.m2))
```

```
## # A tibble: 1 × 24
##   taxon common.name class order         family genus species primarymethod N    
##   <fct> <chr>       <chr> <fct>         <chr>  <chr> <chr>   <chr>         <chr>
## 1 birds caracara    aves  falconiformes falco… cara… cheriw… telemetry     26   
## # ℹ 15 more variables: mean.mass.g <dbl>, log10.mass <dbl>,
## #   alternative.mass.reference <chr>, mean.hra.m2 <dbl>, log10.hra <dbl>,
## #   hra.reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic.guild <chr>, dimension <dbl>, preymass <dbl>, log10.preymass <dbl>,
## #   PPMR <dbl>, prey.size.reference <chr>
```
The crested caracara is also known as the Mexican eagle. They are found in any open or semi-open habitat, even near humans - they just avoid dense forests.

## Push your final code to GitHub!

Please be sure that you check the `keep md` file in the knit preferences.
