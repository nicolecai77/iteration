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
    ## -1.66138 -0.61175 -0.08194 -0.02857  0.62365  2.05656

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
    ##  [1] 2.430628 2.620675 3.541459 4.013572 3.102560 3.372669 4.375220 3.548177
    ##  [9] 3.268292 2.400259 3.986457 1.498742 2.184637 2.281037 3.818010 2.941349
    ## [17] 3.182654 0.791022 2.086061 2.998138
    ## 
    ## $b
    ##  [1] -2.210348  1.992813  3.948470 -2.148589  5.905628  4.737172  2.243800
    ##  [8] -2.187983  5.199355 -1.914922  2.236633  3.897262 -2.196794 -1.322944
    ## [15]  9.888990  3.447221  1.097340  4.229391  1.381143 -2.329977
    ## 
    ## $c
    ##  [1] 10.191317 10.162278  9.774877  9.978624 10.029991 10.055575 10.064923
    ##  [8]  9.909850 10.204491 10.157844 10.320709  9.832307 10.018341  9.875571
    ## [15]  9.698377  9.994695  9.965671 10.177521  9.671753 10.055547
    ## 
    ## $d
    ##  [1] -1.962510 -2.697983 -3.774750 -3.757816 -5.267569 -2.937071 -2.516868
    ##  [8] -3.552801 -4.881741 -2.943257 -3.949886 -3.658855 -3.934788 -2.500549
    ## [15] -4.505565 -4.599636 -3.900135 -2.742227 -1.471827 -2.310070

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
    ## 1  2.92 0.896

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.79  3.44

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.174

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.39  1.01

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

``` r
output=map_dbl(list_norm,median)
```

id will be in the new column called input

``` r
output=map_df(list_norm,mean_and_sd,.id="input")
```

## List columns!

``` r
listcol_df=tibble(
  name = c("a","b","c","d"),
  samp=list_norm
)
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 2.430628 2.620675 3.541459 4.013572 3.102560 3.372669 4.375220 3.548177
    ##  [9] 3.268292 2.400259 3.986457 1.498742 2.184637 2.281037 3.818010 2.941349
    ## [17] 3.182654 0.791022 2.086061 2.998138
    ## 
    ## $b
    ##  [1] -2.210348  1.992813  3.948470 -2.148589  5.905628  4.737172  2.243800
    ##  [8] -2.187983  5.199355 -1.914922  2.236633  3.897262 -2.196794 -1.322944
    ## [15]  9.888990  3.447221  1.097340  4.229391  1.381143 -2.329977
    ## 
    ## $c
    ##  [1] 10.191317 10.162278  9.774877  9.978624 10.029991 10.055575 10.064923
    ##  [8]  9.909850 10.204491 10.157844 10.320709  9.832307 10.018341  9.875571
    ## [15]  9.698377  9.994695  9.965671 10.177521  9.671753 10.055547
    ## 
    ## $d
    ##  [1] -1.962510 -2.697983 -3.774750 -3.757816 -5.267569 -2.937071 -2.516868
    ##  [8] -3.552801 -4.881741 -2.943257 -3.949886 -3.658855 -3.934788 -2.500549
    ## [15] -4.505565 -4.599636 -3.900135 -2.742227 -1.471827 -2.310070

``` r
listcol_df %>% 
  filter(name =="a")
```

    ## # A tibble: 1 × 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operation

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.92 0.896

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.79  3.44

can i just ..map?

``` r
map(listcol_df$samp,mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.92 0.896
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.79  3.44
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.174
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.39  1.01

so can i add a list column?

``` r
listcol_df %>% 
  mutate(summary=map_df(samp,mean_and_sd),
         medians=map_dbl(samp,median))
```

    ## # A tibble: 4 × 4
    ##   name  samp         summary$mean   $sd medians
    ##   <chr> <named list>        <dbl> <dbl>   <dbl>
    ## 1 a     <dbl [20]>           2.92 0.896    3.05
    ## 2 b     <dbl [20]>           1.79 3.44     2.11
    ## 3 c     <dbl [20]>          10.0  0.174   10.0 
    ## 4 d     <dbl [20]>          -3.39 1.01    -3.61
