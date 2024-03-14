---
title: "Homework 13"
author: "Kassidy Yu"
date: "2024-03-14"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
library(tidyverse)
library(shiny)
library(shinydashboard)
library(ggplot2)
```

## Data
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

```r
UC_admit <- read_csv("data/UC_admit.csv")
```

```
## Rows: 2160 Columns: 6
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Campus, Category, Ethnicity, Perc FR
## dbl (2): Academic_Yr, FilteredCountFR
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  

```r
glimpse(UC_admit)
```

```
## Rows: 2,160
## Columns: 6
## $ Campus          <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis", …
## $ Academic_Yr     <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018, …
## $ Category        <chr> "Applicants", "Applicants", "Applicants", "Applicants"…
## $ Ethnicity       <chr> "International", "Unknown", "White", "Asian", "Chicano…
## $ `Perc FR`       <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.35…
## $ FilteredCountFR <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, 15…
```

```r
str(UC_admit) #"Unknown" ethnicity
```

```
## spc_tbl_ [2,160 × 6] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ Campus         : chr [1:2160] "Davis" "Davis" "Davis" "Davis" ...
##  $ Academic_Yr    : num [1:2160] 2019 2019 2019 2019 2019 ...
##  $ Category       : chr [1:2160] "Applicants" "Applicants" "Applicants" "Applicants" ...
##  $ Ethnicity      : chr [1:2160] "International" "Unknown" "White" "Asian" ...
##  $ Perc FR        : chr [1:2160] "21.16%" "2.51%" "18.39%" "30.76%" ...
##  $ FilteredCountFR: num [1:2160] 16522 1959 14360 24024 17526 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Campus = col_character(),
##   ..   Academic_Yr = col_double(),
##   ..   Category = col_character(),
##   ..   Ethnicity = col_character(),
##   ..   `Perc FR` = col_character(),
##   ..   FilteredCountFR = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```


**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**  

```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Admissions Dashboard"),
  dashboardSidebar(
    selectInput("year", "Select Year:", choices = unique(UC_admit$Academic_Yr)),
    selectInput("campus", "Select Campus:", choices = unique(UC_admit$Campus)),
    selectInput("category", "Select Category:", choices = unique(UC_admit$Category))
  ),
  dashboardBody(
    fluidRow(
      box(
        title = "Admissions by Ethnicity",
        status = "primary",
        solidHeader = TRUE,
        plotOutput("admissions_plot")
      )
    )
  )
)

server <- function(input, output) {
  output$admissions_plot <- renderPlot({
    UC_admit %>%
      filter(Academic_Yr == input$year,
             Campus == input$campus,
             Category == input$category,
             Ethnicity != "All") %>%
    ggplot(aes(x = Ethnicity, y = FilteredCountFR, fill = Ethnicity)) +
      geom_bar(stat = "identity") +
      labs(title = NULL,
           x = NULL,
           y = "Count",
           fill = "Ethnicity")+
      coord_flip()+
      theme_minimal()
  })
}

shinyApp(ui, server)
```

```{=html}
<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>
```


**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**

```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Admissions Dashboard"),
  dashboardSidebar(
    selectInput("campus", "Select Campus:", choices = unique(UC_admit$Campus)),
    selectInput("category", "Select Category:", choices = unique(UC_admit$Category)),
    selectInput("ethnicity", "Select Ethnicity:", choices = unique(UC_admit$Ethnicity))
  ),
  dashboardBody(
    fluidRow(
      box(
        title = "Admissions Over the Years",
        status = "primary",
        solidHeader = TRUE,
        plotOutput("enrollments_plot")
      )
    )
  )
)

server <- function(input, output) {
  output$enrollments_plot <- renderPlot({
    UC_admit %>%
      mutate(Academic_Yr = as.factor(Academic_Yr)) %>%
      filter(Campus == input$campus,
             Category == input$category,
             Ethnicity == input$ethnicity) %>%
    ggplot(aes(x = Academic_Yr, y = FilteredCountFR, fill = Academic_Yr)) +
      geom_bar(stat = "identity", position = "dodge") +
      labs(title = NULL,
           x = NULL,
           y = "Count",
           fill = "Academic Year")+
      coord_flip()+
      theme_minimal()
  })
}

shinyApp(ui, server)
```

```{=html}
<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
