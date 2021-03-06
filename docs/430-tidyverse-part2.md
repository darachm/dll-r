---
output:
  pdf_document: default
  html_document: default
---
## Tidyverse 2/2: reshaping data

Goal: Understand when reshaping a dataframe is necessary and how to do it using tidyverse functions `gather()` and `spread()`


### Why tidy data

So far we've used two datasets: `irisz` and `msleep`. Even though there is some missing data in these datasets, for the most part, they are quite neat. Notice how each line represents a single observation and each column represents a single variable. There is no redundancy in the rows or columns. Sometimes datasets will have columns with columns that mean the same thing but apply to only certain subsets of the data. Columns like this can be collapsed.

On the other hand, sometimes data will be recorded together in the same column, when they are in fact two different types of data. Imagine if conservation status in the `msleep` dataset were appended to the name. If we wanted to perform an analysis based on conservation status, we would need to tidy that up by making sure it's in a separate columns.


### How to tidy data

The reality is that datasets don't always come in neat format. Before and during data analysis, we will often need to "tidy" datasets. tidyverse has 4 important functions for tidying data:

- `gather()`: change data from a wide to long format - i.e. reduce extra columns
- `spread()`: change data from a long to wide format - i.e. expand a column into multiple columns
- `separate()`: spearate a column into 2 or more columns based on a pattern or delimeter
- `unite()`: unite multiple columns into 1


In lieu of re-inventing the wheel, here's an excellent explanation and illustration of the concept of tidy data and how to apply these functions to make data tidy.

**Read this excerpt from Data Science in R by Garret Grolemand https://garrettgman.github.io/tidying/**

For the asynchronous portion, let's apply these principles and functions to a new dataset. I'll set up a few example datasets based on the original. Answer the questions starting from the appropriate example dataset (e.g. `df_1`, `df_2`, `df_3`).

### Data exploration with tidyverse

**The dataset: New York Times COVID-19 cases**

Even though we're just using this dataset to learn about tidyverse, it's always important to read about the dataset. Downloading data can be difficult and we don't want to dive into that if the data is not what we want. So always read about it first! Read the [introduction on Github](https://github.com/nytimes/covid-19-data). Based on the description of the dataset, try to answer these questions: (no need to look at the actual data, the description should be sufficient)

1. Why was this data collected? 
2. When was the data last updated?
3. What kind of data is it? Is it genetic data, imaging data, text data? Is it descriptive, numeric, categorical? 
4. How was the data collected? One or many sources? 
5. Are there imperfections in the data? Limitations? E.g. missing data, different data meanings?

We'll read in the data straight from github. This is a lot of data (every county in the U.S.) so to make it more manageable downstream, we'll filter the dataset to only include:

1. A few states: California, Washington, Illinois, Nebraska, and Florida. Feel free to add your own state!
2. The last total count of cases and deaths



```
## ?????? Attaching packages ??????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????? tidyverse 1.3.0 ??????
```

```
## ??? ggplot2 3.3.2     ??? purrr   0.3.4
## ??? tibble  3.0.4     ??? dplyr   1.0.2
## ??? tidyr   1.1.2     ??? stringr 1.4.0
## ??? readr   1.4.0     ??? forcats 0.5.0
```

```
## ?????? Conflicts ???????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????? tidyverse_conflicts() ??????
## ??? dplyr::filter() masks stats::filter()
## ??? dplyr::lag()    masks stats::lag()
```



```r
data_url = "https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-counties.csv"

favorite_states = c("California", "Washington", "Illinois", "Nebraska",  "Florida")

df <- read.csv(data_url,
           header = T) %>%
  filter(state %in% favorite_states) %>%
  mutate(date = as.Date(date,format="%Y-%m-%d")) %>%
  filter(date == max(date))
```

Test your knowledge! Use functions we learned in the last section to answer these questions:

1. What is the last date for recorded cases? Is it the same for all states/counties?
2. Are all of the states listed in `favorite_states` in the dataset?
3. How many counties are there for each of the states?

#### Example datasets

**Use the following three datasets to complete the coding exercises** in [today's worksheet](https://github.com/darachm/dll-r/blob/main/worksheets/dll-r_Day4_Lab.Rmd).
The datasets are based on the New York Times Data, which is loaded above. 

**Example Dataset #1:`df_1`**
Three columns: county, cases, deaths


```r
df_1 <- df %>%
  tidyr::gather(key = "metric", value = "count", c(cases, deaths)) %>%
  arrange(state,county)

head(df_1)
```

```
##         date  county      state fips metric count
## 1 2021-06-13 Alameda California 6001  cases 89408
## 2 2021-06-13 Alameda California 6001 deaths  1719
## 3 2021-06-13  Alpine California 6003  cases    89
## 4 2021-06-13  Alpine California 6003 deaths     0
## 5 2021-06-13  Amador California 6005  cases  3720
## 6 2021-06-13  Amador California 6005 deaths    47
```

**Example Dataset #2:`df_2`**
Five columns: date, fips, metric corresponding to count, count, location (county, state)


```r
df_2 <- df %>%
  tidyr::gather(key = "metric", 
                value = "count", 
                c(cases, deaths)) %>%
  mutate(county_state_fips = paste0(county,",", state, ",", fips)) %>%
  select(-county,-state)

head(df_2)
```

```
##         date fips metric count         county_state_fips
## 1 2021-06-13 6001  cases 89408   Alameda,California,6001
## 2 2021-06-13 6003  cases    89    Alpine,California,6003
## 3 2021-06-13 6005  cases  3720    Amador,California,6005
## 4 2021-06-13 6007  cases 12576     Butte,California,6007
## 5 2021-06-13 6009  cases  2195 Calaveras,California,6009
## 6 2021-06-13 6011  cases  2276    Colusa,California,6011
```

**Example Dataset #3:`df_3`**
A column with the county name, followed by columns for each state with the number of cases for that state/county pair.


```r
df_3 <- df %>%
  select(county,state,cases) %>%
  tidyr::spread(key = "state", value = "cases")
  
head(df_3)
```

```
##      county California Florida Illinois Nebraska Washington
## 1     Adams         NA      NA     8654     3181       2212
## 2   Alachua         NA   25293       NA       NA         NA
## 3   Alameda      89408      NA       NA       NA         NA
## 4 Alexander         NA      NA      473       NA         NA
## 5    Alpine         89      NA       NA       NA         NA
## 6    Amador       3720      NA       NA       NA         NA
```


**Use these three datasets to complete the coding exercises** in [today's worksheet](https://github.com/darachm/dll-r/blob/main/worksheets/dll-r_Day4_Lab.Rmd)


