# R fundamentals

## Basic Datatypes in R

When coding, we work with variables. Say we start our code with `x=1`, then x is a variable. We can do some calculations with `x`, and we can change `x`, depending on the type of data that the variable is. In R, variables can represent the following **datatypes**: 

| Type      | Description    | Example         |
| --------- | -------------- | --------------- |
| Numeric   | Real number    | `3.14`, `-6.7`  |
| Integer   | Whole number   | `1`, `4`, `16`  |
| Character | Text or string | `a`, `hello!`   |
| Logical   | Boolean        | `TRUE`, `FALSE` |

The specification of a datatype is very important. While some datatypes may allow certain functionality, others may not. We can sum two numeric variables, which we can also do for two integers. When we try to add two characters to each other though, R will throw an error. Have a look yourself.

**summing two numeric datatypes**

```r
x = 1.1
y = 2.9
print(x+y)
```

```
## [1] 4
```

**summing two characters**

```r
text1 = 'hello'
text2 = 'there!'
# print(text1+text2) -> this will throw an error!
```

While this doesn't work, there are ways of combining, or rather **concatenating** two strings, namely using `paste()` and `paste0()`:

```r
paste(text1, text2) 
```

```
## [1] "hello there!"
```

```r
paste0(text1, text2)   
```

```
## [1] "hellothere!"
```


## Special datatypes
Besides the basic types of variables mentioned below, R also supports some special datatypes:
| Type    | Description                         | Example                 |
| ------- | ----------------------------------- | ----------------------- |
| Date    | Calendar date                       | `as.Date('2025-08-01')` |
| NA      | Missing value                       |                         |
| NULL    | Empty object                        |                         |
| (-) Inf | Positive or negative infinity       | `1/0`, `-1/0`           |
| NaN     | Not a Number: undefined mathematics | `0/0`                   |

When working with dates, it is very handy to convert what is normally read as text (so "2025-01-01") into Date because there is some additional functionality for resampling dates, and for plotting dates. The others, so `NA`, `NULL`, `INF` and `NaN` often show something is going wrong in the process, so watch out for these!


## Converting between datatypes

It is often necessary to convert data from one type to another. This process is called **type conversion** or **casting**.


```r
x <- "123"
num_x <- as.numeric(x)   
print(num_x + 10)       
```

```
## [1] 133
```

For conversion, watch out for these things:
- NA introduction: if conversion fails, R returns an NA=> `as.numeric("hello")`
- Loss of information: `as.integer(3.99)` will return 3, not 4!
- Convering booleans: `as.numeric(TRUE)` returns and `as.numeric(FALSE)` returns 0

## Data structures in R

While it's nice to be able to work with variables and single datapoints, more often than not we want to work with objects that include multiple data points, and sometimes even different datatypes. These we call **data structures**. In R, we have the following ones:

| Structure  | Description                                                                                                                                             | Example                                                                          |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| List       | Collection of any datatypes                                                                                                                             | `list1 = ['A', 1, TRUE]`                                                         |
| Vector     | Sequence of elements of the same datatype                                                                                                               | `vector1 = c(1,2,3)`<br>`vector2 = c('A','B','C')`                               |
| Matrix     | 2-Dimensional - vector: one object of strictly the same datatype                                                                                        | `matrix1 = matrix(1:6, nrow=2)`                                                  |
| Array      | Multi-Dimensional matrix equivalent                                                                                                                     |                                                                                  |
| Factor     | A special type of vector: one with categorical values.<br>Internally, these values are then seen as a limited number of distinct groups (called levels) | `vector_levels = c('high','medium','high')`<br>`factor1 = factor(vector_levels)` |
| Data frame | Table-like structure with columns that can be mixed datatypes.<br>A single column has one specific datatype                                             |                                                                                  |
The **Data frame** is particularly important. Any excel table that we open in R is put into a Data frame.

### Vectors: The Building Blocks
Vectors are the most fundamental data structure in R. Even when you assign a single number like x <- 5, you're actually creating a vector with one element! Here's how to work with them:
Creating vectors:


```r
# Numeric vector
numbers <- c(1, 2, 3, 4, 5)

# Character vector  
names <- c("Alice", "Bob", "Charlie")

# Logical vector
results <- c(TRUE, FALSE, TRUE, TRUE)

# Using sequences
sequence1 <- 1:10           # Numbers 1 to 10
sequence2 <- seq(0, 1, 0.1) # From 0 to 1 in steps of 0.1
```

Vector operations:
One of R's powerful features is vectorization - operations apply to entire vectors at once:

```r
numbers <- c(1, 2, 3, 4)
numbers * 2        # Multiply all elements by 2: (2, 4, 6, 8)
```

```
## [1] 2 4 6 8
```

```r
numbers + c(10, 20, 30, 40)  # Add corresponding elements: (11, 22, 33, 44)
```

```
## [1] 11 22 33 44
```

### Lists: Mixed Data Collections
Lists are like vectors but can hold different datatypes. Think of them as containers where each "slot" can hold anything:

```r
# Creating a list with mixed datatypes
mixed_list <- list(
  name = "John",
  age = 25,
  married = TRUE,
  children = c("Emma", "Liam")
)

# You can also create lists without names
simple_list <- list("text", 42, TRUE)
```

When to use lists vs vectors:

Vectors: When all your data is the same type (all numbers, all text, etc.)
Lists: When you need to store different types together, or when you want to store complex objects

### Data Frames: The Excel of R
Data frames are like Excel spreadsheets - they have rows and columns, where each column can be a different datatype but within a column, all values must be the same type.
Creating a data frame:

```r
# Creating a simple data frame
students <- data.frame(
  name = c("Alice", "Bob", "Charlie"),
  age = c(20, 21, 19),
  grade = c("A", "B", "A+"),
  passed = c(TRUE, TRUE, TRUE)
)

print(students)
```

```
##      name age grade passed
## 1   Alice  20     A   TRUE
## 2     Bob  21     B   TRUE
## 3 Charlie  19    A+   TRUE
```

### Indexing: Accessing Your Data
Once you have data stored in these structures, you need to be able to get specific pieces out. This is called indexing or subsetting.

Position-based indexing (using square brackets []):

```r
# indexing - vectors
numbers_vector <- c(10, 20, 30, 40, 50)

numbers_vector[1]        # First element: 10
```

```
## [1] 10
```

```r
numbers_vector[3]        # Third element: 30
```

```
## [1] 30
```

```r
numbers_vector[c(1, 3)]  # First and third elements: 10, 30
```

```
## [1] 10 30
```

```r
# indexing - lists
mixed_list <- list(1, "2", 6.7)
mixed_list[1]          # Returns a list containing just the name
```

```
## [[1]]
## [1] 1
```

```r
mixed_list[2]
```

```
## [[1]]
## [1] "2"
```

```r
# indexing - data frame
students <- data.frame(
  name = c("Alice", "Bob", "Charlie"),
  age = c(20, 21, 19),
  grade = c("A", "B", "A+")
)

# Matrix-style indexing [row, column]
students[1, 2]         # First row, second column: 20
```

```
## [1] 20
```

```r
students[1, ]          # Entire first row
```

```
##    name age grade
## 1 Alice  20     A
```

```r
students[, 2]          # Entire second column (ages)
```

```
## [1] 20 21 19
```

```r
students[1:2, c("name", "age")]  # First two rows, name and age columns
```

```
##    name age
## 1 Alice  20
## 2   Bob  21
```

```r
# List-style indexing (by column)
students$name          # The name column
```

```
## [1] "Alice"   "Bob"     "Charlie"
```

```r
students[["age"]]      # The age column
```

```
## [1] 20 21 19
```

```r
students["grade"]      # The grade column (returns data frame)
```

```
##   grade
## 1     A
## 2     B
## 3    A+
```

```r
# Logical indexing
students[students$age > 20, ]        # All rows where age > 20
```

```
##   name age grade
## 2  Bob  21     B
```

```r
students[students$grade == "A", ]    # All rows where grade is "A"
```

```
##    name age grade
## 1 Alice  20     A
```

Understanding these data structures and how to index them is crucial because almost everything you do in R involves manipulating data stored in these formats. Whether you're reading a CSV file (which becomes a data frame), performing calculations (often on vectors), or organizing complex results (in lists), these concepts form the foundation of effective R programming.


## Conditions and Comparisons

One thing we often need to do with our datatypes, are **comparisons**. This is basically checking whether a statement is `TRUE` or `FALSE`. Here are the basic **comparison operators** in R:

| Operator  | Meaning                  | Example         | Result  |
| --------- | ------------------------ | --------------- | ------- |
| &nbsp; == | Equal to                 | `3 == 3`        | `TRUE`  |
| `!=`      | Not equal to             | `3 != 5`        | `TRUE`  |
| `>`       | Greater than             | `4 > 2`         | `TRUE`  |
| `<`       | Less than                | `4 < 2`         | `FALSE` |
| `>=`      | Greater than or equal to | `4 >= 4`        | `TRUE`  |
| `<=`      | Less than or equal to    | `4 <= 3`        | `FALSE` |
| `%in%`    | Element inside an object | `1 %in% c(1,2)` | `TRUE` |

And for combining logical statements, we use **operators**, of which these are available:

| Operator  | Meaning | Example        | Result  |
| --------- | ------- | -------------- | ------- |
| `&`       | AND     | `TRUE & FALSE` | `FALSE` |
| `|`       | OR      | `TRUE | FALSE` | `TRUE`  |
| `!`       | NOT     | `!FALSE`       | `TRUE`  |


##  Conditional execution with 
Sometimes we only want to execute a piece of code under a certain condition. For that, we have what is called an **if-statement**. In R we write `if (...comparison...){code to be executed}`.


The basic if statement:

```r
x <- 4

if (x > 0) {
  print("x is positive")
}
```

```
## [1] "x is positive"
```

Besides the `if` part, we can also add an `else` part, which will be executed if the comparison is `FALSE`.

`if ... else`

```r
x <- -1
if (x > 0) {
  print("x is positive")
} else {
  print("x is not positive")
}
```

```
## [1] "x is not positive"
```
## Repeating lines of code

Sometimes, we want to execute a piece of code a certain number of times, or we want to execute the same code based on a bunch of different variables. We can repeat code using a `for - loop`. Basically, we write `for (iteration) {code to be executed}`. For example, let's print the numbers 1 to 5. For that to be done, first we write a vector from 1 to 5, which we do through `1:5`. Then we iterate through all the elements in that vector by:


```r
for (i in 1:5) { # for i = 1 until i = 5
  print(i)
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```

That was a classical example where we iterate through a bunch of integers. We can also iterate through other things:


```r
names = c('Marie Curie', 'Jenniffer Doudna', 'Alexander Flemming')

for (name in names) {
  print(paste(name, "won the Nobel Prize"))
}
```

```
## [1] "Marie Curie won the Nobel Prize"
## [1] "Jenniffer Doudna won the Nobel Prize"
## [1] "Alexander Flemming won the Nobel Prize"
```

## Functions
People write code into functions in R. These are sometimes added into packages and published, so that anyone using R can use them. Examples of functions are `paste()`, `log()` (taking the logarithm base 10 of a number) and many others. When we make our own code, we sometimes end up writing huge chunks that we want to repeat, but can't really put in a for loop. For that we can write our own functions! Here I'll show an example testing whether or not our variable is negative or positive:


```r
positive_check <- function(x) { # the name of the function is positive_or_negative, with the parameter 'x'
  if (x < 0) {                        # if x is smaller than 0
    return(FALSE)                     # the output of the function is FALSE
  } else {                            # otherwise (if x is larger than, or equal to, 0)
    return(TRUE)                      # return TRUE
  }
}
```

And now that we've defined this function we can easily use it by `positive_check(1)`, for example:

```r
positive_check(1)
```

```
## [1] TRUE
```

```r
positive_check(Inf)
```

```
## [1] TRUE
```

