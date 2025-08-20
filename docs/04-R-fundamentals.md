# R fundamentals

## R syntax

**variable types**

**comparisons**

**if-statements**

**for loops**

**functions**


## Troubleshooting

## Data structures

**vectors/matrices/dataframes**

```r
# Initiating a matrix with numbers
Matrix1     = data.frame(matrix(nrow=3,
                                ncol=2))
Column1     = c(1,2,3)
Column2     = seq(2,6,2)
Matrix1[,c(1,2,3)] = c(Column1,Column2,Column2)
# Oops! we duplicated a column! Now let's remove the third column
Matrix1     = Matrix1[,-3]
rownames(Matrix1) = c("a","b","c")
colnames(Matrix1) = c("time","value")

# Extracting data
Matrix1$time
```

```
## [1] 1 2 3
```

**data importing and exporting**




## writing scripts

**file - organization**
