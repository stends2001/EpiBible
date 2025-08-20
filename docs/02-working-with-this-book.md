# Working with this book

Before diving into the coding content, this chapter explains how this book is organized and how you can make th;e most of the examples and code snippets provied throughout this book.

## What is bookdown?
This book is written using **bookdown**, a pakage in R that helps create digital books and long-form documents using R Markdown. Bookdowns allow us to do multiple things, in one clean and organized document:

- combine text and executable R code in one place
- automatically generates formatted output like HTML, PDF or e-books
- Cross-reference chapters, figures tables and equations
- provide code snippets that any reader can copy paste into executable code chunks for easy practice.

## What is Markdown
The continent you see here is written in **Markdown**, a simple plain-text formatting syntax that's easy to write and read. Markdown allows you to add:

- Headings and subheadings
- Formatted text (bold, italic)

## Code snippets
Throughout this book you will find **code chunks** like the one on the next few lines. In grey and italic you will see comments, denoted by starting with a hashtag ('#'). Any normal looking text is executable code. Upon generation of this  bookdown, all code chunks that are written are executed. Below each chunk, you find another grey background box, which holds the output of that chunk.

```r
# This is a comment
print("this is executable code!")
```

```
## [1] "this is executable code!"
```

Because these chunks are literally the code that I have written myself, you can simply copy paste this into your system, and thereby run the same code.

## Other formats in this book
There's a few common text-formats. I'll be using these:

- bold: **terminology**
- italic: *variables in functions*
- inline code: `functions`
