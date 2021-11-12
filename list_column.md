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
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -2.7554 -0.6768  0.1359  0.1044  0.9026  2.4603

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
    ##  [1] 2.424728 2.684950 3.558717 3.269343 2.542329 1.423601 3.745542 2.994101
    ##  [9] 5.416635 3.562696 3.425026 4.654154 4.188259 1.621313 3.174900 2.269377
    ## [17] 2.930629 3.011521 4.557597 2.280106
    ## 
    ## $b
    ##  [1]  -1.8400004   5.1572932   1.6119244   0.4208640  -0.8022353   4.3898481
    ##  [7]   6.4183294 -10.7390179   8.8326203   2.1305627  -0.1176681   0.2178501
    ## [13]   7.2020978   0.6743878   3.7309972  -3.6977150   4.3034738   5.2760458
    ## [19]  11.6770597  -6.3556135
    ## 
    ## $c
    ##  [1]  9.898511 10.145191  9.894408 10.129294  9.753106 10.199882  9.853706
    ##  [8] 10.293447 10.262645 10.084395  9.968903 10.057549 10.329680  9.933772
    ## [15] 10.075560  9.941716 10.200128  9.836767  9.870526 10.141922
    ## 
    ## $d
    ##  [1] -1.964472 -3.710925 -1.580461 -2.616967 -2.475898 -2.641482 -1.110946
    ##  [8] -2.068745 -2.839987 -1.210232 -2.810481 -1.921648 -1.366256 -1.969162
    ## [15] -4.372408 -3.241539 -1.662851 -3.890221 -2.265617 -2.570182

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
    ## 1  3.19  1.01

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.92  5.24

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.168

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.41 0.892

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
    ##  [1] 2.424728 2.684950 3.558717 3.269343 2.542329 1.423601 3.745542 2.994101
    ##  [9] 5.416635 3.562696 3.425026 4.654154 4.188259 1.621313 3.174900 2.269377
    ## [17] 2.930629 3.011521 4.557597 2.280106
    ## 
    ## $b
    ##  [1]  -1.8400004   5.1572932   1.6119244   0.4208640  -0.8022353   4.3898481
    ##  [7]   6.4183294 -10.7390179   8.8326203   2.1305627  -0.1176681   0.2178501
    ## [13]   7.2020978   0.6743878   3.7309972  -3.6977150   4.3034738   5.2760458
    ## [19]  11.6770597  -6.3556135
    ## 
    ## $c
    ##  [1]  9.898511 10.145191  9.894408 10.129294  9.753106 10.199882  9.853706
    ##  [8] 10.293447 10.262645 10.084395  9.968903 10.057549 10.329680  9.933772
    ## [15] 10.075560  9.941716 10.200128  9.836767  9.870526 10.141922
    ## 
    ## $d
    ##  [1] -1.964472 -3.710925 -1.580461 -2.616967 -2.475898 -2.641482 -1.110946
    ##  [8] -2.068745 -2.839987 -1.210232 -2.810481 -1.921648 -1.366256 -1.969162
    ## [15] -4.372408 -3.241539 -1.662851 -3.890221 -2.265617 -2.570182

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
    ## 1  3.19  1.01

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.92  5.24

can i just ..map?

``` r
map(listcol_df$samp,mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.19  1.01
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.92  5.24
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.168
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.41 0.892

so can i add a list column?

``` r
listcol_df %>% 
  mutate(summary=map_df(samp,mean_and_sd),
         medians=map_dbl(samp,median))
```

    ## # A tibble: 4 × 4
    ##   name  samp         summary$mean   $sd medians
    ##   <chr> <named list>        <dbl> <dbl>   <dbl>
    ## 1 a     <dbl [20]>           3.19 1.01     3.09
    ## 2 b     <dbl [20]>           1.92 5.24     1.87
    ## 3 c     <dbl [20]>          10.0  0.168   10.1 
    ## 4 d     <dbl [20]>          -2.41 0.892   -2.37

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2021-10-17 19:22:04 (7.605)

    ## file min/max dates: 1869-01-01 / 2021-10-31

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2021-10-17 19:22:09 (1.697)

    ## file min/max dates: 1965-01-01 / 2020-02-29

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2021-10-17 19:22:12 (0.912)

    ## file min/max dates: 1999-09-01 / 2021-10-31

Get out list columns ..

``` r
weather_nest =
  weather_df %>% 
  nest(data = date:tmin)
```

``` r
weather_nest %>% pull(name)
```

    ## [1] "CentralPark_NY" "Waikiki_HA"     "Waterhole_WA"

``` r
weather_nest %>% pull(data)
```

    ## [[1]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # … with 355 more rows

suppose i want to regress `tmax` on `tmin` for each station.

``` r
lm(tmax~tmin,data=weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

Let’s write a function

``` r
weather_lm= function(df){
  lm(tmax~tmin,data=df)
}



output = vector("list",3)
for (i in 1:3){
  output[[i]]=weather_lm(weather_nest$data[[i]])
}
```

what about a map..?

``` r
map(weather_nest$data,weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

what about a map in a list column?

``` r
weather_nest=weather_nest %>% 
  mutate(models = map(data,weather_lm))
weather_nest$models
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221
