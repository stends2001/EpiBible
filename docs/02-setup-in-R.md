# Chapter 2 Getting Started with R

## Installing R and Rstudio

R is not a computer program like Microsoft Word. Instead, it is a computer language, just like Python, Matlab, Java and many others. You can tell your computer to execute commands, by writing the commands in such a computer language. RStudio is an interface, an application, that functions as a tool to help you write, understand and organize your R code. In order to get R up and running on your system we therefore need to install both R and Rstudio. Follow the steps on this [website](https://posit.co/download/rstudio-desktop/) specifically for your system (Linux, Mac, Windows).

After installation, you can open the application RStudio and in it run Rcode. You'll notice 4 sections in RStudio:

- script editor (top left); this is where we will do most work. We will write and edit code in this place
- console (bottom left); this is where we execute code. while you can run R commands here directly, we will execute code here written in the script editor.
- environment pane (top right); shows all the current variables, datasets and anything else in the working memory
- file pane (bottom right); allows you to browse overfiles


The R language is incredibly large and diverse. Any user of R can extend the language with new functions and new tools, which come together in the shape of a package. There are about 20 thousand (!) R packages available, each of which has their own speciality. When some clever person creates a new epidemiological model and wants it to become available for the community, they can upload it on R and make it available for us. While the volume of packages make it sound like you can easily lose track of everything that's out there, there's only a handful of packages that really everyone tends to use. For example, the **Tidyverse** is a collection of packages that many people workign with R have experience with. We will also be using it a lot in this course, specifically the packages **ggplot2** (for visualization) **dplyr** (for data manipulation), **readr** (for reading datafiles) and **tidyr** (for reshaping dataframes).

We will go through installing and loading packages, and writing functions later in this course. For now we can get started on some very simple things in R, to get you familiar with doing some coding!

## Starting to code

The `print()` function is a very basic and useful one, which allows you to often check some things quickly. On the first line in the script editor, go and write the following line:


```r
print('hello world!')
```

```
## [1] "hello world!"
```


with your cursor on the right side of that line, press the "run"-button. You will see the output in the console where it now shows in yellow what has been executed, and in white the output is shown.

R is great for doing some easy mathematics. Let's do some calculations and print the output:


```r
x = 10
y = 280
z = 0.4
product = x*y*z
print(product)
```

```
## [1] 1120
```

Here we have multiple lines of code, that we need to execute in that order for it to work. We can't first run the print, and then do the calculations. We can either do that line by line, so press the run button each time. Alternatively, you can select all these lines, and then press the run button. Another option is to use the shortcut (Windows?) CTRL + Enter. This is the equivalent of the Run button. So select all the lines, and press CTRL + Enter

We will discuss more R syntax, and more thigns to code, at a later point. For now let's leave it at this and talk a bit about how we organize our code.
## R Projects and Organization

We can save our code in an R script, which is the most basic R file. To do that, we go CTR + S and can navigate to the location to save our file. Besides this R file (extension: .R) we have a couple of other options, including an R Markdown. I am a huge fan of these. If you're familiar with Python; R Markdown - files are the R equivalent of a jupyter notebook file. They allow you to write code in the same file as nicely organized text. You can put in graphs that look really nice, add citations and a bibliography, add chapters, and so much more. This whole textbook is actually running in a handful of R Markdown files. Eventually, I want to write a small project with you in an R Markdown as well, though for now we can get started with R scripts.

To organize our R code properly, we shouldn't work with individual R scripts. It is much better to have a specific environment for your R code, in which you store your analysis (code), your data and your results and figures. Something I struggle a lot with myself is with directories. A directory is a certain folder's location, a way of pinpointing where R is working (the workign directory). This is important to know, because when we tell R where to find a certain dataset on our computer, the computer needs to know the starting point of our trip.

To fix all these issues, we have something called an R project. This is exactly an environment to bundle different R files in a seperate folder, where the working directory is atuomatically set to the root of the project; the location of the project file. We create an R project through the following steps:

- File > New Project
- A window will pop up asking to Save the current workspace:
        - Don't save
- New Directory > New Project
- In Directory name: write "epiproject_00"
- In Create project as a subdirectory of:
        - press browse, and navigate to a place on hyour computer you would like to store your R codes. I suggest this NOT TO BE your Downloads or Desktop folder.
- Create Project


Now that we're in a project, open a new R script. Save it in the project under the name "00_syntax" and let's set started in the next chapter.
