Writing Functions
================

Load key packages.

``` r
library(tidyverse)
library(readxl)
library(rvest)
```

## Writing my first function

As an example, here’s a z-score computation.

``` r
x_vec = rnorm(n = 25, mean = 10, sd = 3.5)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.05807789 -1.08559941  0.13778254  1.38929253 -0.63484946 -0.92503761
    ##  [7]  0.57513388 -0.86221453 -0.18001851  1.25976400 -0.60331281 -0.39603796
    ## [13]  0.93580592  0.42779944 -0.57566870 -1.94511098 -0.08808018  0.40463653
    ## [19] -0.22783565 -1.22071247 -1.40813290  1.14240146  1.17618744  0.50512223
    ## [25]  2.14060729

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

    ##  [1]  0.05807789 -1.08559941  0.13778254  1.38929253 -0.63484946 -0.92503761
    ##  [7]  0.57513388 -0.86221453 -0.18001851  1.25976400 -0.60331281 -0.39603796
    ## [13]  0.93580592  0.42779944 -0.57566870 -1.94511098 -0.08808018  0.40463653
    ## [19] -0.22783565 -1.22071247 -1.40813290  1.14240146  1.17618744  0.50512223
    ## [25]  2.14060729

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

    ##  [1]  0.05807789 -1.08559941  0.13778254  1.38929253 -0.63484946 -0.92503761
    ##  [7]  0.57513388 -0.86221453 -0.18001851  1.25976400 -0.60331281 -0.39603796
    ## [13]  0.93580592  0.42779944 -0.57566870 -1.94511098 -0.08808018  0.40463653
    ## [19] -0.22783565 -1.22071247 -1.40813290  1.14240146  1.17618744  0.50512223
    ## [25]  2.14060729

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
    ## 1  10.3  3.31

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
    ## 1  9.82  4.93

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
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.66  11.7

``` r
sim_mean_sd(true_mean = 4, true_sd = 12, samp_size = 30)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.94  12.6

``` r
sim_mean_sd(30, 16, 2)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  16.0  2.35

``` r
sim_mean_sd(30)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  11.0  6.00

## Revisit LotR

``` r
fellowship_df = 
  readxl::read_excel("data/LotR_Words.xlsx", range = "B3:D6") |> 
  mutate(movie = "fellowship") |> 
  janitor::clean_names()

two_towers_df = 
  readxl::read_excel("data/LotR_Words.xlsx", range = "F3:H6") |> 
  mutate(movie = "two_towers") |> 
  janitor::clean_names()

return_king_df = 
  readxl::read_excel("data/LotR_Words.xlsx", range = "J3:L6") |> 
  mutate(movie = "return_king") |> 
  janitor::clean_names()
```

Let’s do this using a function instead.

``` r
lotr_import = function(cell_range, movie_title){
  
  movie_df = 
    readxl::read_excel("data/LotR_Words.xlsx", range = cell_range) |> 
    mutate(movie = movie_title) |> 
    janitor::clean_names() |> 
    pivot_longer(
      female:male, 
      names_to = "sex",
      values_to = "words"
    ) |> 
    select(movie, everything())
 
  return(movie_df) 
  
}

lotr_df = 
  bind_rows(
    lotr_import("B3:D6", "fellowship"),
    lotr_import("F3:H6", "two_towers"),
    lotr_import("J3:L6", "return_king")
)
```

## NSDUH

``` r
nsduh_url = "https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

nsduh_html = read_html(nsduh_url)

marj_table = 
  nsduh_html |> 
  html_table() |> 
  nth(1) |> 
  slice(-1) |> 
  mutate(drug = "marj")

cocaine_table = 
  nsduh_html |> 
  html_table() |> 
  nth(1) |> 
  slice(-1) |> 
  mutate(drug = "cocaine")

heroin_table = 
  nsduh_html |> 
  html_table() |> 
  nth(1) |> 
  slice(-1) |> 
  mutate(drug = "heroin")
```

Do this instead:

``` r
# source("source/nsduh_table_format.R")
```

``` r
nsduh_table_format = function(html, table_num, table_name) {
  
  out_table = 
    nsduh_html |> 
    html_table() |> 
    nth(table_num) |> 
    slice(-1) |> 
    mutate(drug = table_name) |> 
    select(-contains("P Value"))
  
  return(out_table)
}

# nsduh_table_format(html = nsduh_html, 1, "marj")

bind_rows(
  nsduh_table_format(html = nsduh_html, 1, "marj"),
  nsduh_table_format(html = nsduh_html, 4, "cocaine"),
  nsduh_table_format(html = nsduh_html, 5, "heroin")
)
```

    ## # A tibble: 168 × 12
    ##    State `12+(2013-2014)` `12+(2014-2015)` `12-17(2013-2014)` `12-17(2014-2015)`
    ##    <chr> <chr>            <chr>            <chr>              <chr>             
    ##  1 Tota… 12.90a           13.36            13.28b             12.86             
    ##  2 Nort… 13.88a           14.66            13.98              13.51             
    ##  3 Midw… 12.40b           12.76            12.45              12.33             
    ##  4 South 11.24a           11.64            12.02              11.88             
    ##  5 West  15.27            15.62            15.53a             14.43             
    ##  6 Alab… 9.98             9.60             9.90               9.71              
    ##  7 Alas… 19.60a           21.92            17.30              18.44             
    ##  8 Ariz… 13.69            13.12            15.12              13.45             
    ##  9 Arka… 11.37            11.59            12.79              12.14             
    ## 10 Cali… 14.49            15.25            15.03              14.11             
    ## # ℹ 158 more rows
    ## # ℹ 7 more variables: `18-25(2013-2014)` <chr>, `18-25(2014-2015)` <chr>,
    ## #   `26+(2013-2014)` <chr>, `26+(2014-2015)` <chr>, `18+(2013-2014)` <chr>,
    ## #   `18+(2014-2015)` <chr>, drug <chr>
