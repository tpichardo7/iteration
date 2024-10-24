Writing Functions
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

## Writing my first function

As an example, here’s a z-score computation.

``` r
x_vec = rnorm(n = 25, mean = 10, sd = 3.5)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  1.484120e+00  5.783908e-01 -1.071553e+00  3.541991e-01 -5.881732e-01
    ##  [6]  1.797736e+00 -1.056237e+00  7.806215e-01 -1.935862e+00  1.544510e-01
    ## [11]  7.049257e-05 -1.193501e+00 -9.559455e-01 -3.066508e-01 -1.249853e-01
    ## [16] -6.836749e-01  6.818880e-01  5.852892e-01 -9.932290e-01 -4.488420e-02
    ## [21] -2.091439e-01  1.636651e+00  5.328628e-01 -9.821351e-01  1.559695e+00

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

    ##  [1]  1.484120e+00  5.783908e-01 -1.071553e+00  3.541991e-01 -5.881732e-01
    ##  [6]  1.797736e+00 -1.056237e+00  7.806215e-01 -1.935862e+00  1.544510e-01
    ## [11]  7.049257e-05 -1.193501e+00 -9.559455e-01 -3.066508e-01 -1.249853e-01
    ## [16] -6.836749e-01  6.818880e-01  5.852892e-01 -9.932290e-01 -4.488420e-02
    ## [21] -2.091439e-01  1.636651e+00  5.328628e-01 -9.821351e-01  1.559695e+00

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

    ##  [1]  1.484120e+00  5.783908e-01 -1.071553e+00  3.541991e-01 -5.881732e-01
    ##  [6]  1.797736e+00 -1.056237e+00  7.806215e-01 -1.935862e+00  1.544510e-01
    ## [11]  7.049257e-05 -1.193501e+00 -9.559455e-01 -3.066508e-01 -1.249853e-01
    ## [16] -6.836749e-01  6.818880e-01  5.852892e-01 -9.932290e-01 -4.488420e-02
    ## [21] -2.091439e-01  1.636651e+00  5.328628e-01 -9.821351e-01  1.559695e+00
