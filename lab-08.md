Lab 08 - University of Edinburgh Art Collection
================
Zi Li
3/22/2025

## Load Packages and Data

load the necessary packages:

``` r
library(tidyverse) 

# install.packages("skimr")
library(skimr)
```

Now, load the dataset. If your data isn’t ready yet, you can leave
`eval = FALSE` for now and update it when needed.

``` r
# Remove eval = FALSE or set it to TRUE once data is ready to be loaded
uoe_art <- read_csv("data/uoe-art.csv")
```

## Exercise 9

``` r
uoe_art <- uoe_art %>%
  separate(title, into = c("title", "date"), sep = " \\(") %>%
  mutate(year = str_remove(date, "\\)") %>% as.numeric()) %>%
  select(title, artist, year, link)
```

    ## Warning: Expected 2 pieces. Additional pieces discarded in 47 rows [76, 261, 299, 302,
    ## 530, 539, 628, 699, 788, 792, 921, 951, 952, 1126, 1155, 1178, 1181, 1430,
    ## 1454, 1471, ...].

    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 700 rows [3, 11, 19, 23,
    ## 25, 33, 37, 38, 40, 41, 44, 49, 50, 52, 54, 61, 65, 66, 69, 77, ...].

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `year = str_remove(date, "\\)") %>% as.numeric()`.
    ## Caused by warning in `str_remove(date, "\\)") %>% as.numeric()`:
    ## ! NAs introduced by coercion

## Exercise 10

the warning is ok,because some date is NA, which is expected and normal.

## Exercise 11

``` r
skim(uoe_art)
```

|                                                  |         |
|:-------------------------------------------------|:--------|
| Name                                             | uoe_art |
| Number of rows                                   | 3312    |
| Number of columns                                | 4       |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |         |
| Column type frequency:                           |         |
| character                                        | 3       |
| numeric                                          | 1       |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |         |
| Group variables                                  | None    |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| title         |         0 |             1 |   2 |  95 |     0 |     1590 |          0 |
| artist        |         2 |             1 |   2 | 459 |     0 |     2536 |          0 |
| link          |         0 |             1 |  57 |  60 |     0 |     3312 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |    mean |   sd |  p0 |  p25 |  p50 |  p75 | p100 | hist  |
|:--------------|----------:|--------------:|--------:|-----:|----:|-----:|-----:|-----:|-----:|:------|
| year          |      1579 |          0.52 | 1964.54 | 53.2 |   2 | 1953 | 1962 | 1977 | 2020 | ▁▁▁▁▇ |

``` r
# 2 pieces missing artist; 1579 pieces missing year.
```

## Exercises 12

``` r
ggplot(uoe_art, aes(x = year)) +
  geom_histogram(binwidth = 22, fill = "pink", color = "blue") 
```

    ## Warning: Removed 1579 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](lab-08_files/figure-gfm/histogram%20of%20years-1.png)<!-- -->

``` r
# it seems like, there’s one weird outlier year that’s super low — like 1 or 2; 
```

## Exercises 13

``` r
# check the year. I filter the year, and arrange the year from smallest to largest. 
uoe_art %>%
  filter(!is.na(year)) %>%
  arrange(year) %>%
  head(10) # Show the 10 earliest years
```

    ## # A tibble: 10 × 4
    ##    title                                  artist                      year link 
    ##    <chr>                                  <chr>                      <dbl> <chr>
    ##  1 Death Mask                             H. Dempshall Death Mask        2 http…
    ##  2 Sampler. Mary Ann Park                 Mary Ann Park Sampler. Ma…  1819 http…
    ##  3 Fine lawn collar                       Unknown Fine lawn collar    1820 http…
    ##  4 Dying Gaul                             Unknown Dying Gaul          1822 http…
    ##  5 The Dead Christ                        Unknown The Dead Christ     1831 http…
    ##  6 Crouching Venus                        Sarti Crouching Venus       1834 http…
    ##  7 Gates of Paradise                      Clement Pappi Gates of Pa…  1835 http…
    ##  8 Castor and Pollux                      Sarti Castor and Pollux     1835 http…
    ##  9 Metope 9, south entablature, Parthenon Metope                      1837 http…
    ## 10 Metope 8, south entablature, Parthenon Metope                      1837 http…

``` r
uoe_art <- uoe_art %>%
  mutate(year = if_else(title == "Death Mask",1964, year))
```

## Exercises 14

``` r
uoe_art %>%
  count(artist, sort = TRUE) %>% # sort=true show largest group at top, head(10): only want 10 largest group. 
  head(10)
```

    ## # A tibble: 10 × 2
    ##    artist                                        n
    ##    <chr>                                     <int>
    ##  1 Unknown Untitled                            103
    ##  2 Unknown Unknown                              64
    ##  3 Emma Gillies Plate                           31
    ##  4 Emma Gillies Saucer                          24
    ##  5 Emma Gillies Teacup                          22
    ##  6 Boris Bu                                     17
    ##  7 Riders from South Frieze of the Parthenon    14
    ##  8 Emma Gillies Espresso Saucer                 13
    ##  9 Eduardo Luigi Paolozzi KBE                   12
    ## 10 Robert Rivers Untitled                       12

``` r
# top one is "unknown", then Emma Gillies Plate. (never heard of her tho)
```

## Exercises 15

``` r
uoe_art %>% filter (str_detect(title, "Child") )%>%
  count()
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1    11

``` r
# I see, we supposed to use the combination of filter() and str_detect(), no wonder I kept getting error. 
# return 0, when use child; and 11 when use Child.

# 11 art pieces have the word Child in their title. 
```
