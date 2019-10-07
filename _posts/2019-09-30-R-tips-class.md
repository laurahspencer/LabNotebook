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
readxl package: fancy way to read in excel data - https://readxl.tidyverse.org/

#### stringr package 

#### lubridate package 
`years()`
`year()`

---

####  broom 
tidying up stats results!!!  
`tidy()`: Creates  a tibble with results from stats functions !!  
`augment()`: adds on statistics results to dataframe <--- WOW!  

#### janitor 
`clean_names()`: cleans up column names by adding things like underscores  

#### ggplot 

`facet_wrap(~factor, scales = 'free_y')+`: make multiple plots by a factor (e.g. same plot repeated by population), and "free_y" adjusts each plots y axis   
`facet_grid(site~factor)+`: make multiple plots by some factors, gridded out where everything on y=by site, and things on x=factor.
`aes(col=factor, group=factor)`: try using `group=` in addition to color/shape etc. to ensure correct grouping. 

Set a personalized theme that is frequently used, so not to repeat commonly used customizations 

    my_theme <- function(...) {
      theme(legend.title = element_blank(), #play around with turning these elements on and off one at a time
            plot.background = element_rect(), 
            panel.background = element_rect(fill = 'white'), #color background of plot
            panel.border = element_rect(fill = NA), #border of plot
            panel.grid = element_blank(),
            legend.key = element_blank(),
      ...)
    }

Multipanel plots: 
patchwork library:  https://github.com/thomasp85/patchwork 
cowplot library:  

Example of how to use cowplot for multipanel figures (this example in RMarkdown): 

    ```{r cowplot, fig.width = 11}
    a <- ggplot(...)
    b <- ggplot(...)
    plot_grid(a, b, ncol = 2)
    ```
