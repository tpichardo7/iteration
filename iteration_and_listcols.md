Iteration and List Columns
================

``` r
knitr::opts_chunk$set(
        echo = TRUE,
        warning = FALSE,
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)

theme_set(theme_minimal() +theme(legend.position = "bottom"))

options(
  ggplot2.continuous.color = "viridis",
  ggplot2.continuous.fill = "viridis"
)

scale_color_discrete = scale_colour_viridis_d
scale_fill_discrete = scale_fill_viridis_d
```

## Here’s some lists.

``` r
l = list(
  vec_numeric = 1:4,
  unif_sample = runif(100),
  mat = matrix(1:8, nrow = 2, ncol = 4, byrow = TRUE),
  summary = summary(rnorm(1000))
)

l
```

    ## $vec_numeric
    ## [1] 1 2 3 4
    ## 
    ## $unif_sample
    ##   [1] 0.967273150 0.588691221 0.030372994 0.674701524 0.225689583 0.000363206
    ##   [7] 0.321560314 0.587287810 0.144730938 0.351926612 0.960089570 0.407831008
    ##  [13] 0.257254413 0.883151520 0.748877495 0.091618182 0.848324212 0.036213052
    ##  [19] 0.302037972 0.165044649 0.046495496 0.091874654 0.647605434 0.353400432
    ##  [25] 0.285730000 0.576908186 0.887657732 0.567243600 0.555596931 0.803689557
    ##  [31] 0.464890046 0.175963028 0.170325880 0.820822614 0.582431441 0.885484402
    ##  [37] 0.770978645 0.493130020 0.787341456 0.934860939 0.797646229 0.500704261
    ##  [43] 0.060929648 0.117674815 0.327550319 0.569945927 0.025976958 0.594363675
    ##  [49] 0.151141034 0.801345711 0.809669114 0.780921695 0.265925657 0.211156972
    ##  [55] 0.440067158 0.502406765 0.284242694 0.801740510 0.348267638 0.241045417
    ##  [61] 0.001483758 0.287944967 0.570734918 0.143125252 0.489691769 0.794553640
    ##  [67] 0.527294403 0.093054353 0.355305145 0.662778833 0.084477771 0.814636086
    ##  [73] 0.018091373 0.927109336 0.609015299 0.229423528 0.870475197 0.579815004
    ##  [79] 0.014177283 0.785209170 0.510314519 0.046736710 0.821988180 0.674064748
    ##  [85] 0.105059449 0.815030931 0.612589824 0.401069251 0.321874085 0.990889460
    ##  [91] 0.438827662 0.860428379 0.637768484 0.466780410 0.339579652 0.979894381
    ##  [97] 0.772944873 0.931246497 0.990446325 0.453487888
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    5    6    7    8
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -3.30946 -0.68868 -0.05702 -0.05661  0.63302  4.09509

``` r
l$mat
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    5    6    7    8

``` r
l[["mat"]][1, 3]
```

    ## [1] 3

``` r
l[[1]]
```

    ## [1] 1 2 3 4

``` r
l[[4]]
```

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -3.30946 -0.68868 -0.05702 -0.05661  0.63302  4.09509

``` r
list_norm = 
  list(
    a = rnorm(20, 0, 5),
    b = rnorm(20, 4, 5),
    c = rnorm(20, 0, 10),
    d = rnorm(20, 4, 10)
  )

list_norm[["b"]]
```

    ##  [1]  0.8549957930  4.2831465820 18.6181804004  1.7619312920  8.1033056830
    ##  [6] -0.1232108881 -2.6360442699 13.3106595693  8.9064100006 -0.8066839044
    ## [11]  6.9099233860  7.3309710630 -0.0004266869  6.5374454599  5.5977450026
    ## [16]  3.8578599083  4.9660493497  8.1977038232 -6.6134745860  5.0116836906

Let’s reuse the function we wrote last time.

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
```

Let’s use the function to take the mean and sd of all samples

``` r
mean_and_sd(list_norm[["a"]])
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0642  4.76

``` r
mean_and_sd(list_norm[["b"]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.70  5.65

``` r
mean_and_sd(list_norm[["c"]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.03  8.64

``` r
mean_and_sd(list_norm[["d"]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.98  10.8

## Use a for loop

Create output list, and run a for loop

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```

## Do the same thing

but with `map` instead

``` r
output = map(list_norm, mean_and_sd)
```

Let’s do a couple of other things

``` r
output = 
  map(list_norm, mean_and_sd) |> 
  bind_rows()

output = map_dfr(list_norm, mean_and_sd)

output = map_dbl(list_norm, IQR)
```

## List columns

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )

listcol_df
```

    ## # A tibble: 4 × 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>  
    ## 2 b     <dbl [20]>  
    ## 3 c     <dbl [20]>  
    ## 4 d     <dbl [20]>

``` r
listcol_df |> 
  filter(name %in% c("a", "b"))
```

    ## # A tibble: 2 × 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>  
    ## 2 b     <dbl [20]>

``` r
listcol_df |> 
  select(-samp)
```

    ## # A tibble: 4 × 1
    ##   name 
    ##   <chr>
    ## 1 a    
    ## 2 b    
    ## 3 c    
    ## 4 d

``` r
listcol_df[["samp"]][["a"]]
```

    ##  [1] -8.0092680 -1.1702878  1.3559245  4.0878126 -0.1955188 -4.6746160
    ##  [7]  2.0116665  2.7949407  8.1617226  5.7628548 -1.0765265 -6.3071788
    ## [13]  9.8697544  2.1421498 -3.9237179 -1.5523440  1.8851418 -5.5462104
    ## [19] -0.2021546 -4.1303006

Compute mean and sd

``` r
mean_and_sd(listcol_df[["samp"]][["a"]])
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0642  4.76

``` r
mean_and_sd(listcol_df[["samp"]][["b"]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.70  5.65

``` r
map(listcol_df[["samp"]], mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0642  4.76
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.70  5.65
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.03  8.64
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.98  10.8

Add a list column

``` r
listcol_df |> 
  mutate(output = map(samp, mean_and_sd),
  iqr = map(samp, IQR))
```

    ## # A tibble: 4 × 4
    ##   name  samp         output           iqr         
    ##   <chr> <named list> <named list>     <named list>
    ## 1 a     <dbl [20]>   <tibble [1 × 2]> <dbl [1]>   
    ## 2 b     <dbl [20]>   <tibble [1 × 2]> <dbl [1]>   
    ## 3 c     <dbl [20]>   <tibble [1 × 2]> <dbl [1]>   
    ## 4 d     <dbl [20]>   <tibble [1 × 2]> <dbl [1]>

``` r
listcol_df |> 
  mutate(
    output = map(samp, mean_and_sd),
    iqr = map_dbl(samp, IQR)) |> 
  select(-samp) |> 
  unnest(output)
```

    ## # A tibble: 4 × 4
    ##   name    mean    sd   iqr
    ##   <chr>  <dbl> <dbl> <dbl>
    ## 1 a     0.0642  4.76  6.28
    ## 2 b     4.70    5.65  6.88
    ## 3 c     1.03    8.64  8.59
    ## 4 d     2.98   10.8  15.9

### NSDUH

This is a version of our function from last time.

``` r
nsduh_table_format = function(html, table_num) {
  
  out_table = 
    nsduh_html |> 
    html_table() |> 
    nth(table_num) |> 
    slice(-1) |> 
    select(-contains("P Value"))
  
  return(out_table)
  
}
```

We need to import the html, and then extract the correct tables.

``` r
nsduh_url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

nsduh_html = read_html(nsduh_url)
```

``` r
nsduh_table_format(html = nsduh_html, table_num = 1)
```

    ## # A tibble: 56 × 11
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
    ## # ℹ 46 more rows
    ## # ℹ 6 more variables: `18-25(2013-2014)` <chr>, `18-25(2014-2015)` <chr>,
    ## #   `26+(2013-2014)` <chr>, `26+(2014-2015)` <chr>, `18+(2013-2014)` <chr>,
    ## #   `18+(2014-2015)` <chr>

``` r
nsduh_table_format(html = nsduh_html, table_num = 4)
```

    ## # A tibble: 56 × 11
    ##    State `12+(2013-2014)` `12+(2014-2015)` `12-17(2013-2014)` `12-17(2014-2015)`
    ##    <chr> <chr>            <chr>            <chr>              <chr>             
    ##  1 Tota… 1.66a            1.76             0.60               0.64              
    ##  2 Nort… 1.94a            2.18             0.60               0.66              
    ##  3 Midw… 1.37             1.43             0.48               0.54              
    ##  4 South 1.45b            1.56             0.53               0.57              
    ##  5 West  2.03             2.05             0.82               0.85              
    ##  6 Alab… 1.23             1.22             0.42               0.41              
    ##  7 Alas… 1.54a            2.00             0.51               0.65              
    ##  8 Ariz… 2.25             2.29             1.01               0.85              
    ##  9 Arka… 0.93             1.07             0.41               0.48              
    ## 10 Cali… 2.14             2.16             0.89               0.94              
    ## # ℹ 46 more rows
    ## # ℹ 6 more variables: `18-25(2013-2014)` <chr>, `18-25(2014-2015)` <chr>,
    ## #   `26+(2013-2014)` <chr>, `26+(2014-2015)` <chr>, `18+(2013-2014)` <chr>,
    ## #   `18+(2014-2015)` <chr>

``` r
nsduh_table_format(html = nsduh_html, table_num = 5)
```

    ## # A tibble: 56 × 11
    ##    State `12+(2013-2014)` `12+(2014-2015)` `12-17(2013-2014)` `12-17(2014-2015)`
    ##    <chr> <chr>            <chr>            <chr>              <chr>             
    ##  1 Tota… 0.30             0.33             0.12               0.10              
    ##  2 Nort… 0.43a            0.54             0.13               0.13              
    ##  3 Midw… 0.30             0.31             0.11               0.10              
    ##  4 South 0.27             0.26             0.12               0.08              
    ##  5 West  0.25             0.29             0.13               0.11              
    ##  6 Alab… 0.22             0.27             0.10               0.08              
    ##  7 Alas… 0.70a            1.23             0.11               0.08              
    ##  8 Ariz… 0.32a            0.55             0.17               0.20              
    ##  9 Arka… 0.19             0.17             0.10               0.07              
    ## 10 Cali… 0.20             0.20             0.13               0.09              
    ## # ℹ 46 more rows
    ## # ℹ 6 more variables: `18-25(2013-2014)` <chr>, `18-25(2014-2015)` <chr>,
    ## #   `26+(2013-2014)` <chr>, `26+(2014-2015)` <chr>, `18+(2013-2014)` <chr>,
    ## #   `18+(2014-2015)` <chr>

``` r
nsduh_df = 
  tibble(
    drug = c("marj", "cocaine", "heroin"),
    table_n = c(1, 4, 5)
  ) |> 
  mutate(table = 
           map(table_n, \(x) nsduh_table_format(html = nsduh_html, table_num = x))) |> 
  unnest(table)

nsduh_df |> 
  filter(State == "New York")
```

    ## # A tibble: 3 × 13
    ##   drug    table_n State    `12+(2013-2014)` `12+(2014-2015)` `12-17(2013-2014)`
    ##   <chr>     <dbl> <chr>    <chr>            <chr>            <chr>             
    ## 1 marj          1 New York 14.24b           15.04            13.94             
    ## 2 cocaine       4 New York 2.28             2.54             0.71              
    ## 3 heroin        5 New York 0.38a            0.52             0.13              
    ## # ℹ 7 more variables: `12-17(2014-2015)` <chr>, `18-25(2013-2014)` <chr>,
    ## #   `18-25(2014-2015)` <chr>, `26+(2013-2014)` <chr>, `26+(2014-2015)` <chr>,
    ## #   `18+(2013-2014)` <chr>, `18+(2014-2015)` <chr>

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USW00022534 = "Molokai_HI",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: /Users/tamarapichardo/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2024-09-26 10:20:30.829978 (8.651)

    ## file min/max dates: 1869-01-01 / 2024-09-30

    ## using cached file: /Users/tamarapichardo/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2024-09-26 10:20:35.481187 (3.932)

    ## file min/max dates: 1949-10-01 / 2024-09-30

    ## using cached file: /Users/tamarapichardo/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2024-09-26 10:20:37.049433 (1.036)

    ## file min/max dates: 1999-09-01 / 2024-09-30

Create a list column.

``` r
weather_nest = 
  weather_df |> 
    nest(data = date:tmin)
```

``` r
weather_nest[["data"]][[1]]
```

    ## # A tibble: 730 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2021-01-01   157   4.4   0.6
    ##  2 2021-01-02    13  10.6   2.2
    ##  3 2021-01-03    56   3.3   1.1
    ##  4 2021-01-04     5   6.1   1.7
    ##  5 2021-01-05     0   5.6   2.2
    ##  6 2021-01-06     0   5     1.1
    ##  7 2021-01-07     0   5    -1  
    ##  8 2021-01-08     0   2.8  -2.7
    ##  9 2021-01-09     0   2.8  -4.3
    ## 10 2021-01-10     0   5    -1.6
    ## # ℹ 720 more rows

Let’s try regressing tmax on tmin.

``` r
lm(tmax ~ tmin, data = weather_nest[["data"]][[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest[["data"]][[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.514        1.034

``` r
lm(tmax ~ tmin, data = weather_nest[["data"]][[2]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest[["data"]][[2]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     21.7547       0.3222

``` r
lm(tmax ~ tmin, data = weather_nest[["data"]][[3]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest[["data"]][[3]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.532        1.137

``` r
weather_nest |> 
  mutate(model_fit = map(data, \(x) lm(tmax ~ tmin, data = x))) |> 
  pull(model_fit)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = x)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.514        1.034  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = x)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     21.7547       0.3222  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = x)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.532        1.137

Let’s define a function that fits the regression I want.

``` r
weather_lm = function(df){
  
  lm(tmax~tmin, data = df)
  
}
```

I could runt the models using the short function

``` r
weather_lm(weather_nest[["data"]][[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.514        1.034

``` r
map(weather_nest[["data"]], weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.514        1.034  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     21.7547       0.3222  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.532        1.137

``` r
weather_nest |> 
  mutate(model_fit = map(data, \(x) lm (tmax ~ tmin, data = x)))
```

    ## # A tibble: 3 × 4
    ##   name           id          data               model_fit
    ##   <chr>          <chr>       <list>             <list>   
    ## 1 CentralPark_NY USW00094728 <tibble [730 × 4]> <lm>     
    ## 2 Molokai_HI     USW00022534 <tibble [730 × 4]> <lm>     
    ## 3 Waterhole_WA   USS0023B17S <tibble [730 × 4]> <lm>
