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
    ## -2.62418 -0.75082 -0.09708 -0.08067  0.62156  2.27445

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
    ##  [1] 1.2624907 0.8422488 2.2197139 3.0718708 3.7476883 1.6143206 3.2480441
    ##  [8] 2.8871676 2.9402356 3.9601808 2.2291721 4.4881329 3.5331537 1.7373697
    ## [15] 1.5416026 1.5344042 2.7331223 2.0427471 2.8130220 3.1737521
    ## 
    ## $b
    ##  [1]  1.4210540  0.8429183  4.9835096 -9.8822390 -3.8480522  0.6976152
    ##  [7] -0.5364649 -5.4349246 -1.5838821 -0.9966199 -8.5127851 11.5232974
    ## [13]  5.4621980  5.9064190 -6.7663035 -5.3930158 -2.0540453 -0.3923769
    ## [19] -3.2236947  8.6225454
    ## 
    ## $c
    ##  [1]  9.905968 10.234580  9.758238 10.125794  9.387429  9.933214 10.457348
    ##  [8] 10.079915 10.200149 10.208123  9.959641  9.823427 10.314863 10.073476
    ## [15]  9.919685  9.601680 10.586382 10.212059 10.033016 10.214920
    ## 
    ## $d
    ##  [1] -1.7656350 -1.8593650 -1.9964156 -3.5162934 -3.6294470 -3.1724268
    ##  [7] -2.9035576 -1.0122240 -3.4133013 -2.3971319 -0.8466493 -1.7013156
    ## [13] -1.0169147 -3.6939171 -3.4205965 -3.0884627 -3.9087230 -3.6189930
    ## [19] -3.6234303 -3.9914751

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
    ## 1  2.58 0.981

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.458  5.64

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.281

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.73  1.05

Let’s use a for loop:

``` r
output=vector("list",length=4)

for (i in 1:4){
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  }
```
