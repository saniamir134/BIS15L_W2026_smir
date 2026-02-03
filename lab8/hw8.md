---
title: "Homework 8"
author: "Sania Mir"
date: "2026-02-03"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and/or complete the exercises in RMarkdown. Please embed all of your code and push the final work to your repository. Your report should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run.  

## Load the libraries

``` r
library("tidyverse")
library("janitor")
#library("naniar")
options(scipen = 999)
```

## About the Data
For this assignment we are going to work with a data set from the [United Nations Food and Agriculture Organization](https://www.fao.org/fishery/en/collection/capture) on world fisheries. These data were downloaded and cleaned using the `fisheries_clean.Rmd` script.  

Load the data `fisheries_clean.csv` as a new object titled `fisheries_clean`.

``` r
fisheries_clean <- read_csv("data/fisheries_clean.csv")
```

1. Explore the data. What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables, etc.? You may use the functions that you prefer.

``` r
names(fisheries_clean)
```

```
## [1] "period"          "continent"       "geo_region"      "country"        
## [5] "scientific_name" "common_name"     "taxonomic_code"  "catch"          
## [9] "status"
```

``` r
fisheries_clean
```

```
## # A tibble: 1,055,015 × 9
##    period continent geo_region    country     scientific_name common_name       
##     <dbl> <chr>     <chr>         <chr>       <chr>           <chr>             
##  1   1950 Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  2   1951 Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  3   1952 Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  4   1953 Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  5   1954 Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  6   1955 Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  7   1956 Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  8   1957 Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  9   1958 Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
## 10   1959 Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
## # ℹ 1,055,005 more rows
## # ℹ 3 more variables: taxonomic_code <chr>, catch <dbl>, status <chr>
```

2. Convert the following variables to factors: `period`, `continent`, `geo_region`, `country`, `scientific_name`, `common_name`, `taxonomic_code`, and `status`.

``` r
fisheries_clean %>% 
  mutate(across(c(period,continent,geo_region,country,scientific_name,common_name,taxonomic_code,status), as.factor))
```

```
## # A tibble: 1,055,015 × 9
##    period continent geo_region    country     scientific_name common_name       
##    <fct>  <fct>     <fct>         <fct>       <fct>           <fct>             
##  1 1950   Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  2 1951   Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  3 1952   Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  4 1953   Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  5 1954   Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  6 1955   Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  7 1956   Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  8 1957   Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
##  9 1958   Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
## 10 1959   Asia      Southern Asia Afghanistan Osteichthyes    Freshwater fishes…
## # ℹ 1,055,005 more rows
## # ℹ 3 more variables: taxonomic_code <fct>, catch <dbl>, status <fct>
```

##3. Are there any missing values in the data? If so, which variables contain missing values and how many are missing for each variable?


4. How many countries are represented in the data?

``` r
fisheries_clean %>% 
  count(country)
```

```
## # A tibble: 249 × 2
##    country                 n
##    <chr>               <int>
##  1 Afghanistan            74
##  2 Albania              2836
##  3 Algeria              2766
##  4 American Samoa       2565
##  5 Andorra                54
##  6 Angola               4831
##  7 Anguilla              238
##  8 Antigua and Barbuda   887
##  9 Argentina            9246
## 10 Armenia               199
## # ℹ 239 more rows
```

5. The variables `common_name` and `taxonomic_code` both refer to species. How many unique species are represented in the data based on each of these variables? Are the numbers the same or different?

``` r
fisheries_clean %>% 
  count(common_name)
```

```
## # A tibble: 3,390 × 2
##    common_name                   n
##    <chr>                     <int>
##  1 Aba                          58
##  2 Abalones NEI                763
##  3 Abu mullet                   13
##  4 Abyssal smooth-head           2
##  5 Abyssal spiderfish            3
##  6 Acadian redfish              14
##  7 Acoupa weakfish              73
##  8 Adriatic sole                 8
##  9 Aesop shrimp                227
## 10 African armoured searobin    12
## # ℹ 3,380 more rows
```

``` r
fisheries_clean %>% 
  count(taxonomic_code)
```

```
## # A tibble: 3,722 × 2
##    taxonomic_code     n
##    <chr>          <int>
##  1 101103101002       1
##  2 101103101003      74
##  3 101103101004      14
##  4 101103102001      87
##  5 1011031XXXXX     143
##  6 102001002603     224
##  7 102001003401     204
##  8 1020010XXXXX     220
##  9 103102001001     111
## 10 103102001401     465
## # ℹ 3,712 more rows
```


``` r
fisheries_clean %>% 
  group_by(common_name, taxonomic_code) %>% 
  summarize(n=n())
```

```
## `summarise()` has grouped output by 'common_name'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 3,722 × 3
## # Groups:   common_name [3,390]
##    common_name               taxonomic_code     n
##    <chr>                     <chr>          <int>
##  1 Aba                       117806101001      58
##  2 Abalones NEI              3701112010XX     763
##  3 Abu mullet                165001109001      13
##  4 Abyssal smooth-head       121514007801       2
##  5 Abyssal spiderfish        145504101403       3
##  6 Acadian redfish           172525111418      14
##  7 Acoupa weakfish           182006107401      73
##  8 Adriatic sole             155220107003       8
##  9 Aesop shrimp              228927103410     227
## 10 African armoured searobin 172520103401      12
## # ℹ 3,712 more rows
```
#they are the same 

6. In 2023, what were the top five countries that had the highest overall catch?

``` r
names(fisheries_clean)
```

```
## [1] "period"          "continent"       "geo_region"      "country"        
## [5] "scientific_name" "common_name"     "taxonomic_code"  "catch"          
## [9] "status"
```


``` r
fisheries_clean %>% 
  group_by(country) %>% 
  summarize(max_catch_country=max(catch, na.rm=T)) %>% 
  slice_max(max_catch_country, n=5)
```

```
## # A tibble: 5 × 2
##   country                                      max_catch_country
##   <chr>                                                    <dbl>
## 1 Peru                                                  12277000
## 2 Japan                                                  4488411
## 3 Chile                                                  4404193
## 4 China                                                  4394586
## 5 Union of Soviet Socialist Republics [former]           3582779
```

7. In 2023, what were the top 10 most caught species? To keep things simple, assume `common_name` is sufficient to identify species. What does `NEI` stand for in some of the common names? How might this be concerning from a fisheries management perspective?

``` r
names(fisheries_clean)
```

```
## [1] "period"          "continent"       "geo_region"      "country"        
## [5] "scientific_name" "common_name"     "taxonomic_code"  "catch"          
## [9] "status"
```

``` r
fisheries_clean %>% 
  group_by(common_name) %>% 
  summarize(max_common_name=max(catch, na.rm=T)) %>% 
  slice_max(max_common_name, n=10)
```

```
## # A tibble: 10 × 2
##    common_name                    max_common_name
##    <chr>                                    <dbl>
##  1 Anchoveta(=Peruvian anchovy)          12277000
##  2 Pacific sardine                        4488411
##  3 Chilean jack mackerel                  4404193
##  4 Marine fishes NEI                      4394586
##  5 Alaska pollock(=Walleye poll.)         3582779
##  6 Capelin                                2115701
##  7 Pacific chub mackerel                  1625753
##  8 Freshwater fishes NEI                  1615758
##  9 Atlantic herring                       1461500
## 10 Marine molluscs NEI                    1381595
```

``` r
#NEI is like a tag you put if you have added species in it that are not really that specific, if you have a lot of NEI it makes it difficult to assess specificty of the fisheries
```

8. For the species that was caught the most above (not NEI), which country had the highest catch in 2023?

``` r
names(fisheries_clean)
```

```
## [1] "period"          "continent"       "geo_region"      "country"        
## [5] "scientific_name" "common_name"     "taxonomic_code"  "catch"          
## [9] "status"
```

``` r
fisheries_clean %>% 
  filter(period=="2023") %>% 
  select(country, period, catch) %>% 
  summarize(max_catch_country_2023=max(catch, na.rm=T), country=country, period= period) %>% 
  slice_max(period, n=1)
```

```
## Warning: Returning more (or less) than 1 row per `summarise()` group was deprecated in
## dplyr 1.1.0.
## ℹ Please use `reframe()` instead.
## ℹ When switching from `summarise()` to `reframe()`, remember that `reframe()`
##   always returns an ungrouped data frame and adjust accordingly.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
## # A tibble: 17,665 × 3
##    max_catch_country_2023 country     period
##                     <dbl> <chr>        <dbl>
##  1               2661523. Afghanistan   2023
##  2               2661523. Albania       2023
##  3               2661523. Albania       2023
##  4               2661523. Albania       2023
##  5               2661523. Albania       2023
##  6               2661523. Albania       2023
##  7               2661523. Albania       2023
##  8               2661523. Albania       2023
##  9               2661523. Albania       2023
## 10               2661523. Albania       2023
## # ℹ 17,655 more rows
```

9. How has fishing of this species changed over the last decade (2013-2023)? Create a  plot showing total catch by year for this species.


10. Perform one exploratory analysis of your choice. Make sure to clearly state the question you are asking before writing any code.


## Knit and Upload
Please knit your work as an .html file and upload to Canvas. Homework is due before the start of the next lab. No late work is accepted. Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  
