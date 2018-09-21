Template for analysis in R
================
rasi
20 September, 2018

-   [Introduction](#introduction)
-   [Import libraries](#import-libraries)
-   [Import data](#import-data)
-   [Transform data](#transform-data)
-   [Plot data](#plot-data)

Introduction
------------

-   This is an example outline of how we will analyze data inside RStudio.
-   Use the `Run` followed by `Run All` button in the tool bar above to view the results of running this script by scrolling below.

Import libraries
----------------

``` r
# standard analysis and plotting functions, includes dplyr, ggplot2 
library(tidyverse)
```

Import data
-----------

``` r
data <- read_csv("https://github.com/tidyverse/readr/raw/master/inst/extdata/mtcars.csv")
```

Transform data
--------------

``` r
processed_data <- data %>% 
  filter(am == 1) %>%
  select(mpg, qsec, carb)

processed_data 
```

    ## # A tibble: 13 x 3
    ##      mpg  qsec  carb
    ##    <dbl> <dbl> <int>
    ##  1  21    16.5     4
    ##  2  21    17.0     4
    ##  3  22.8  18.6     1
    ##  4  32.4  19.5     1
    ##  5  30.4  18.5     2
    ##  6  33.9  19.9     1
    ##  7  27.3  18.9     1
    ##  8  26    16.7     2
    ##  9  30.4  16.9     2
    ## 10  15.8  14.5     4
    ## 11  19.7  15.5     6
    ## 12  15    14.6     8
    ## 13  21.4  18.6     2

Plot data
---------

``` r
processed_data %>% 
  gather(statistic, value, -carb) %>% 
  ggplot(aes(x = carb, y = value, color = statistic)) +
  facet_wrap(~ statistic, ncol = 2) +
  geom_jitter(width = 0)
```

![](example_rmarkdown_document_files/figure-markdown_github/example_plot-1.png)
