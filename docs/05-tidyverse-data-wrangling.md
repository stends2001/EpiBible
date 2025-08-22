# Introduction to the Tidyverse

The **tidyverse** is a collection of R packages designed for data science that share an underlying design philosophy, grammar, and data structures. These packages are essential tools for anyone working with data in R, providing intuitive and consistent functions for data cleaning, manipulation, and visualization.

The tidyverse follows Hadley Wickham's philosophy of **tidy data**, where:

- Each variable is a column
- Each observation is a row  
- Each value is a cell

This structure makes data analysis more intuitive and efficient.


```r
library(tidyverse)  # Loads core tidyverse packages
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
library(gapminder)  # Example dataset
```

## Data Structure: Wide vs. Tidy Data

### Understanding the Difference

Let's explore the concept of tidy data with a practical example:


```r
# Create a wide-format dataset
flu_cases <- data.frame(
  year = c(2020, 2021),
  male_cases = c(120, 150),
  female_cases = c(100, 130)
)

print("Wide format (not tidy):")
```

```
## [1] "Wide format (not tidy):"
```

```r
flu_cases
```

```
##   year male_cases female_cases
## 1 2020        120          100
## 2 2021        150          130
```

**Why isn't this tidy?** The variable "sex" is encoded in the column names rather than being a proper column with values.

### Reshaping Data with tidyr

#### Making Data Longer with `pivot_longer()`


```r
flu_tidy <- flu_cases %>%
  pivot_longer(
    cols = c(male_cases, female_cases),     # Columns to pivot
    names_to = "sex",                       # New column for variable names
    values_to = "cases"                     # New column for values
  ) %>%
  mutate(sex = str_replace(sex, "_cases", ""))  # Clean up the sex values

print("Tidy format (long):")
```

```
## [1] "Tidy format (long):"
```

```r
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

#### Making Data Wider with `pivot_wider()`


```r
flu_wide_again <- flu_tidy %>%
  pivot_wider(
    names_from = sex,           # Column to get new column names from
    values_from = cases,        # Column to get values from
    names_glue = "{sex}_cases"  # Template for new column names
  )

print("Back to wide format:")
```

```
## [1] "Back to wide format:"
```

```r
flu_wide_again
```

```
## # A tibble: 2 × 3
##    year male_cases female_cases
##   <dbl>      <dbl>        <dbl>
## 1  2020        120          100
## 2  2021        150          130
```

## Core dplyr Functions for Data Manipulation

Let's explore the essential dplyr functions using the gapminder dataset:


```r
head(gapminder, 10)
```

```
## # A tibble: 10 × 6
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
```

### The Pipe Operator (`%>%`)

The pipe operator is the backbone of tidyverse workflows. It passes the result of one function as the first argument to the next function:


```r
# Instead of: function_c(function_b(function_a(data), arg1), arg2)
# We write: data %>% function_a() %>% function_b(arg1) %>% function_c(arg2)
```

### `select()`: Choose Columns


```r
# Select specific columns
selected_columns <- gapminder %>% 
  select(country, year, lifeExp, pop)

head(selected_columns)
```

```
## # A tibble: 6 × 4
##   country      year lifeExp      pop
##   <fct>       <int>   <dbl>    <int>
## 1 Afghanistan  1952    28.8  8425333
## 2 Afghanistan  1957    30.3  9240934
## 3 Afghanistan  1962    32.0 10267083
## 4 Afghanistan  1967    34.0 11537966
## 5 Afghanistan  1972    36.1 13079460
## 6 Afghanistan  1977    38.4 14880372
```

```r
# Select columns by pattern
gapminder %>% 
  select(country, starts_with("life")) %>% 
  head()
```

```
## # A tibble: 6 × 2
##   country     lifeExp
##   <fct>         <dbl>
## 1 Afghanistan    28.8
## 2 Afghanistan    30.3
## 3 Afghanistan    32.0
## 4 Afghanistan    34.0
## 5 Afghanistan    36.1
## 6 Afghanistan    38.4
```

```r
# Select everything except certain columns
gapminder %>% 
  select(-continent, -gdpPercap) %>% 
  head()
```

```
## # A tibble: 6 × 4
##   country      year lifeExp      pop
##   <fct>       <int>   <dbl>    <int>
## 1 Afghanistan  1952    28.8  8425333
## 2 Afghanistan  1957    30.3  9240934
## 3 Afghanistan  1962    32.0 10267083
## 4 Afghanistan  1967    34.0 11537966
## 5 Afghanistan  1972    36.1 13079460
## 6 Afghanistan  1977    38.4 14880372
```

### `filter()`: Choose Rows


```r
# Filter by exact match
gapminder %>% 
  filter(year == 2007) %>% 
  head()
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
# Filter by multiple conditions
gapminder %>% 
  filter(year == 2007, continent == "Europe") %>% 
  head()
```

```
## # A tibble: 6 × 6
##   country                continent  year lifeExp      pop gdpPercap
##   <fct>                  <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Albania                Europe     2007    76.4  3600523     5937.
## 2 Austria                Europe     2007    79.8  8199783    36126.
## 3 Belgium                Europe     2007    79.4 10392226    33693.
## 4 Bosnia and Herzegovina Europe     2007    74.9  4552198     7446.
## 5 Bulgaria               Europe     2007    73.0  7322858    10681.
## 6 Croatia                Europe     2007    75.7  4493312    14619.
```

```r
# Filter using %in% operator
gapminder %>% 
  filter(continent %in% c('Oceania', 'Africa')) %>% 
  head()
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
# Filter by numeric conditions
gapminder %>% 
  filter(pop > 100000000, lifeExp > 70) %>% 
  head()
```

```
## # A tibble: 6 × 6
##   country   continent  year lifeExp        pop gdpPercap
##   <fct>     <fct>     <int>   <dbl>      <int>     <dbl>
## 1 Brazil    Americas   2002    71.0  179914212     8131.
## 2 Brazil    Americas   2007    72.4  190010647     9066.
## 3 China     Asia       1997    70.4 1230075000     2289.
## 4 China     Asia       2002    72.0 1280400000     3119.
## 5 China     Asia       2007    73.0 1318683096     4959.
## 6 Indonesia Asia       2007    70.6  223547000     3541.
```

### `mutate()`: Create or Modify Columns


```r
# Create new columns
gapminder_enhanced <- gapminder %>%
  mutate(
    pop_millions = pop / 1000000,
    gdp_total = pop * gdpPercap,
    pop_log = log10(pop)
  )

head(gapminder_enhanced)
```

```
## # A tibble: 6 × 9
##   country     continent  year lifeExp      pop gdpPercap pop_millions  gdp_total
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>        <dbl>      <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.         8.43    6.57e 9
## 2 Afghanistan Asia       1957    30.3  9240934      821.         9.24    7.59e 9
## 3 Afghanistan Asia       1962    32.0 10267083      853.        10.3     8.76e 9
## 4 Afghanistan Asia       1967    34.0 11537966      836.        11.5     9.65e 9
## 5 Afghanistan Asia       1972    36.1 13079460      740.        13.1     9.68e 9
## 6 Afghanistan Asia       1977    38.4 14880372      786.        14.9     1.17e10
## # ℹ 1 more variable: pop_log <dbl>
```

```r
# Conditional mutations with ifelse
gapminder %>%
  mutate(
    pop_category = ifelse(pop > 50000000, "Large", "Small")
  ) %>%
  head()
```

```
## # A tibble: 6 × 7
##   country     continent  year lifeExp      pop gdpPercap pop_category
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl> <chr>       
## 1 Afghanistan Asia       1952    28.8  8425333      779. Small       
## 2 Afghanistan Asia       1957    30.3  9240934      821. Small       
## 3 Afghanistan Asia       1962    32.0 10267083      853. Small       
## 4 Afghanistan Asia       1967    34.0 11537966      836. Small       
## 5 Afghanistan Asia       1972    36.1 13079460      740. Small       
## 6 Afghanistan Asia       1977    38.4 14880372      786. Small
```

### `transmute()`: Create New Columns (Drop Others)


```r
# Keep only the newly created columns
gapminder %>%
  transmute(
    country = country,
    year = year,
    gdp_total = pop * gdpPercap,
    pop_millions = pop / 1000000
  ) %>%
  head()
```

```
## # A tibble: 6 × 4
##   country      year    gdp_total pop_millions
##   <fct>       <int>        <dbl>        <dbl>
## 1 Afghanistan  1952  6567086330.         8.43
## 2 Afghanistan  1957  7585448670.         9.24
## 3 Afghanistan  1962  8758855797.        10.3 
## 4 Afghanistan  1967  9648014150.        11.5 
## 5 Afghanistan  1972  9678553274.        13.1 
## 6 Afghanistan  1977 11697659231.        14.9
```

### `arrange()`: Sort Rows


```r
# Sort by one column (ascending)
gapminder %>% 
  filter(year == 2007) %>%
  arrange(lifeExp) %>%
  head()
```

```
## # A tibble: 6 × 6
##   country      continent  year lifeExp      pop gdpPercap
##   <fct>        <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Swaziland    Africa     2007    39.6  1133066     4513.
## 2 Mozambique   Africa     2007    42.1 19951656      824.
## 3 Zambia       Africa     2007    42.4 11746035     1271.
## 4 Sierra Leone Africa     2007    42.6  6144562      863.
## 5 Lesotho      Africa     2007    42.6  2012649     1569.
## 6 Angola       Africa     2007    42.7 12420476     4797.
```

```r
# Sort by multiple columns (descending)
gapminder %>% 
  filter(year == 2007) %>%
  arrange(desc(gdpPercap), desc(lifeExp)) %>%
  head()
```

```
## # A tibble: 6 × 6
##   country          continent  year lifeExp       pop gdpPercap
##   <fct>            <fct>     <int>   <dbl>     <int>     <dbl>
## 1 Norway           Europe     2007    80.2   4627926    49357.
## 2 Kuwait           Asia       2007    77.6   2505559    47307.
## 3 Singapore        Asia       2007    80.0   4553009    47143.
## 4 United States    Americas   2007    78.2 301139947    42952.
## 5 Ireland          Europe     2007    78.9   4109086    40676.
## 6 Hong Kong, China Asia       2007    82.2   6980412    39725.
```

### `group_by()` and `summarise()`: Group Operations


```r
# Basic grouping and summarizing
gapminder %>% 
  filter(year == 2007) %>%
  group_by(continent) %>% 
  summarise(
    mean_lifeExp = mean(lifeExp),
    sd_lifeExp = sd(lifeExp),
    total_pop = sum(pop),
    n_countries = n(),
    .groups = 'drop'  # Automatically ungroup
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

```r
# Multiple grouping variables
gapminder %>%
  filter(year %in% c(1997, 2007)) %>%
  group_by(continent, year) %>%
  summarise(
    avg_gdp = mean(gdpPercap),
    countries = n(),
    .groups = 'drop'
  )
```

```
## # A tibble: 10 × 4
##    continent  year avg_gdp countries
##    <fct>     <int>   <dbl>     <int>
##  1 Africa     1997   2379.        52
##  2 Africa     2007   3089.        52
##  3 Americas   1997   8889.        25
##  4 Americas   2007  11003.        25
##  5 Asia       1997   9834.        33
##  6 Asia       2007  12473.        33
##  7 Europe     1997  19077.        30
##  8 Europe     2007  25054.        30
##  9 Oceania    1997  24024.         2
## 10 Oceania    2007  29810.         2
```

## Additional Essential Functions

### `rename()`: Change Column Names


```r
# Rename columns
gapminder %>%
  rename(
    life_expectancy = lifeExp,
    gdp_per_capita = gdpPercap,
    population = pop
  ) %>%
  head()
```

```
## # A tibble: 6 × 6
##   country     continent  year life_expectancy population gdp_per_capita
##   <fct>       <fct>     <int>           <dbl>      <int>          <dbl>
## 1 Afghanistan Asia       1952            28.8    8425333           779.
## 2 Afghanistan Asia       1957            30.3    9240934           821.
## 3 Afghanistan Asia       1962            32.0   10267083           853.
## 4 Afghanistan Asia       1967            34.0   11537966           836.
## 5 Afghanistan Asia       1972            36.1   13079460           740.
## 6 Afghanistan Asia       1977            38.4   14880372           786.
```

```r
# Rename with a function
gapminder %>%
  rename_with(toupper, c(country, continent)) %>%
  head()
```

```
## # A tibble: 6 × 6
##   COUNTRY     CONTINENT  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
## 4 Afghanistan Asia       1967    34.0 11537966      836.
## 5 Afghanistan Asia       1972    36.1 13079460      740.
## 6 Afghanistan Asia       1977    38.4 14880372      786.
```

### Data Type Conversions


```r
# Create sample data with mixed types
sample_data <- data.frame(
  id = c("1", "2", "3"),
  date_string = c("2020-01-01", "2020-02-01", "2020-03-01"),
  value = c("10.5", "20.3", "15.7"),
  stringsAsFactors = FALSE
)

# Convert data types
sample_data_converted <- sample_data %>%
  mutate(
    id = as.numeric(id),
    date_string = as.Date(date_string),
    value = as.numeric(value),
    id_char = as.character(id)
  )

str(sample_data_converted)
```

```
## 'data.frame':	3 obs. of  4 variables:
##  $ id         : num  1 2 3
##  $ date_string: Date, format: "2020-01-01" "2020-02-01" ...
##  $ value      : num  10.5 20.3 15.7
##  $ id_char    : chr  "1" "2" "3"
```

### `across()`: Apply Functions to Multiple Columns


```r
# Apply function to multiple columns
gapminder %>%
  group_by(continent) %>%
  summarise(
    across(c(lifeExp, pop, gdpPercap), mean),
    .groups = 'drop'
  )
```

```
## # A tibble: 5 × 4
##   continent lifeExp       pop gdpPercap
##   <fct>       <dbl>     <dbl>     <dbl>
## 1 Africa       48.9  9916003.     2194.
## 2 Americas     64.7 24504795.     7136.
## 3 Asia         60.1 77038722.     7902.
## 4 Europe       71.9 17169765.    14469.
## 5 Oceania      74.3  8874672.    18622.
```

```r
# Apply multiple functions
gapminder %>%
  filter(year == 2007) %>%
  group_by(continent) %>%
  summarise(
    across(c(lifeExp, gdpPercap), 
           list(mean = mean, sd = sd, min = min, max = max)),
    .groups = 'drop'
  )
```

```
## # A tibble: 5 × 9
##   continent lifeExp_mean lifeExp_sd lifeExp_min lifeExp_max gdpPercap_mean
##   <fct>            <dbl>      <dbl>       <dbl>       <dbl>          <dbl>
## 1 Africa            54.8      9.63         39.6        76.4          3089.
## 2 Americas          73.6      4.44         60.9        80.7         11003.
## 3 Asia              70.7      7.96         43.8        82.6         12473.
## 4 Europe            77.6      2.98         71.8        81.8         25054.
## 5 Oceania           80.7      0.729        80.2        81.2         29810.
## # ℹ 3 more variables: gdpPercap_sd <dbl>, gdpPercap_min <dbl>,
## #   gdpPercap_max <dbl>
```

### `recode()`: Recode Values


```r
# Recode values
gapminder %>%
  mutate(
    continent_short = recode(continent,
      "Africa" = "AF",
      "Americas" = "AM", 
      "Asia" = "AS",
      "Europe" = "EU",
      "Oceania" = "OC"
    )
  ) %>%
  select(country, continent, continent_short) %>%
  head()
```

```
## # A tibble: 6 × 3
##   country     continent continent_short
##   <fct>       <fct>     <fct>          
## 1 Afghanistan Asia      AS             
## 2 Afghanistan Asia      AS             
## 3 Afghanistan Asia      AS             
## 4 Afghanistan Asia      AS             
## 5 Afghanistan Asia      AS             
## 6 Afghanistan Asia      AS
```

### `case_when()`: Multiple Conditional Logic


```r
# Multiple conditions with case_when
gapminder %>%
  mutate(
    development_level = case_when(
      gdpPercap < 1000 ~ "Low income",
      gdpPercap < 5000 ~ "Lower middle income", 
      gdpPercap < 15000 ~ "Upper middle income",
      gdpPercap >= 15000 ~ "High income",
      TRUE ~ "Unknown"  # Default case
    ),
    pop_size = case_when(
      pop < 10000000 ~ "Small",
      pop < 50000000 ~ "Medium",
      pop >= 50000000 ~ "Large"
    )
  ) %>%
  select(country, year, gdpPercap, development_level, pop, pop_size) %>%
  filter(year == 2007) %>%
  head(10)
```

```
## # A tibble: 10 × 6
##    country      year gdpPercap development_level         pop pop_size
##    <fct>       <int>     <dbl> <chr>                   <int> <chr>   
##  1 Afghanistan  2007      975. Low income           31889923 Medium  
##  2 Albania      2007     5937. Upper middle income   3600523 Small   
##  3 Algeria      2007     6223. Upper middle income  33333216 Medium  
##  4 Angola       2007     4797. Lower middle income  12420476 Medium  
##  5 Argentina    2007    12779. Upper middle income  40301927 Medium  
##  6 Australia    2007    34435. High income          20434176 Medium  
##  7 Austria      2007    36126. High income           8199783 Small   
##  8 Bahrain      2007    29796. High income            708573 Small   
##  9 Bangladesh   2007     1391. Lower middle income 150448339 Large   
## 10 Belgium      2007    33693. High income          10392226 Medium
```

### `replace_na()`: Handle Missing Values


```r
# Create data with missing values
data_with_na <- data.frame(
  name = c("Alice", "Bob", NA, "David"),
  age = c(25, NA, 35, 40),
  city = c("New York", "Boston", "Chicago", NA)
)

print("Original data:")
```

```
## [1] "Original data:"
```

```r
data_with_na
```

```
##    name age     city
## 1 Alice  25 New York
## 2   Bob  NA   Boston
## 3  <NA>  35  Chicago
## 4 David  40     <NA>
```

```r
# Replace NA values
data_cleaned <- data_with_na %>%
  mutate(
    name = replace_na(name, "Unknown"),
    age = replace_na(age, median(age, na.rm = TRUE)),
    city = replace_na(city, "Not specified")
  )

print("After replacing NA values:")
```

```
## [1] "After replacing NA values:"
```

```r
data_cleaned
```

```
##      name age          city
## 1   Alice  25      New York
## 2     Bob  35        Boston
## 3 Unknown  35       Chicago
## 4   David  40 Not specified
```

### `which()`: Find Positions


```r
# Using which to find positions
sample_vector <- c(10, 25, 30, 15, 40, 35)

# Find positions where condition is TRUE
positions <- which(sample_vector > 20)
print(paste("Positions where value > 20:", paste(positions, collapse = ", ")))
```

```
## [1] "Positions where value > 20: 2, 3, 5, 6"
```

```r
# Use in data frame context
gapminder %>%
  filter(year == 2007) %>%
  mutate(rank_gdp = rank(desc(gdpPercap))) %>%
  filter(rank_gdp <= 5) %>%
  select(country, gdpPercap, rank_gdp) %>%
  arrange(rank_gdp)
```

```
## # A tibble: 5 × 3
##   country       gdpPercap rank_gdp
##   <fct>             <dbl>    <dbl>
## 1 Norway           49357.        1
## 2 Kuwait           47307.        2
## 3 Singapore        47143.        3
## 4 United States    42952.        4
## 5 Ireland          40676.        5
```

## Practical Example: Data Analysis Workflow

This example analyzes life expectancy patterns across continents, decades, and economic development levels using the gapminder dataset. The goal is to uncover how life expectancy varies when we consider geographic location, time period (1990s vs 2000s), and economic status (GDP per capita categories) simultaneously. The result is a structured summary showing average life expectancy for different combinations of these factors, filtered to ensure statistical reliability.

**workflow**

- create categorical variables for time periods and GDP levels 
- group by continent, decade, and GDP category
- calculate summary statistics 
- remove unreliable small groups 
- arrange results for easy interpretation.


```r
# Comprehensive analysis: Life expectancy trends by continent
life_exp_analysis <- gapminder %>%
  # Filter for recent decades
  filter(year >= 1990) %>%
  # Create additional variables
  mutate(
    decade = case_when(
      year %in% 1990:1999 ~ "1990s",
      year %in% 2000:2007 ~ "2000s"
    ),
    gdp_category = case_when(
      gdpPercap < 2000 ~ "Low GDP",
      gdpPercap < 10000 ~ "Medium GDP", 
      gdpPercap >= 10000 ~ "High GDP"
    )
  ) %>%
  # Group and summarize
  group_by(continent, decade, gdp_category) %>%
  summarise(
    avg_life_exp = mean(lifeExp),
    countries = n(),
    total_pop = sum(pop),
    .groups = 'drop'
  ) %>%
  # Filter out small groups
  filter(countries >= 3) %>%
  # Arrange results
  arrange(continent, decade, desc(avg_life_exp))

print("Life expectancy analysis by continent, decade, and GDP category:")
```

```
## [1] "Life expectancy analysis by continent, decade, and GDP category:"
```

```r
life_exp_analysis
```

```
## # A tibble: 21 × 6
##    continent decade gdp_category avg_life_exp countries  total_pop
##    <fct>     <chr>  <chr>               <dbl>     <int>      <dbl>
##  1 Africa    1990s  Medium GDP           61.9        28  381335877
##  2 Africa    1990s  Low GDP              50.3        74 1019466696
##  3 Africa    2000s  Medium GDP           59.7        27  632430409
##  4 Africa    2000s  High GDP             58.5         7   13862646
##  5 Africa    2000s  Low GDP              51.5        70 1116970553
##  6 Americas  1990s  High GDP             75.1        10  689423253
##  7 Americas  1990s  Medium GDP           69.9        38  833511034
##  8 Americas  2000s  High GDP             76.3        15  976865109
##  9 Americas  2000s  Medium GDP           72.3        33  755668372
## 10 Asia      1990s  High GDP             75.0        21  479574324
## # ℹ 11 more rows
```

## Function Summary Table

Here's a comprehensive summary of all the tidyverse functions covered:

| Function | Description | Example |
|----------|-------------|---------|
| `select()` | Choose specific columns from a dataset | `select(country, year, pop)` |
| `filter()` | Choose specific rows based on conditions | `filter(year == 2007, pop > 1000000)` |
| `mutate()` | Create new columns or modify existing ones | `mutate(gdp_total = pop * gdpPercap)` |
| `transmute()` | Create new columns and drop all others | `transmute(country, gdp_billions = gdp/1e9)` |
| `arrange()` | Sort rows by one or more columns | `arrange(desc(lifeExp))` |
| `group_by()` | Group rows for grouped operations | `group_by(continent, year)` |
| `summarise()` | Calculate summary statistics for groups | `summarise(mean_life = mean(lifeExp))` |
| `rename()` | Change column names | `rename(life_expectancy = lifeExp)` |
| `pivot_longer()` | Convert wide data to long format (tidy) | `pivot_longer(cols = c(col1, col2))` |
| `pivot_wider()` | Convert long data to wide format | `pivot_wider(names_from = key)` |
| `across()` | Apply functions to multiple columns at once | `across(c(col1, col2), mean)` |
| `recode()` | Recode values in a column | `recode(continent, 'Asia' = 'AS')` |
| `case_when()` | Handle multiple conditional logic | `case_when(pop < 1e6 ~ 'Small')` |
| `replace_na()` | Replace missing (NA) values | `replace_na(column, 'Unknown')` |
| `as.numeric()` | Convert to numeric data type | `as.numeric(character_column)` |
| `as.character()` | Convert to character data type | `as.character(numeric_column)` |
| `which()` | Find positions where condition is TRUE | `which(vector > threshold)` |


## Conclusion

The tidyverse provides a consistent and intuitive grammar for data manipulation in R. By mastering these core functions, you'll be able to efficiently clean, transform, and analyze data. The key principles to remember are:

1. **Start with tidy data** - each variable in a column, each observation in a row
2. **Use the pipe operator (`%>%`)** to chain operations together
3. **Combine functions** to create powerful data manipulation workflows
4. **Group operations** allow for sophisticated analyses across different categories
