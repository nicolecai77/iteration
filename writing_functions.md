writing\_functions
================

## Z scores

``` r
x_vec = rnorm(25, mean = 5, sd = 4)
(x_vec - mean(x_vec))/sd(x_vec)
```

    ##  [1] -0.09732319  1.99084072  1.42307341 -0.71311647  0.13238758  1.81693530
    ##  [7] -1.01298439 -1.02604366  0.12987634  0.70050076 -1.86791473 -0.44313520
    ## [13] -0.71792580  0.47411119 -1.52558764  0.08142432  0.49142716 -0.07454866
    ## [19] -0.75872592  0.86480523  0.65072997 -0.90341025  0.43835380  0.92827580
    ## [25] -0.98202567

``` r
z_scores = function(x) {
  
  z = (x - mean(x)) / sd(x)
  return(z)
  
}

z_scores(x=x_vec)
```

    ##  [1] -0.09732319  1.99084072  1.42307341 -0.71311647  0.13238758  1.81693530
    ##  [7] -1.01298439 -1.02604366  0.12987634  0.70050076 -1.86791473 -0.44313520
    ## [13] -0.71792580  0.47411119 -1.52558764  0.08142432  0.49142716 -0.07454866
    ## [19] -0.75872592  0.86480523  0.65072997 -0.90341025  0.43835380  0.92827580
    ## [25] -0.98202567

``` r
y_vec=rnorm(40, mean = 12, sd = 0.3)
z_scores(x=y_vec)
```

    ##  [1]  1.52829489  0.39408917 -0.79803511  2.83562714  0.59889680  1.04236873
    ##  [7]  0.52605244 -2.39118092 -0.89995555  1.30034965 -1.16017996 -0.51931657
    ## [13] -0.61395986 -1.67184034  0.46891636  1.11593331  1.05837952 -0.55845498
    ## [19] -0.27220132 -0.71392175 -0.58427162 -0.95870422 -1.05865025 -0.94178487
    ## [25]  0.24780322 -0.31831952 -1.02145972  1.01147046 -0.30676869  0.17718358
    ## [31]  0.22530028 -0.25138720  0.90950865  0.31373290 -0.45496494  1.08598086
    ## [37]  0.01137138  0.47176452  0.90341469 -0.73108115

How great is this ?? only kinda great let’s try again

``` r
z_scores = function(x){
  if(!is.numeric(x)){
    stop (" x should be numeric")
  }
  if (length(x)<3){
    stop ("x should have at least 3 numbers")
  }

  z = (x - mean(x)) / sd(x)
    return(z)
}
```

``` r
z_scores(3)
```

    ## Error in z_scores(3): x should have at least 3 numbers

``` r
z_scores(c("my", "name", "is", "jeff"))
```

    ## Error in z_scores(c("my", "name", "is", "jeff")):  x should be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars):  x should be numeric

## Multiple outputs

``` r
mean_and_sd = function(x){
  if(!is.numeric(x)){
    stop (" x should be numeric")
  }
  if (length(x)<3){
    stop ("x should have at least 3 numbers")
  }

  mean_x = mean(x)
  sd_x =sd(x)
  
  output_df =tibble(
    mean=mean_x,
    sd=sd_x
  )
  return(output_df)
}
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.48  3.83

``` r
mean_and_sd(y_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  12.0 0.295

\#\#different sample sizes, means,sds

``` r
sim_data = tibble(
  x = rnorm(30, mean = 2, sd = 3)
)

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.14  3.36

Let’s write a function that simulates data,computes the mean and sd we
can use named matching, which uses the argument name in the function
call. Named matching can be a bit more stable when you’re writing your
own functions (in case you decide to change the order of the inputs, for
example) but isn’t strictly necessary. Named arguments can be supplied
in any order:

``` r
sim_mean_sd = function(n,mu,sigma){
  #do checks on imputs
  
    sim_data = tibble(
    x = rnorm(n, mean = mu, sd = sigma)
  )
  
  sim_data %>% 
    summarize(
      mean = mean(x),
      sd = sd(x)
    )
    
}  
  
sim_mean_sd(n=30,mu=4,sigma=3)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.86  3.73

## revisit

convert them into text and put into the table

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

okay but there are a lot of pages of review write a function that gets
reviews based on page url put page into seperate page

``` r
get_page_reviews = function(page_url){
  page_html = read_html(page_url)
  
  review_titles = 
    page_html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()
  
  review_stars = 
    page_html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text() %>%
    str_extract("^\\d") %>%
    as.numeric()
  
  review_text = 
    page_html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim()
  
  reviews = tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text
  )
   return(reviews)
}
base_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="
urls = str_c(base_url,1:5)

bind_rows(
  get_page_reviews(urls[1]),
  get_page_reviews(urls[2]),
  get_page_reviews(urls[3]),
  get_page_reviews(urls[4]),
  get_page_reviews(urls[5]))
```

    ## # A tibble: 50 × 3
    ##    title                                                 stars text             
    ##    <chr>                                                 <dbl> <chr>            
    ##  1 it was                                                    5 "mad good yo"    
    ##  2 Fun!                                                      4 "Fun and enterta…
    ##  3 Vintage                                                   5 "Easy to order. …
    ##  4 too many commercials                                      1 "5 minutes into …
    ##  5 this film is so good!                                     5 "VOTE FOR PEDRO!"
    ##  6 Good movie                                                5 "Weird story, go…
    ##  7 I Just everyone to know this....                          5 "VOTE FOR PEDRO …
    ##  8 the cobweb in his hair during the bike ramp scene lol     5 "5 stars for bei…
    ##  9 Best quirky movie ever                                    5 "You all know th…
    ## 10 Classic Film                                              5 "Had to order th…
    ## # … with 40 more rows
