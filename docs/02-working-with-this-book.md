# Working with this book

Before diving into the coding content, this chapter explains how this document is organized and how you can make the most of the examples and code snippets provided throughout this reader.

## What is bookdown?
This book is written using **bookdown**, a package in R that helps create digital books and long-form documents using R Markdown. Bookdowns allow us to do multiple things, in one clean and organized document:

- combine text and executable R code in one place
- automatically generates formatted output like HTML, PDF or e-books
- Cross-reference chapters, figures tables and equations
- provide code snippets that any reader can copy paste into executable code chunks for easy practice.

## Code snippets
Throughout this book you will find **code chunks** like the one on the next few lines. In grey and italic you will see comments, denoted by starting with a hashtag (`#`). Any normal looking text is executable code. Upon generation of this document, all code chunks that are written are executed. The code found in these chunks can be copied, and pasted into your own R. To do so, hover over the top right corner of the chunk, and the option to copy should pop up.


```r
# This is a comment
print("this is executable code!")
```

```
## [1] "this is executable code!"
```

Below each chunk, you find another grey background box, which holds the output of that chunk. These boxes contain text that start with `## [1]` and so on. This is the way of R to show the index of the first element in that row of printed output. It helps you orient yourself in long outputs. If we print 100 numbers for example, you'll see a number of these indices showing up, by which R tells us that the 73rd output was the number 73, etc.


```r
print(1:100)
```

```
##   [1]   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18
##  [19]  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35  36
##  [37]  37  38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53  54
##  [55]  55  56  57  58  59  60  61  62  63  64  65  66  67  68  69  70  71  72
##  [73]  73  74  75  76  77  78  79  80  81  82  83  84  85  86  87  88  89  90
##  [91]  91  92  93  94  95  96  97  98  99 100
```

## A Hand's-on Approach
You can't learn a programming language from reading, similarly to how you can't learn a language by only reading. You need to speak, and to write as well, to make sure you find our own way of speaking the language, and so that you can practice what you learned in theory! We will be following the same philosophy. For now, this document is mostly meant as a reader, but soon enough there will be exercises in here as well, at the end of each chapter, so that you get to practice what you learned. 

**Text formats in this Reader**

There's a few common text-formats. I'll be using these:

- bold: **terminology**
- italic: *variables in functions*
- inline code: `functions`
