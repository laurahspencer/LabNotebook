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
`readxl` package: fancy way to read in excel data - https://readxl.tidyverse.org/   
`purrr::modify_if(is.factor, as.character)` - turn factors into characters (!)  
`tidyr::expand_grid()` - provides factorial combinations of things (!)  
`broom::augment()` - stats  
`purrr::safely()` = wrap functions with safely(fctn), and it returns a list with two objects: the result of the function, and any error messages  

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

### Functions 
Best practices:
- Always end a function with a `return()`  
- If function produces a ton of information, you can end it with `invisible()`  
- `formals()` = arguments that you feed to the function   
- `body()` = meat of the function, the code   
- You can set default arguments (aka formals) in functions when writing it. But, when calling the function you can give define arguments, which overwrite the deafult.  
- `...` - special arguent, allows you to pass additional arguments, that are unspecified in the function.  
- 

### Functional programming 
- "pure" functions = always spit out the same thing  
- "impure" functions = results depend on the input   
- Best practices is to group all your "impure" functions together   
- Best practices - store each function in a separate file  
- `!!` (bang-bang) evaluates the contents of the enquoted variable 
- the program `purrr` is good for lots of things that apply functions do (see below)  
- In RStudio use "CODE -> INSERT ROXYGEN FORMAT" to autopopulate a roxygen header to describe a function   

#### Example of how to use a function to create a ggplot, where you can feed it different variables. More complicated than you think: 

    foo <- function(data, histvar, fillvar) {
      histvar <- enquo(histvar) # captures what the user typed  
      fillvar <- enquo(fillvar)
        data %>%
        ggplot(aes(!!histvar, fill = !!fillvar)) +
        geom_histogram()  
    }
    foo(iris, Sepal.Length, Species)

#### Debugging
- first run `traceback()`, which will estimate where in the function your error happened   
- `browser()` = add this inside a function near the error, and it will walk you through how to find the error  

#### `purrr`  
- `map` = workhorse, basically the `apply()` function: map("Lists to apply function to","Function to apply across lists","Additional parameters").   
    `map(mtcars, mean, na.rm = T)` # <-- calculate the mean of mtcar columns, spits out as a vector  

- `map_THING()` = there are lots of `map` variations to define the output format   
    `map_df(mtcars, mean, na.rm=T) # <--- calculate the mean of mtcar columns, spits out a dataframe (well, a tibble)  
  
- `map2()` = allows you to apply a function to multiple lists. Example ... 
        map2_chr(c('one','two','red','blue'), c('fish'), paste)
        ## [1] "one fish"  "two fish"  "red fish"  "blue fish"
        
A more explicit way of calling the paste with map2, using an anonymous function (`~`) - 

        map2_chr(c('one','two','red','blue'), c('fish'), ~paste(.x, .y)) 
        
#### Wrangling and leveraging lists  
- `listviewer::jsonedit(MYLIST) - great way to interact with lists  
- Tibbles let you store lists inside dataframe columns. (Handy for any time you want to organize a complex result (like a regression) in a dataframe.)  
- `nest()`. Here's a really cool script that runs and returns lm objects for life expectancy by a country: 

        gapminder %>% 
          group_by(country) %>% 
          nest() %>% 
          mutate(foo_model = map(data, ~lm(lifeExp ~ gdpPercap, data = .x))) 

Then, if we want to clean up the model output names using `broom::tidy()`, then unnest lists to show lm statistical results in dataframe format: 

        gapminder %>% 
          group_by(country) %>% 
          nest() %>% 
          mutate(foo_model = map(data, ~lm(lifeExp ~ gdpPercap, data = .x))) %>% 
          mutate(foo_coefs = map(foo_model, broom::tidy)) %>% 
          unnest(cols = foo_coefs)


#### Wramgling lists with `purrr` 
- Extract items from a list with `map_chr('COLUMN NAME'). Example, "got_chars" is a list containing information about game of thrones characters. So, to get items in the "names" column from the first 5 items in the list: 

        got_chars[1:5] %>%
            map_chr('name')
Another example:  
        
        thing <- list(list(y = 2, z = list(w = 'hello')),  # create imbedded list (russian doll) 
                list(y = 2, z = list(w = 'world')))
         map_chr(thing, c("z","w")) 

Another example, where we apply the function `[` to extract names and allegiances of characters from the first 5 people in the list of game of throwns charaters: 

        got_chars[1:5] %>%
          map(`[`, c('name', 'allegiances'))  

Putting things together, now suppose that we want to find all the Lanisters in the GoT list. The `~` in the `map_lgl` function is used to tell R that the next thing is a function, not the name of a thing.  

        names <- map_chr(got_chars, "name")
        is_lannister <- map_lgl(names, ~ stringr::str_detect(.x, "Lannister")) 
        got_chars %>% 
          purrr::set_names(names) %>% 
          purrr::keep(is_lannister) %>% 
          listviewer::jsonedit()


       
