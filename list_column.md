list\_columns
================

## Lists

You can put anything in a list.

``` r
l=list(
vec_numeric=5:8,
vec_logical = c(TRUE, TRUE, TRUE, FALSE),
mat=matrix(1:8,nrow=2,ncol=4),
summary=summary(rnorm(100)))
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE  TRUE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -2.37213 -0.66205 -0.09689 -0.08606  0.57129  3.35634

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## `for` loop

create a new list.

``` r
list_norm =
   list(
    a = rnorm(20, mean=3, sd=1),
    b = rnorm(20, mean=0, sd=5),
    c = rnorm(20, mean=10, sd=.2),
    d = rnorm(20, mean=-3, sd=1)
  )
```

``` r
list_norm
```

    ## $a
    ##  [1] 4.515635 1.778194 1.916122 3.165337 3.410862 3.087257 3.467150 3.686879
    ##  [9] 2.176740 5.646345 3.832047 2.528506 2.826963 1.596078 1.806266 3.600785
    ## [17] 3.638548 3.113063 3.933863 1.513910
    ## 
    ## $b
    ##  [1]   0.7601772   3.2182713   1.9386832   8.8903037  -7.3195630  -0.7389246
    ##  [7]  -0.6308710   5.4023528  -0.8011400  -2.9791745  -3.3017953 -18.0395409
    ## [13]  -1.6379122   2.6295809   3.2099543  -0.6781319   3.0547064   3.7980442
    ## [19]  -3.6159582  -5.9694761
    ## 
    ## $c
    ##  [1] 10.058913  9.865210  9.629963 10.469326 10.371742 10.221095  9.853101
    ##  [8]  9.789941  9.917433  9.856676  9.704639 10.112501  9.679981  9.969912
    ## [15] 10.078856 10.134129  9.945195 10.000618  9.990064 10.412825
    ## 
    ## $d
    ##  [1] -3.902907 -2.939401 -3.473503 -2.568298 -3.385318 -3.817464 -3.844699
    ##  [8] -4.002472 -1.549258 -3.798796 -2.671603 -3.212143 -2.226586 -1.692597
    ## [15] -3.081594 -2.834166 -2.137294 -3.094546 -3.208505 -2.080213

pause and get my old function

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)

  tibble(
    mean = mean_x, 
    sd = sd_x
  )
}
```

I can apply that function to each list element

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.06  1.07

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.641  5.66

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.236

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.98 0.748

Let’s use a for loop:

``` r
output=vector("list",length=4)

for (i in 1:4){
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  }
```

\#lets try map

``` r
output=map(list_norm,mean_and_sd)
```

what if you want a different function?

``` r
output=map(list_norm,median)
```
