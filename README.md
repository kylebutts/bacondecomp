
[![](https://www.r-pkg.org/badges/version/bacondecomp?color=green)](https://cran.r-project.org/package=bacondecomp)
![](https://github.com/evanjflack/bacondecomp/workflows/R-CMD-check/badge.svg)
[![Build
Status](https://travis-ci.com/evanjflack/bacondecomp.svg?branch=master)](https://travis-ci.com/evanjflack/bacondecomp)
[![Coverage
status](https://codecov.io/gh/evanjflack/bacondecomp/branch/master/graph/badge.svg)](https://codecov.io/github/evanjflack/bacondecomp?branch=master)
[![Example Jupyter
Notebook](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/evanjflack/bacondecomp/master?filepath=index.ipynb)

<!-- README.md is generated from README.Rmd. Please edit that file -->

# bacondecomp

`bacondecomp` is a package with tools to perform the Goodman-Bacon
decomposition for differences-in-differences with variation in treatment
timing. The decomposition can be done with and without time-varying
covariates.

## Installation

You can install `bacondecomp 0.1.2` from CRAN:

``` r
install.packages("bacondecomp")
```

You can install the development version of `bacondecomp` from GitHub:

``` r
library(devtools)
install_github("evanjflack/bacondecomp")
```

## Functions

-   `bacon()`: calculates all 2x2 differences-in-differences estimates
    and weights for the Bacon-Goodman decomposition.

## Data

-   `math_refom`: Aggregated data from Goodman (2019, JOLE)
-   `castle`: Data from Cheng and Hoekstra (2013, JHR)
-   `divorce:` Data from Stevenson and Wolfers (2006, QJE)

## Example

This is a basic example which shows you how to use the bacon() function
to decompose the two-way fixed effects estimate of the effect of an
education reform on future earnings following Goodman (2019, JOLE).

``` r
library(bacondecomp)

df_bacon <- bacon(incearn_ln ~ reform_math,
                  data = bacondecomp::math_reform,
                  id_var = "state",
                  time_var = "class")

# All 2x2 Comparisons
head(df_bacon)
#>    treated untreated    estimate      weight                     type
#> 2     1987      1985  0.04575585 0.031655309 Later vs Earlier Treated
#> 4     1986      1985  0.11390411 0.001978457 Later vs Earlier Treated
#> 5     1984      1985  0.12012908 0.001978457 Earlier vs Later Treated
#> 6     1985      1987  0.01021379 0.039569136 Earlier vs Later Treated
#> 9     1986      1987 -0.09374495 0.005440756 Earlier vs Later Treated
#> 10    1984      1987  0.09784857 0.013354583 Earlier vs Later Treated

# Summary of Early vs. Later, Later vs. Earlier, and Treated vs. Untreated
bacon_summary(df_bacon)
#>                       type  weight  avg_est
#> 1 Earlier vs Later Treated 0.06353  0.02868
#> 2 Later vs Earlier Treated 0.05265  0.03375
#> 3     Treated vs Untreated 0.88382 -0.00129
```

``` r
library(ggplot2)

ggplot(df_bacon) +
  aes(x = weight, y = estimate, shape = factor(type)) +
  geom_point() +
  geom_hline(yintercept = 0) + 
  theme_minimal() +
  labs(x = "Weight", y = "Estimate", shape = "Type")
```

<img src="man/figures/README-example-plot-1.png" width="100%" />

## References

Goodman-Bacon, Andrew. 2018. “Difference-in-Differences with Variation
in Treatment Timing.” National Bureau of Economic Research Working Paper
Series No. 25018. doi: 10.3386/w25018.

[Paper
Link](https://cdn.vanderbilt.edu/vu-my/wp-content/uploads/sites/2318/2019/07/29170757/ddtiming_7_29_2019.pdf)
