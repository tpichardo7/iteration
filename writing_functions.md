Writing Functions
================

Load key packages.

``` r
library(tidyverse)
library(rvest)
```

## Writing my first function

As an example, here’s a z-score computation.

``` r
x_vec = rnorm(n = 25, mean = 10, sd = 3.5)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.69968743  0.78552853  0.43676639 -0.92345356  0.79444431 -0.72427269
    ##  [7] -1.06298597  1.38772788  0.32241299  0.31957302 -0.78240026  0.43988883
    ## [13]  1.52537095  0.04107432  0.17103049 -1.99052968  0.98157872  0.95879280
    ## [19]  0.32871294  0.24454088 -1.58004049 -0.75862896  0.38656485  1.25679195
    ## [25] -1.85880084

Now I’ll write a function to do this.

``` r
z_scores = function(x) {
  
  if(!is.numeric(x)) {
    stop("x needs to be numeric")
  }
  
  if(length(x) < 5) {
    stop("you need at least five numbers to comput the z score")
  }
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}

z_scores(x = x_vec)
```

    ##  [1] -0.69968743  0.78552853  0.43676639 -0.92345356  0.79444431 -0.72427269
    ##  [7] -1.06298597  1.38772788  0.32241299  0.31957302 -0.78240026  0.43988883
    ## [13]  1.52537095  0.04107432  0.17103049 -1.99052968  0.98157872  0.95879280
    ## [19]  0.32871294  0.24454088 -1.58004049 -0.75862896  0.38656485  1.25679195
    ## [25] -1.85880084

Does this always work?

``` r
z_scores(x = 3)
```

    ## Error in z_scores(x = 3): you need at least five numbers to comput the z score

``` r
z_scores(x = c("my", "name", "is", "Tamara"))
```

    ## Error in z_scores(x = c("my", "name", "is", "Tamara")): x needs to be numeric

``` r
z_scores(x = x_vec)
```

    ##  [1] -0.69968743  0.78552853  0.43676639 -0.92345356  0.79444431 -0.72427269
    ##  [7] -1.06298597  1.38772788  0.32241299  0.31957302 -0.78240026  0.43988883
    ## [13]  1.52537095  0.04107432  0.17103049 -1.99052968  0.98157872  0.95879280
    ## [19]  0.32871294  0.24454088 -1.58004049 -0.75862896  0.38656485  1.25679195
    ## [25] -1.85880084

## A new function

``` r
mean_and_sd = function(x) {
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  out_df = 
    tibble(
      mean = mean_x,
      sd = sd_x
    )
  
  return(out_df)
}

mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.8  3.31

## Check stuff using a simulation

``` r
sim_df = 
  tibble(
    x = rnorm(30, 10, 5)
  )

sim_df |> 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.48  5.93

Simulation function to check sample mean and standard deviation

``` r
sim_mean_sd = function(samp_size, true_mean = 10, true_sd = 5) {
  
  sim_df = 
  tibble(
    x = rnorm(samp_size, true_mean, true_sd)
  )

out_df = 
  sim_df |> 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
  
  return(out_df)
}

sim_mean_sd(samp_size = 30, true_mean = 4, true_sd = 12)
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.235  10.2

``` r
sim_mean_sd(true_mean = 4, true_sd = 12, samp_size = 30)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.11  11.4

``` r
sim_mean_sd(30, 16, 2)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  16.2  2.04

``` r
sim_mean_sd(30)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  11.5  4.53
