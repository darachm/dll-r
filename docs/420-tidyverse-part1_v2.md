
## Using tidyverse

### Goals to be reworked alongside content

* Socio-technical context of tidyverse, ie that it's not base R but oh boy it's popular and well supported by a company - but not base R
* Tidyverse/dplyr takes a more database-like approach to moving data tables around
* Identify and predict what a magrittr pipe does
* Filter data tables, select and rename columns
* Mutate columns
* Pipe some stuff between functions, and write that out in a script
* Melt and gather
* Utilize group_by


### Welcome to the tidyverse! 

[Tidyverse](https://www.tidyverse.org) is a suite of R packages that are very useful for data science when we need to manipulate data frames and visualize data. The most important packages in tidyverse for our workshop will be [dplyr](https://dplyr.tidyverse.org) and [ggplot2](https://ggplot2.tidyverse.org). It builds on the base R functions and data types we've studied so far. It just provides a different design framework for working with data in R. The main two features of tidyverse are 1) a new suite of very useful functions and 2) the pipe `%>%` which will allow us to go send data from one function to the next without using intermediate steps or nesting functions together.

#### Preview: new functions in tidyverse

Let's say we have a dataframe, `df`, which has the name and age (in years) of everyone in a class. We want to create a new variable of age in months. In base R, we would do it this way:
`df$age_months <- df$age_years * 12`

In tidyverse, we can use the function `mutate()` to create a new variable. As with most tidyverse functions, `mutate()` takes the dataset as the first argument and the operation as the second. Here the operation is to create the new variable.

`mutate(df, age_months = age_years * 12)`

As you will see with more complex examples, these functions can make dataset operations more readable, especially when strung together with a pipe. 

#### Preview: the pipe in tidyverse


For example, recall the `head()` function used in the previous section. If you have a data frame, `df`, instead of peaking at it with `head(df)`, you can pipe it into the `head()` function like this: `df %>% head()`. This might seem like more work than just calling the function around the data frame object, but when you need to run a bunch of functions on a data frame, it becomes much TIDIER! 

In the previous example, we could call the function using a pipe like so:

`df %>% mutate(age_months = age_years * 12)`

#### The power of tidyverse in functions and pipes

To experience the power of tidyverse, consider this more complex example. Don't worry too much about the exact syntax. 

We have a dataframe, `df`,  with measumerments of flowers. We're interested in finding the average size of the setosa species across different geographical locations. We'll break this down into a few steps:

1. Filter the dataframe to only include measurments for the setosa species
2. Create a new single variable for size which combines length and width of the petal of a flower `size = petal.length * petal.width`
3. Group measurements by location and calculate the mean and standard deviation of flower sizes in that location


```r
df %>%
  filter(Species == "setosa") %>%
  mutate(Size = Petal.Length * Petal.Width) %>%
  group_by(Location) %>%
  summarize(Size.Mean = mean(Size),
            Size.SD = sd(Size)) %>%
  ungroup()
```

The tidyverse has many handy functions to manipulate data frames. The ones shown above allow us to:

- Filter data using `filter()`
- Create new variables with `mutate()`
- Group variables with `group_by()`
- Create a new variable over a group using `summarize()`

Notice how the `%>%` pipe is sending the output from before it to the output after it. This `%>%` is similar to the `|` we use on the command line to send data from one operation to the next: `sort data.txt | uniq -c` (sort a data file and count the number of unique lines)

How would you perform the above operation in base R, without the `%>%`? Give it a try!


```r
# Filter the data
df_filtered <- df[Species == "setosa"]

# Create new variable
df_filtered$Size = df_filtered$Petal.Length * df_filtered$Petal.Width

# aggregate and summarize
aggregate(data = df_filtered, 
          by = Location, 
          FUN = function(x) c(mean = mean(x), sd = sd(x)))
```

Note how in the base R version above we have to use intermediate steps. 
Alternatively, in base R, we could nestle all of these functions but that is also difficult to read.
The functions and pipe in the tidyverse make data manipulation TIDY!

Feeling motivated to accelerate data cleaning and analysis using the tidyverse?


### Let's start by making sure Tidyverse is installed

Download tidyverse
Remember we only have to install once


```r
install.packages("tidyverse")
```

Load the tidyverse library


```r
library(tidyverse)
```

```
## ?????? Attaching packages ????????????????????????????????????????????????????????????????????????????????????????????????????????????????????? tidyverse 1.3.1 ??????
```

```
## ??? ggplot2 3.3.3     ??? purrr   0.3.4
## ??? tibble  3.1.2     ??? dplyr   1.0.6
## ??? tidyr   1.1.3     ??? stringr 1.4.0
## ??? readr   1.4.0     ??? forcats 0.5.1
```

```
## ?????? Conflicts ?????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????? tidyverse_conflicts() ??????
## ??? dplyr::filter() masks stats::filter()
## ??? dplyr::lag()    masks stats::lag()
```

If loading the library returns an error message saying it's not installed, then go to the [SSRP Pre-installation guide](https://tinyurl.com/ssrpsoftwareguide) and follow the instructions for installing tidyverse

### Try tidyverse on the iris dataset we loaded in the previous section


```r
# Load the dataset and peak at it with the head() function
irisz <- read.csv("data/iris.csv")

head(irisz)
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species Location
## 1          5.1         3.5          1.4         0.2  setosa    Korea
## 2          4.9           3          1.4         0.2  setosa    China
## 3          4.7         3.2          1.3         0.2  setosa    Korea
## 4          4.6         3.1          1.5         0.2  setosa    China
## 5            5         3.6          1.4         0.2  setosa    China
## 6          5.4                      1.7         0.4  setosa   Canada
```

```r
# Now try it with tidyverse piping

# irisz %>% 
# library(tidyverse)

df <- irisz
df %>% head()
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species Location
## 1          5.1         3.5          1.4         0.2  setosa    Korea
## 2          4.9           3          1.4         0.2  setosa    China
## 3          4.7         3.2          1.3         0.2  setosa    Korea
## 4          4.6         3.1          1.5         0.2  setosa    China
## 5            5         3.6          1.4         0.2  setosa    China
## 6          5.4                      1.7         0.4  setosa   Canada
```

```r
# Let's pipe the data frame into a summary() function
# to get summary statistics

# In base R, we would do 
# summary(df)

# In tidyverse we do it this way:

irisz %>%
  summary()
```

```
##  Sepal.Length       Sepal.Width        Petal.Length       Petal.Width       
##  Length:150         Length:150         Length:150         Length:150        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##    Species            Location        
##  Length:150         Length:150        
##  Class :character   Class :character  
##  Mode  :character   Mode  :character
```


Now let's try that complex example from before


```r
# many of these columns are character data types
# Try the example from earlier
# since the petal measurments are characters, we need to convert them to numeric
# in order to perform math on them
# We also need to tell the mean() and sd() functions to ignore the NAs 
# where there are no measurments

irisz %>%
  filter(Species == "setosa") %>%
  mutate(Size = as.numeric(Petal.Length) * as.numeric(Petal.Width)) %>%
  group_by(Location) %>%
  summarize(Size.Mean = mean(Size, na.rm = T),
            Size.SD = sd(Size, na.rm = T)) %>%
  ungroup()
```

```
## # A tibble: 6 x 3
##   Location Size.Mean Size.SD
##   <chr>        <dbl>   <dbl>
## 1 ""           0.42  NA     
## 2 "Canada"     0.394  0.164 
## 3 "China"      0.408  0.205 
## 4 "Japan"      0.374  0.219 
## 5 "Korea"      0.415  0.238 
## 6 "Russia"     0.273  0.0795
```


Hopefully by now you have a sense for how tidyverse works, how tidyverse compares to the base R functions we've been using up until now, and are motivated to learn more!

## Essential tidyverse functions

Let's start with the essential functions of `tidyverse`. All of these are from the sub-package [dplyr](https://dplyr.tidyverse.org)

`select()`: subset columns
`filter()`: subset rows on conditions
`arrange()`: sort results
`mutate()`: create new columns by using information from other columns
`group_by()` and `summarize()`: create summary statistics on grouped data
`count()`: count discrete values




```r
# Let's select just the location 
select(irisz, Location)
```

```
##     Location
## 1      Korea
## 2      China
## 3      Korea
## 4      China
## 5      China
## 6     Canada
## 7      China
## 8      China
## 9     Russia
## 10     Japan
## 11    Russia
## 12    Canada
## 13     Korea
## 14    Russia
## 15    Canada
## 16    Canada
## 17     Korea
## 18     Korea
## 19     Japan
## 20     Korea
## 21    Russia
## 22     Korea
## 23     Japan
## 24     Japan
## 25    Russia
## 26    Russia
## 27     Japan
## 28    Russia
## 29     Japan
## 30    Canada
## 31    Canada
## 32    Canada
## 33    Russia
## 34    Russia
## 35    Russia
## 36    Russia
## 37     Korea
## 38     Japan
## 39     Korea
## 40     Japan
## 41     Japan
## 42     Japan
## 43     Japan
## 44     Korea
## 45     China
## 46          
## 47    Canada
## 48    Canada
## 49    Canada
## 50    Canada
## 51          
## 52          
## 53          
## 54       USA
## 55       USA
## 56       USA
## 57       USA
## 58       USA
## 59       USA
## 60          
## 61    Canada
## 62    Canada
## 63    Canada
## 64    Canada
## 65    Canada
## 66    Canada
## 67    Canada
## 68    Canada
## 69    Canada
## 70    Canada
## 71    Canada
## 72    Canada
## 73       USA
## 74    Canada
## 75    Canada
## 76    Canada
## 77    Canada
## 78    Canada
## 79          
## 80       USA
## 81    Canada
## 82    Canada
## 83    Canada
## 84       USA
## 85       USA
## 86       USA
## 87       USA
## 88       USA
## 89       USA
## 90    Canada
## 91       USA
## 92       USA
## 93       USA
## 94       USA
## 95       USA
## 96       USA
## 97    Canada
## 98    Canada
## 99    Canada
## 100         
## 101      USA
## 102      USA
## 103      USA
## 104      USA
## 105      USA
## 106      USA
## 107         
## 108      USA
## 109      USA
## 110      USA
## 111      USA
## 112      USA
## 113      USA
## 114         
## 115      USA
## 116      USA
## 117      USA
## 118      USA
## 119      USA
## 120      USA
## 121      USA
## 122      USA
## 123      USA
## 124      USA
## 125      USA
## 126      USA
## 127         
## 128      USA
## 129      USA
## 130      USA
## 131      USA
## 132      USA
## 133      USA
## 134         
## 135      USA
## 136      USA
## 137      USA
## 138         
## 139      USA
## 140      USA
## 141      USA
## 142      USA
## 143         
## 144      USA
## 145      USA
## 146      USA
## 147      USA
## 148      USA
## 149      USA
## 150
```

```r
# Let's select both the Location and Species columns 
# and see what unique combinations are present in the dataset
unique(select(irisz, c(Location, Species)))
```

```
##     Location    Species
## 1      Korea     setosa
## 2      China     setosa
## 6     Canada     setosa
## 8      China           
## 9     Russia     setosa
## 10     Japan     setosa
## 46               setosa
## 51           versicolor
## 54       USA versicolor
## 61    Canada versicolor
## 101      USA  virginica
## 107           virginica
## 143
```

```r
# We can also do this with the pipe
irisz %>%
  select(Location, Species) %>%
  unique()
```

```
##     Location    Species
## 1      Korea     setosa
## 2      China     setosa
## 6     Canada     setosa
## 8      China           
## 9     Russia     setosa
## 10     Japan     setosa
## 46               setosa
## 51           versicolor
## 54       USA versicolor
## 61    Canada versicolor
## 101      USA  virginica
## 107           virginica
## 143
```

```r
# Note that select() subsets the dataframe to the specified columns
# The output is a dataframe, even if only one column is selected

# What if we want to extract one column as a vector?
# For this we use pull() instead of select()
# pull() returns a vector

irisz %>%
  pull(Location) 
```

```
##   [1] "Korea"  "China"  "Korea"  "China"  "China"  "Canada" "China"  "China" 
##   [9] "Russia" "Japan"  "Russia" "Canada" "Korea"  "Russia" "Canada" "Canada"
##  [17] "Korea"  "Korea"  "Japan"  "Korea"  "Russia" "Korea"  "Japan"  "Japan" 
##  [25] "Russia" "Russia" "Japan"  "Russia" "Japan"  "Canada" "Canada" "Canada"
##  [33] "Russia" "Russia" "Russia" "Russia" "Korea"  "Japan"  "Korea"  "Japan" 
##  [41] "Japan"  "Japan"  "Japan"  "Korea"  "China"  ""       "Canada" "Canada"
##  [49] "Canada" "Canada" ""       ""       ""       "USA"    "USA"    "USA"   
##  [57] "USA"    "USA"    "USA"    ""       "Canada" "Canada" "Canada" "Canada"
##  [65] "Canada" "Canada" "Canada" "Canada" "Canada" "Canada" "Canada" "Canada"
##  [73] "USA"    "Canada" "Canada" "Canada" "Canada" "Canada" ""       "USA"   
##  [81] "Canada" "Canada" "Canada" "USA"    "USA"    "USA"    "USA"    "USA"   
##  [89] "USA"    "Canada" "USA"    "USA"    "USA"    "USA"    "USA"    "USA"   
##  [97] "Canada" "Canada" "Canada" ""       "USA"    "USA"    "USA"    "USA"   
## [105] "USA"    "USA"    ""       "USA"    "USA"    "USA"    "USA"    "USA"   
## [113] "USA"    ""       "USA"    "USA"    "USA"    "USA"    "USA"    "USA"   
## [121] "USA"    "USA"    "USA"    "USA"    "USA"    "USA"    ""       "USA"   
## [129] "USA"    "USA"    "USA"    "USA"    "USA"    ""       "USA"    "USA"   
## [137] "USA"    ""       "USA"    "USA"    "USA"    "USA"    ""       "USA"   
## [145] "USA"    "USA"    "USA"    "USA"    "USA"    ""
```

### Filtering dataframes

The basics of filtering are the `filter()` function and boolean operators (`>`, `<`, `==`), learned in section xxx


```r
# Filter for Petal.Length greater than x
x = 7

irisz %>%
  filter(as.numeric(Sepal.Length) > x)
```

```
## Warning in mask$eval_all_filter(dots, env_filter): NAs introduced by coercion
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width   Species Location
## 1           7.1           3          5.9         2.1 virginica      USA
## 2           7.6           3          6.6         2.1 virginica      USA
## 3           7.2         3.6          6.1         2.5 virginica      USA
## 4           7.7         3.8          6.7         2.2 virginica      USA
## 5           7.7         2.6          6.9          23 virginica      USA
## 6           7.7         2.8          6.7           2 virginica      USA
## 7           7.2         3.2            6         1.8 virginica      USA
## 8           7.2           3          5.8         1.6 virginica      USA
## 9           7.4         2.8          6.1         1.9 virginica      USA
## 10          7.9         3.8          6.4           2 virginica      USA
## 11          7.7           3          6.1         2.3 virginica      USA
```

```r
# filter to get observations of setosa species from Korea
irisz %>%
  filter(Species == "setosa",
         Location == "Korea")
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species Location
## 1           5.1         3.5          1.4         0.2  setosa    Korea
## 2           4.7         3.2          1.3         0.2  setosa    Korea
## 3           4.8           3          1.4         0.1  setosa    Korea
## 4           5.4         3.9          1.3         0.4  setosa    Korea
## 5           5.1         3.5          1.4         0.3  setosa    Korea
## 6           5.1         3.8          1.5         0.3  setosa    Korea
## 7           5.1         3.7          1.5         0.4  setosa    Korea
## 8           5.5        0.35          1.3         0.2  setosa    Korea
## 9           4.4           3          1.3         0.2  setosa    Korea
## 10          n/a         n/a          1.6         0.6  setosa    Korea
```


### Creating new variables


<to do >

### Summarizing data


Summarizing data is very important in data analysis. It allows you to get a big picture overview of a new dataset: type, range of values, etc. These are also types of summaries to think about including when in the exploratory stages: before running analyses on a dataset, create a summary of the dataset. Present this in during your update meetings during your summer project, and keep these summaries on hand in case you or others have questions about it throughout the course of your project.

For summarizing data in tidyverse, let's explore the functions `count()`, `group_by()`, `summarize()`


```r
# Count the number of observations for each Location
irisz %>%
  count(Location)
```

```
##   Location  n
## 1          14
## 2   Canada 35
## 3    China  6
## 4    Japan 11
## 5    Korea 10
## 6   Russia 11
## 7      USA 63
```

```r
# Count the number of observations for each unique combination
# of location and species

irisz %>%
  count(Location, Species)
```

```
##    Location    Species  n
## 1                       1
## 2               setosa  1
## 3           versicolor  6
## 4            virginica  6
## 5    Canada     setosa 11
## 6    Canada versicolor 24
## 7     China             1
## 8     China     setosa  5
## 9     Japan     setosa 11
## 10    Korea     setosa 10
## 11   Russia     setosa 11
## 12      USA versicolor 20
## 13      USA  virginica 43
```

What if we want to do more than just count the number of occurrences for a given group? Let's calculate summary statistics like mean and standard deviation.
`group_by` takes a variable and groups the data by that variable. Anything that happens after that is only calculated within the group variable. To turn off grouping, use `ungroup()`. It's always good practice to `ungroup()` after you're done grouping. Although it doesn't matter if you won't be doing any more operations on the dataset.


```r
# Here's an example that produces a similar result 
# as in the count() example above

irisz %>%
  group_by(Location) %>%
  summarize(n = n()) %>%
  ungroup()
```

```
## # A tibble: 7 x 2
##   Location     n
##   <chr>    <int>
## 1 ""          14
## 2 "Canada"    35
## 3 "China"      6
## 4 "Japan"     11
## 5 "Korea"     10
## 6 "Russia"    11
## 7 "USA"       63
```

```r
# Since mean() and sd() require numeric inputs,
# let's use mutate to change Petal.Length into a numeric data type
# We'll also tell mean() and sd() to ignore NAs in the calculation

irisz %>%
  mutate(Petal.Length = as.numeric(Petal.Length)) %>%
  group_by(Location) %>%
  summarize(n = n(),
            mean(Petal.Length, na.rm = T),
            sd(Petal.Length, na.rm = T)) %>%
  ungroup()
```

```
## Warning in mask$eval_all_mutate(quo): NAs introduced by coercion
```

```
## # A tibble: 7 x 4
##   Location     n `mean(Petal.Length, na.rm = T)` `sd(Petal.Length, na.rm = T)`
##   <chr>    <int>                           <dbl>                         <dbl>
## 1 ""          14                            4.51                         0.990
## 2 "Canada"    35                            3.39                         1.30 
## 3 "China"      6                            1.52                         0.194
## 4 "Japan"     11                            1.43                         0.205
## 5 "Korea"     10                            1.4                          0.105
## 6 "Russia"    11                            1.48                         0.218
## 7 "USA"       63                            5.21                         0.826
```

Comprehension: Try calculating the min and max Petal Length for each Species. Use `group_by()`.


We can also use base R functions with the tidyverse `%>%` and/or functions


```r
# How many of each location are there? Use the base R table() function
# Use the pull() function
irisz %>%
  pull(Location) %>%
  table()
```

```
## .
##        Canada  China  Japan  Korea Russia    USA 
##     14     35      6     11     10     11     63
```






### Updating fields based on a condition


### Reshaping data: long and wide




`gather()` and `spread()`


Advanced: `reshape()` (dcast package)





