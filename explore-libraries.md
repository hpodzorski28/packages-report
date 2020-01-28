explore-libraries.R
================
hpodzorski29
2020-01-27

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "C:/Users/hpodzorski29/Documents/R/win-library/3.6"
    ## [2] "C:/Program Files/R/R-3.6.2/library"

``` r
.Library
```

    ## [1] "C:/PROGRA~1/R/R-36~1.2/library"

Installed packages

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
pkgs <-as.data.frame(installed.packages())
## use installed.packages() to get all installed packages
## if you like working with data frame or tibble, make it so right away!
## remember to use View(), dplyr::glimpse(), or similar to inspect

## how many packages?
nrow(pkgs)
```

    ## [1] 125

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
pkgs %>% count(LibPath,Priority)
```

    ## Warning: Factor `Priority` contains implicit NA, consider using
    ## `forcats::fct_explicit_na`

    ## # A tibble: 5 x 3
    ##   LibPath                                           Priority        n
    ##   <fct>                                             <fct>       <int>
    ## 1 C:/Program Files/R/R-3.6.2/library                base           14
    ## 2 C:/Program Files/R/R-3.6.2/library                recommended    15
    ## 3 C:/Program Files/R/R-3.6.2/library                <NA>            1
    ## 4 C:/Users/hpodzorski29/Documents/R/win-library/3.6 recommended     4
    ## 5 C:/Users/hpodzorski29/Documents/R/win-library/3.6 <NA>           91

``` r
  ##   * how break down re: version of R they were built on
  
  ## for tidyverts, here are some useful patterns
  # data %>% count(var)
  # data %>% count(var1, var2)
  # data %>% count(var) %>% mutate(prop = n / sum(n))
  
  #' Reflections
  
  ## reflect on ^^ and make a few notes to yourself; inspiration
  ##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
## study package naming style (all lower case, contains '.', etc)
## use `fields` argument to installed.packages() to get more info and use it!
```
