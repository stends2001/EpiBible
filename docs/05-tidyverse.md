# Tidyverse

The collection tidyverse includes some of the most widely used packages in R. Representing a collection of packages used for data cleaning, processing and visualization, every person familiar with R should know these packages! In this chapter we will be focussing on the implementation of the functions that make up the tidyverse, with a particular interest to data manipulation and visualization.

The packages in tidyverse work on dataframes which we saw in the previous chapter. In particular, the tidyverse is made for **tidy**-dataframes. Following Wickman's philosophy, a tidy dataframe is one with the following features:

- each variable is a column
- each observation is a row
- each value is a cell


## Wide and tidy data

Let’s go through an example together. Is the following table a tidy dataframe?


```r
# Column-wise matrix: male_cases, female_cases, year
flu_cases <- matrix(
  c(120, 150,   # male_cases
    100, 130,   # female_cases
    2020, 2021),# year
  nrow = 2,
  ncol = 3,
  byrow = FALSE   # fill column-wise
)

# Add column names
colnames(flu_cases) <- c("male_cases", "female_cases", "year")

flu_cases <- data.frame(flu_cases)
flu_cases
```

```
##   male_cases female_cases year
## 1        120          100 2020
## 2        150          130 2021
```

The answer is no!
Here, the variable sex is stored inside the column names instead of as a proper column.

The tidyverse provides two key functions in the tidyr package to reshape data:

- `pivot_longer()` → makes data longer (good for tidy analysis, many rows, fewer columns)
- `pivot_wider()` → makes data wider (the opposite transformation, more columns, fewer rows)

**pivot_longer**

We can reshape our flu data into a tidy format:

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
flu_tidy <- flu_cases %>%
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
Now indeed each variable (year, sex, cases) has its own column.
Tidy tables tend to be long, not wide — which is exactly what tidyverse functions expect.

**pivot_wider**

We can also go back to wide format:

```r
flu_wide_again <- flu_tidy %>%
  pivot_wider(
    names_from = sex,
    values_from = cases,
    names_glue = "{sex}_cases"
  )

flu_wide_again
```

```
## # A tibble: 2 × 3
##    year male_cases female_cases
##   <dbl>      <dbl>        <dbl>
## 1  2020        120          100
## 2  2021        150          130
```
Now we are back to the original table, where sex is encoded in the column names.


## Data wrangling using dplyr

There's loads of functions within tidyverse. We will go through a couple of them together, using one of the default datasets in R, namely gapminder. This includes yearly observations per country in the world, of many variables as you'll see. Then we'll use what we learned on an epidemiological dataset to figure out the source of an outbreak!


```r
library(gapminder)
head(gapminder)
```

```
## # A tibble: 6 × 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
## 4 Afghanistan Asia       1967    34.0 11537966      836.
## 5 Afghanistan Asia       1972    36.1 13079460      740.
## 6 Afghanistan Asia       1977    38.4 14880372      786.
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
head(gapminder %>% filter(year == 2007))
```

```
## # A tibble: 6 × 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       2007    43.8 31889923      975.
## 2 Albania     Europe     2007    76.4  3600523     5937.
## 3 Algeria     Africa     2007    72.3 33333216     6223.
## 4 Angola      Africa     2007    42.7 12420476     4797.
## 5 Argentina   Americas   2007    75.3 40301927    12779.
## 6 Australia   Oceania    2007    81.2 20434176    34435.
```

```r
head(gapminder %>% filter(continent %in% c('Oceania', 'Africa')))
```

```
## # A tibble: 6 × 6
##   country continent  year lifeExp      pop gdpPercap
##   <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Algeria Africa     1952    43.1  9279525     2449.
## 2 Algeria Africa     1957    45.7 10270856     3014.
## 3 Algeria Africa     1962    48.3 11000948     2551.
## 4 Algeria Africa     1967    51.4 12760499     3247.
## 5 Algeria Africa     1972    54.5 14760787     4183.
## 6 Algeria Africa     1977    58.0 17152804     4910.
```

```r
head(gapminder %>% filter(pop > 100000000))
```

```
## # A tibble: 6 × 6
##   country    continent  year lifeExp       pop gdpPercap
##   <fct>      <fct>     <int>   <dbl>     <int>     <dbl>
## 1 Bangladesh Asia       1987    52.8 103764241      752.
## 2 Bangladesh Asia       1992    56.0 113704579      838.
## 3 Bangladesh Asia       1997    59.4 123315288      973.
## 4 Bangladesh Asia       2002    62.0 135656790     1136.
## 5 Bangladesh Asia       2007    64.1 150448339     1391.
## 6 Brazil     Americas   1972    59.5 100840058     4986.
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

**Transmutate**

The function `transmutate()` does the same as `mutate()`, with the only difference being that exclusively the changed column is selected. Thus, `transmutate()` is a basically a combination of `mutate %>% select`.


**Arrange**

The function `arrange()` does not actually change anything inside of hte dataframe, but only adjusts the order in which we see it. Based on one or more columns, the order of rows is changed. By default in ascending order (low to high). To change to descening values we add `desc` as follows


```r
# get the smallest popularion sizes in 1992
head(gapminder %>% filter(year == 1992) %>% arrange(pop))
```

```
## # A tibble: 6 × 6
##   country               continent  year lifeExp    pop gdpPercap
##   <fct>                 <fct>     <int>   <dbl>  <int>     <dbl>
## 1 Sao Tome and Principe Africa     1992    62.7 125911     1429.
## 2 Iceland               Europe     1992    78.8 259012    25144.
## 3 Djibouti              Africa     1992    51.6 384156     2377.
## 4 Equatorial Guinea     Africa     1992    47.5 387838     1132.
## 5 Comoros               Africa     1992    57.9 454429     1247.
## 6 Bahrain               Asia       1992    72.6 529491    19036.
```

```r
# get the highest life expectancies in 2007 in Africa
head(gapminder %>% filter(year == 2007) %>% filter(continent == 'Africa') %>% arrange(desc(lifeExp)))
```

```
## # A tibble: 6 × 6
##   country   continent  year lifeExp      pop gdpPercap
##   <fct>     <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Reunion   Africa     2007    76.4   798094     7670.
## 2 Libya     Africa     2007    74.0  6036914    12057.
## 3 Tunisia   Africa     2007    73.9 10276158     7093.
## 4 Mauritius Africa     2007    72.8  1250882    10957.
## 5 Algeria   Africa     2007    72.3 33333216     6223.
## 6 Egypt     Africa     2007    71.3 80264543     5581.
```


**group_by**

The function `group_by()` allows us to group rows in a dataframe based on one or more columns. This does not yet change the dataframe values but rather sets up a grouped structure that works especially well with functions like `summarise()`.


```r
# group the data by continent
gapminder %>% group_by(continent)
```

```
## # A tibble: 1,704 × 6
## # Groups:   continent [5]
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
Notice nothing much looks different yet — but now the data is grouped internally. To see its power, we usually combine it with `summarise()`.


**summarise**

The function `summarise()` (or `summarize()`) lets us reduce each group to one row by applying summary functions such as `mean()`, `sum()`, `n()` (count rows), etc.

```r
# average life expectancy per continent in 2007
gapminder %>% 
  filter(year == 2007) %>% 
  group_by(continent) %>% 
  summarise(mean_lifeExp = mean(lifeExp))
```

```
## # A tibble: 5 × 2
##   continent mean_lifeExp
##   <fct>            <dbl>
## 1 Africa            54.8
## 2 Americas          73.6
## 3 Asia              70.7
## 4 Europe            77.6
## 5 Oceania           80.7
```

```r
# total population per continent in 2007
gapminder %>% 
  filter(year == 2007) %>% 
  group_by(continent) %>% 
  summarise(total_pop = sum(pop))
```

```
## # A tibble: 5 × 2
##   continent  total_pop
##   <fct>          <dbl>
## 1 Africa     929539692
## 2 Americas   898871184
## 3 Asia      3811953827
## 4 Europe     586098529
## 5 Oceania     24549947
```

```r
# combine multiple summaries
gapminder %>% 
  filter(year == 2007) %>% 
  group_by(continent) %>% 
  summarise(
    mean_lifeExp = mean(lifeExp),
    sd_lifeExp = sd(lifeExp),
    total_pop = sum(pop),
    n_countries = n()
  )
```

```
## # A tibble: 5 × 5
##   continent mean_lifeExp sd_lifeExp  total_pop n_countries
##   <fct>            <dbl>      <dbl>      <dbl>       <int>
## 1 Africa            54.8      9.63   929539692          52
## 2 Americas          73.6      4.44   898871184          25
## 3 Asia              70.7      7.96  3811953827          33
## 4 Europe            77.6      2.98   586098529          30
## 5 Oceania           80.7      0.729   24549947           2
```


new functions:
- rename
- as.character/as.numeric/as.Date
- across()
- recode
- case_when
- replace_na
- which()

## Data visualization using ggplot
