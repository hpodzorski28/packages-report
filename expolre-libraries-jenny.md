expolre-libraries-jenny.R
================
hpodzorski29
2020-01-27

Which libraries does R search for packages?

``` r
## how jenny might do this in a first exploration
## purposely leaving a few things to change later!

print("hello")
```

    ## [1] "hello"

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "C:/Users/hpodzorski29/Documents/R/win-library/3.6"
    ## [2] "C:/Program Files/R/R-3.6.2/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "C:/PROGRA~1/R/R-36~1.2/library"

``` r
identical(.Library, .libPaths()[2])
```

    ## [1] FALSE

``` r
## Huh? Maybe this is an symbolic link issue?
library(fs)
identical(path_real(.Library), path_real(.libPaths()[2]))
```

    ## [1] TRUE

Installed packages

``` r
library(tidyverse)
```

    ## -- Attaching packages ----------------------------------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.2.1     v purrr   0.3.3
    ## v tibble  2.1.3     v dplyr   0.8.3
    ## v tidyr   1.0.2     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## -- Conflicts -------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 125

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 5 x 3
    ##   LibPath                                           Priority        n
    ##   <chr>                                             <chr>       <int>
    ## 1 C:/Program Files/R/R-3.6.2/library                base           14
    ## 2 C:/Program Files/R/R-3.6.2/library                recommended    15
    ## 3 C:/Program Files/R/R-3.6.2/library                <NA>            1
    ## 4 C:/Users/hpodzorski29/Documents/R/win-library/3.6 recommended     4
    ## 5 C:/Users/hpodzorski29/Documents/R/win-library/3.6 <NA>           91

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n  prop
    ##   <chr>            <int> <dbl>
    ## 1 no                  55  0.44
    ## 2 yes                 65  0.52
    ## 3 <NA>                 5  0.04

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   Built     n  prop
    ##   <chr> <int> <dbl>
    ## 1 3.6.0     3 0.024
    ## 2 3.6.1     2 0.016
    ## 3 3.6.2   120 0.96

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
all_default_pkgs <- list.files(.Library)
all_br_pkgs <- ipt %>%
  filter(Priority %in% c("base", "recommended")) %>%
  pull(Package)
setdiff(all_default_pkgs, all_br_pkgs)
```

    ## [1] "translations"

``` r
## study package naming style (all lower case, contains '.', etc

## use `fields` argument to installed.packages() to get more info and use it!
ipt2 <- installed.packages(fields = "URL") %>%
  as_tibble()
ipt2 %>%
  mutate(github = grepl("github", URL)) %>%
  count(github) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   github     n  prop
    ##   <lgl>  <int> <dbl>
    ## 1 FALSE     45  0.36
    ## 2 TRUE      80  0.64
