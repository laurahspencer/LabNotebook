---
layout: post
title: Running list of R tips from super-advanced class
---

Cheat Seets:  https://rstudio.com/resources/cheatsheets/

## tidyverse 

`here()` : https://cran.r-project.org/web/packages/here/index.html  
`read_csv()`: the way to read in a csv file tidily  (as opposed to 'read.csv'). Read in all data using "read_THING()"  
`unite()`:  reshape a dataframe  
`filter()`: subset a dataframe  
`select()`: select certain columns in a tibble  
`mutate()`: create a new column as a function of another function  
`pivot_longer()`: convert a tibble from wide to long format  
`pivot_wider()`: covert a tibble from long to wide  
`left_join()`: take data set 1, and join matching values from data set 2  <-- like bind/merge functions (but smarter)  
`right_join()`: take data set 2, and join matching values from data set 1  
`inner_join()`: join rows with matching values  
`full_join()`: join all columns and rows  

#### stringr package 

#### lubridate package 
`years()`
`year()`

---

####  broom 
tidying up stats results!!! 
`tidy()`: Creates  a tibble with results from stats functions !! 
`augment()`: adds on statistics results to dataframe <--- WOW! 
