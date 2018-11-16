<!-- README.md is generated from README.Rmd. Please edit that file -->
STAT 545: Homework 07
=====================

### By: Stephen Chignell

Welcome to the repository! Here you'll find all the files and associated material for [Howework 7](http://stat545.com/Classroom/assignments/hw07/hw07.html).

For this assignment, I chose to add a function to the `foofactors` package by Jenny Bryan (original is [here](https://github.com/jennybc/foofactors)).

I sometimes have to work with messy data where there are typos and inconsistencies as a result of data entry errors, so for HW07, I wrote a function called `fclean` which goes through all of the factor levels and trims extra white space while converting each word to title case.

-   Access my modifed (forked) version of the `foofactors` package [here](https://github.com/schignel/foofactors)

### Overview of `foofactors`

**NOTE: This is a toy package created for expository purposes. It is not meant to actually be useful. If you want a package for factor handling, please see [forcats](https://cran.r-project.org/package=forcats).**

Factors are a very useful type of variable in R, but they can also drive you nuts. This package provides some helper functions for the care and feeding of factors.

### Installation

``` r
devtools::install_github("schignel/foofactors")
```

### Quick demo

Binding two factors via `fbind()`:

``` r
library(foofactors)
a <- factor(c("character", "hits", "your", "eyeballs"))
b <- factor(c("but", "integer", "where it", "counts"))
```

Simply catenating two factors leads to a result that most don't expect.

``` r
c(a, b)
#> [1] 1 3 4 2 1 3 4 2
```

The `fbind()` function glues two factors together and returns factor.

``` r
fbind(a, b)
#> [1] character hits      your      eyeballs  but       integer   where it 
#> [8] counts   
#> Levels: but character counts eyeballs hits integer where it your
```

Often we want a table of frequencies for the levels of a factor. The base `table()` function returns an object of class `table`, which can be inconvenient for downstream work. Processing with `as.data.frame()` can be helpful but it's a bit clunky.

``` r
set.seed(1234)
x <- factor(sample(letters[1:5], size = 100, replace = TRUE))
table(x)
#> x
#>  a  b  c  d  e 
#> 25 26 17 17 15
as.data.frame(table(x))
#>   x Freq
#> 1 a   25
#> 2 b   26
#> 3 c   17
#> 4 d   17
#> 5 e   15
```

The `freq_out()` function returns a frequency table as a well-named `tbl_df`:

``` r
freq_out(x)
#> # A tibble: 5 x 2
#>   x         n
#>   <fct> <int>
#> 1 a        25
#> 2 b        26
#> 3 c        17
#> 4 d        17
#> 5 e        15
```

Human error during data entry often leads to typos and inconsistent spacing and capitlization. This is particularly true when there are multiple individuals working on entering the same categorical data.

``` r
production <- factor(c("  HIGH production", "MED production  ",  "MED production  ", " low Production"))
production
#> [1]   HIGH production MED production    MED production     low Production  
#> Levels:   HIGH production  low Production MED production
```

This can be fixed by identifying individual inconsistencies after factor created, but is time consuming and cumbersome.

The `fclean` function will go through all of the factor levels and trim extra white space and convert to title case using the `stringr` package.

``` r
fclean(production)
#> [1] High Production Med Production  Med Production  Low Production 
#> Levels: High Production Low Production Med Production
```
