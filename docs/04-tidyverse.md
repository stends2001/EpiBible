# Tidyverse

The collection tidyverse includes some of the most widely used packages in R. Representing a collection of packages used for data cleaning, processing and visualization, every person familiar with R should know these packages! In this chapter we will be focussing on the implementation of the functions that make up the tidyverse, with a particular interest to data manipulation and visualization.

The packages in tidyverse work on dataframes which we saw in the previous chapter. In particular, the tidyverse is made for **tidy**-dataframes. Following Wickman's philosophy, a tidy dataframe is one with the following features:

- each variable is a column
- each observation is a row
- each value is a cell

Let's go through an example together. Is the following table a tidy dataframe?

```r
# Column-wise matrix: male_cases, female_cases, year
flu_cases_1 <- matrix(
  c(120, 150,   # male_cases
    100, 130,   # female_cases
    2020, 2021),# year
  nrow = 2,
  ncol = 3,
  byrow = FALSE   # fill column-wise
)

# Add column names
colnames(flu_cases_1) <- c("male_cases", "female_cases", "year")

flu_cases_1 = data.frame(flu_cases_1)
flu_cases_1
```

```
##   male_cases female_cases year
## 1        120          100 2020
## 2        150          130 2021
```
The answer is no!


```r
library(tidyr)
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
flu_tidy <- flu_cases_1 %>%
  pivot_longer(
    cols = c(male_cases, female_cases),
    names_to = "sex",
    values_to = "cases"
  ) %>%
  mutate(sex = ifelse(sex == "male_cases", "male", "female"))

flu_tidy
```

```
## # A tibble: 4 × 3
##    year sex    cases
##   <dbl> <chr>  <dbl>
## 1  2020 male     120
## 2  2020 female   100
## 3  2021 male     150
## 4  2021 female   130
```

Now indeed each variable (sex, cases and year) have their own column. Tidy tables or dataframes tend to be very long, more so than wide. This is the type of dataframes that we need for the tidyverse.



## Data wrangling using dplyr

There's loads of functions within tidyverse. We will go through a couple of them together, using one of the default datasets in R, namely gapminder. This includes yearly observations per country in the world, of many variables as you'll see. Then we'll use what we learned on an epidemiological dataset to figure out the source of an outbreak!


```r
library(gapminder)
gapminder
```

```
## # A tibble: 1,704 × 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.
##  2 Afghanistan Asia       1957    30.3  9240934      821.
##  3 Afghanistan Asia       1962    32.0 10267083      853.
##  4 Afghanistan Asia       1967    34.0 11537966      836.
##  5 Afghanistan Asia       1972    36.1 13079460      740.
##  6 Afghanistan Asia       1977    38.4 14880372      786.
##  7 Afghanistan Asia       1982    39.9 12881816      978.
##  8 Afghanistan Asia       1987    40.8 13867957      852.
##  9 Afghanistan Asia       1992    41.7 16317921      649.
## 10 Afghanistan Asia       1997    41.8 22227415      635.
## # ℹ 1,694 more rows
```

**pipeline-operator**
We connect different tidyverse functions using a so called pipeline operator: `%>%`. We can write it like:
`new_dataframe = old_dataframe %>% tidyverse_function`

**select**

Using the function `select()` we select specific column names


```r
selected_columns = gapminder %>% select(country, year, lifeExp)
head(selected_columns)
```

```
## # A tibble: 6 × 3
##   country      year lifeExp
##   <fct>       <int>   <dbl>
## 1 Afghanistan  1952    28.8
## 2 Afghanistan  1957    30.3
## 3 Afghanistan  1962    32.0
## 4 Afghanistan  1967    34.0
## 5 Afghanistan  1972    36.1
## 6 Afghanistan  1977    38.4
```

**filter**

The filter function is a very powerful function that allows us to only keep the rows in our dataframe that fulfill a certain condition. See chapter ... for more conditional logic in R, but here's a couple of examples


```r
gapminder %>% filter(year == 2007)
```

```
## # A tibble: 142 × 6
##    country     continent  year lifeExp       pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>     <int>     <dbl>
##  1 Afghanistan Asia       2007    43.8  31889923      975.
##  2 Albania     Europe     2007    76.4   3600523     5937.
##  3 Algeria     Africa     2007    72.3  33333216     6223.
##  4 Angola      Africa     2007    42.7  12420476     4797.
##  5 Argentina   Americas   2007    75.3  40301927    12779.
##  6 Australia   Oceania    2007    81.2  20434176    34435.
##  7 Austria     Europe     2007    79.8   8199783    36126.
##  8 Bahrain     Asia       2007    75.6    708573    29796.
##  9 Bangladesh  Asia       2007    64.1 150448339     1391.
## 10 Belgium     Europe     2007    79.4  10392226    33693.
## # ℹ 132 more rows
```

```r
gapminder %>% filter(continent %in% c('Oceania', 'Africa'))
```

```
## # A tibble: 648 × 6
##    country continent  year lifeExp      pop gdpPercap
##    <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Algeria Africa     1952    43.1  9279525     2449.
##  2 Algeria Africa     1957    45.7 10270856     3014.
##  3 Algeria Africa     1962    48.3 11000948     2551.
##  4 Algeria Africa     1967    51.4 12760499     3247.
##  5 Algeria Africa     1972    54.5 14760787     4183.
##  6 Algeria Africa     1977    58.0 17152804     4910.
##  7 Algeria Africa     1982    61.4 20033753     5745.
##  8 Algeria Africa     1987    65.8 23254956     5681.
##  9 Algeria Africa     1992    67.7 26298373     5023.
## 10 Algeria Africa     1997    69.2 29072015     4797.
## # ℹ 638 more rows
```

```r
gapminder %>% filter(pop > 100000000)
```

```
## # A tibble: 77 × 6
##    country    continent  year lifeExp       pop gdpPercap
##    <fct>      <fct>     <int>   <dbl>     <int>     <dbl>
##  1 Bangladesh Asia       1987    52.8 103764241      752.
##  2 Bangladesh Asia       1992    56.0 113704579      838.
##  3 Bangladesh Asia       1997    59.4 123315288      973.
##  4 Bangladesh Asia       2002    62.0 135656790     1136.
##  5 Bangladesh Asia       2007    64.1 150448339     1391.
##  6 Brazil     Americas   1972    59.5 100840058     4986.
##  7 Brazil     Americas   1977    61.5 114313951     6660.
##  8 Brazil     Americas   1982    63.3 128962939     7031.
##  9 Brazil     Americas   1987    65.2 142938076     7807.
## 10 Brazil     Americas   1992    67.1 155975974     6950.
## # ℹ 67 more rows
```


**mutate**

This is another very powerful and useful function. With mutate, we can change values in a dataframe. We can even change values in a dataframe that fulfill a certain condition. We do this by 
`mutate(column_name = ...)`

if column_name doesn't exist in the dataframe yet, a new column with this name is created.


```r
gapminder_pop_log = gapminder %>% mutate(pop_log = log(pop))
gapminder_pop_log
```

```
## # A tibble: 1,704 × 7
##    country     continent  year lifeExp      pop gdpPercap pop_log
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>   <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.    15.9
##  2 Afghanistan Asia       1957    30.3  9240934      821.    16.0
##  3 Afghanistan Asia       1962    32.0 10267083      853.    16.1
##  4 Afghanistan Asia       1967    34.0 11537966      836.    16.3
##  5 Afghanistan Asia       1972    36.1 13079460      740.    16.4
##  6 Afghanistan Asia       1977    38.4 14880372      786.    16.5
##  7 Afghanistan Asia       1982    39.9 12881816      978.    16.4
##  8 Afghanistan Asia       1987    40.8 13867957      852.    16.4
##  9 Afghanistan Asia       1992    41.7 16317921      649.    16.6
## 10 Afghanistan Asia       1997    41.8 22227415      635.    16.9
## # ℹ 1,694 more rows
```

```r
# some case_when functionality...
```


 Let’s check out gapminder. This is an example of tidy data, with 1704 rows and 6 columns.

```r
library(Epi)

data(S.typh)

typhoid_outbreak = S.typh
head(typhoid_outbreak)
```

```
##   id set case age sex abroad beef pork veal poultry liverp veg fruit egg plant7
## 1  1   1    1  52   1      0    1    1    1       1      1   0     1   1      1
## 2  2   1    0  52   1      0    1    0    0       0      1   1     1   0      0
## 3  3   1    0  52   1      0    1    1    0       1      1   1     1   1      0
## 4  4   2    1  41   1      0    1   NA   NA      NA      1   1    NA  NA     NA
## 5  5   2    0  41   1      0    1    1    0       0      1   1     0   1      1
## 6  6   2    0  41   1      0    1    1    0       0      1   1     1   1      1
```
First let's get some more information

```r
?S.typh
```

```
## starting httpd help server ... done
```



Let's make this a tidy dataframe first

```r
typhoid_outbreak_tidy = typhoid_outbreak %>%
  pivot_longer(
    cols = beef:plant7,         # all food/exposure columns
    names_to = "food",          # new column for the type of food
    values_to = "ate"           # 1 = ate, 0 = did not eat
  )

x = typhoid_outbreak_tidy %>%
  group_by(food,ate) %>%
  summarise(cases = sum(case))
```

```
## `summarise()` has grouped output by 'food'. You can override using the
## `.groups` argument.
```



### Tidy functions
There’s many different useful functions within tidyverse that allow manipulation and correction of data in a very easy way. If you use multiple commands of tidyr, they should be linked by the pipeline-operator (%>%). Here’s a couple of examples of commands:

pivot_longer(column_name(s), names_to=newname, values_to=val) allows you to create such a long tidy dataframe. You can have multiple columns, and change the column name into a new column itself (in this name “newname”) and change the values that used to belong to each of these columns to a new column (called “val” in this case).

pivot_wider(names_from=...,values_from=...) is effectively the opposite; it creates new columns that are the different levels of the “names_from-column” and merges the values thereof accordingly. I sometimes use this command to then reverse it using a pivot longer, to make sure that there is a value (mostly NA), for each and every cell.






transmuate() does the same as mutate(), the only difference being that exclusively the just (trans)mutated column is shown. Thus, transmutate() is a combination of mutate() and select().

group_by(cn) allows the use of further comments, per different value of a factor-column. Thus if you wish to get the mean life population per contintent, you’d write the following: gapminder %>% group_by(continent) %>% summarise(mean(pop))

Using Nest_by(), you can nest a dataframe into seperate dataframes per value of that column.

summarise() is super powerful but often requires that you know very well what you’re doing. It creates a new dataframe, often based on a grouped variable. Using accross, you can do a summarise on multiple columns. You can get back basic statistics, such as:

mean()

median()

sd()

min() or max()

arrange() allows you to adjust the order of the rows, based on one or more columns. By default it orders by ascending values, to get descending you write arrange(desc(cn1),desc(cn2))

## Data visualization using ggplot
