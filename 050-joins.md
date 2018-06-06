---
title: "joins"
author: "Daniel Chen"
date: "6/5/2018"
output: 
  html_document: 
    keep_md: yes
    number_sections: yes
    toc: yes
editor_options: 
  chunk_output_type: console
---





```r
library(tibble)
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```


```r
x <- tribble(
    ~key, ~val_x,
    1, "x1",
    2, "x2",
    3, "x3"
)
x
```

```
## # A tibble: 3 x 2
##     key val_x
##   <dbl> <chr>
## 1     1 x1   
## 2     2 x2   
## 3     3 x3
```


```r
y <- tribble(
    ~key, ~val_y,
    1, "y1",
    2, "y2",
    4, "y3"
)
y
```

```
## # A tibble: 3 x 2
##     key val_y
##   <dbl> <chr>
## 1     1 y1   
## 2     2 y2   
## 3     4 y3
```



```r
x %>% 
    inner_join(y, by = 'key')
```

```
## # A tibble: 2 x 3
##     key val_x val_y
##   <dbl> <chr> <chr>
## 1     1 x1    y1   
## 2     2 x2    y2
```



```r
x %>% 
    left_join(y, by = 'key')
```

```
## # A tibble: 3 x 3
##     key val_x val_y
##   <dbl> <chr> <chr>
## 1     1 x1    y1   
## 2     2 x2    y2   
## 3     3 x3    <NA>
```


```r
x %>% 
    right_join(y, by = 'key')
```

```
## # A tibble: 3 x 3
##     key val_x val_y
##   <dbl> <chr> <chr>
## 1     1 x1    y1   
## 2     2 x2    y2   
## 3     4 <NA>  y3
```


```r
x %>% 
    full_join(y, by = 'key')
```

```
## # A tibble: 4 x 3
##     key val_x val_y
##   <dbl> <chr> <chr>
## 1     1 x1    y1   
## 2     2 x2    y2   
## 3     3 x3    <NA> 
## 4     4 <NA>  y3
```

# many to one OR one to many

```r
(x <- tribble(
    ~key, ~val_x,
    1, "x1",
    2, "x2",
    3, "x3",
    1, "x4",
    1, "x5",
    1, "x6"
))
```

```
## # A tibble: 6 x 2
##     key val_x
##   <dbl> <chr>
## 1     1 x1   
## 2     2 x2   
## 3     3 x3   
## 4     1 x4   
## 5     1 x5   
## 6     1 x6
```

```r
(y <- tribble(
    ~key, ~val_y,
    1, "y1",
    2, "y2",
    4, "y3"
))
```

```
## # A tibble: 3 x 2
##     key val_y
##   <dbl> <chr>
## 1     1 y1   
## 2     2 y2   
## 3     4 y3
```


```r
x %>%
    left_join(y, by = 'key')
```

```
## # A tibble: 6 x 3
##     key val_x val_y
##   <dbl> <chr> <chr>
## 1     1 x1    y1   
## 2     2 x2    y2   
## 3     3 x3    <NA> 
## 4     1 x4    y1   
## 5     1 x5    y1   
## 6     1 x6    y1
```



# many to many


```r
(x <- tribble(
    ~key, ~val_x,
    1, "x1",
    2, "x2",
    3, "x3",
    1, "x4",
    1, "x5",
    1, "x6"
))
```

```
## # A tibble: 6 x 2
##     key val_x
##   <dbl> <chr>
## 1     1 x1   
## 2     2 x2   
## 3     3 x3   
## 4     1 x4   
## 5     1 x5   
## 6     1 x6
```

```r
(y <- tribble(
    ~key, ~val_y,
    1, "y1",
    2, "y2",
    4, "y3",
    1, "y4",
    1, "y5"
))
```

```
## # A tibble: 5 x 2
##     key val_y
##   <dbl> <chr>
## 1     1 y1   
## 2     2 y2   
## 3     4 y3   
## 4     1 y4   
## 5     1 y5
```

a many to many join creates a cartesian product


```r
x %>%
    left_join(y, by = 'key')
```

```
## # A tibble: 14 x 3
##      key val_x val_y
##    <dbl> <chr> <chr>
##  1     1 x1    y1   
##  2     1 x1    y4   
##  3     1 x1    y5   
##  4     2 x2    y2   
##  5     3 x3    <NA> 
##  6     1 x4    y1   
##  7     1 x4    y4   
##  8     1 x4    y5   
##  9     1 x5    y1   
## 10     1 x5    y4   
## 11     1 x5    y5   
## 12     1 x6    y1   
## 13     1 x6    y4   
## 14     1 x6    y5
```


The below code won't work, but it's to show you that you can join by multiple keys

```r
x %>%
    left_join(y, by = c('key', 'col2', 'col3'))
```

as well as do joins when columns do not share the same name

```r
x %>%
    left_join(y, by = c('key' = 'key2', 'col2' = 'other', 'col3'))
```
