---
title: "Class 04: Data Basics"
author: "Taylor Arnold"
output: html
---



## Learning Objectives

- Describe the two components of tabular data (observations and variables)
- Identify a comma seperated values (CSV) file
- Create tabular data in a spreadsheet and export to CSV
- Read a local CSV file into R with `read_csv`

## Tabular data formats

In this course we will store data in a tabular format.
These tables will have **observations** stored in rows and
**variables** stored in columns. The individual elements are
called **values**. So, each row represents a
particular object in our dataset and each column represents
some feature of the objects.

![](../assets/img/tidy-1.png)

Let's look at the births dataset again from last time:


{% highlight r %}
library(readr)

births <- read_csv("https://statsmaths.github.io/stat_data/arbuthnot.csv")
births
{% endhighlight %}



{% highlight text %}
## # A tibble: 82 x 6
##    head_of_state  year  boys girls total boy_to_girl_ratio
##    <chr>         <int> <int> <int> <int>             <dbl>
##  1 Charles I      1629  5218  4683  9901              1.11
##  2 Charles I      1630  4858  4457  9315              1.09
##  3 Charles I      1631  4422  4102  8524              1.08
##  4 Charles I      1632  4994  4590  9584              1.09
##  5 Charles I      1633  5158  4839  9997              1.07
##  6 Charles I      1634  5035  4820  9855              1.04
##  7 Charles I      1635  5106  4928 10034              1.04
##  8 Charles I      1636  4917  4605  9522              1.07
##  9 Charles I      1637  4703  4457  9160              1.06
## 10 Charles I      1638  5359  4952 10311              1.08
## # ... with 72 more rows
{% endhighlight %}

The observations here are *years* and the variables are: `head_of_state`,
`year`, `boys`, `girls`, `total`, and `boy_to_girl_ratio`. Each variable
measures something about a given observation. What exactly
constitutes a row of the data is called a **unit of analysis**.
Keeping in mind what the unit of analysis is will be very
important as we think about how data is being used.

## Comma separated values

The type of R object that stores such a dataset is called a
**data frame**. Data frames store tabular data for us within R. We also
need a way to store tabular data as a file. One option would be to store
data as Google sheets or Excel file. While these programs are
great for data input, it is not a good idea to store raw data in
these formats. Instead, we want a minimal **plain text** format.
That is, the file should be readable by any basic text editor.

The plan text data format that we will use is called a
**comma-separated values** or CSV file. In this format, each
column of the data is (as the name suggests) separated by a
comma. Every observation is stored on its own row. The first
row gives the names of the columns. Here are what the first few
rows of the births dataset look like stored as a CSV file:

```
head_of_state,year,boys,girls,total,boy_to_girl_ratio
Charles I,1629,5218,4683,9901,1.114
Charles I,1630,4858,4457,9315,1.09
Charles I,1631,4422,4102,8524,1.078
Charles I,1632,4994,4590,9584,1.088
Charles I,1633,5158,4839,9997,1.066
Charles I,1634,5035,4820,9855,1.045
```

A CSV file can be created and read by R, Excel, GoogeSheets,
OpenOffice, and most other data processing and statistical tools.
It is one of the most common formats that you will see data stored
as online. By convention, a CSV file has the extension ".csv".

As we have seen, we can read a dataset using the `read_csv` function. The
function either takes a URL, as we have above, or a path to the file on your
computer. We will test out the second example now.

## Activity: Data creation

We are now going to collect some data as a class. Specifically, you will each
record information about your six favorite restaurants:

  - name of the restaurant
  - location
  - cuisine
  - favorite dish
  - cost of a typical meal per person
  - how many times you visit each year
  - last time that you visited

Let's start by doing this individually in Google Sheets (I'll explain these
steps in person). Once you are done, download the dataset as a CSV file.

## Reading in a local file

Once you have downloaded the file, rename it to `my_restaurants.csv`, and
place it wherever you save your RMarkdown files. Then, download the fourth
lab here:

<a href="https://raw.githubusercontent.com/statsmaths/stat209-f18/master/labs/lab04.Rmd" download="lab04.Rmd" target="_blank">lab04.Rmd</a>

Save the lab in the same location and open the file in RStudio. We will work
on the first few questions together.

## Combining the data

It will be much more interesting if we can combine the data from everyone in
the class. Use the links here to link into the collective Google Sheet and
add your data to the appropriate sheet:

- [Favorite Restaurants](https://docs.google.com/spreadsheets/d/10LFQMcRRBiRXEauX1aUfv3dYhiJ4zS616BwyDnXC3kc/edit?usp=sharing)

Once we are finished with the, you will then download the entire class' data
as a single file. Download it and name the file `class_restaurants.csv`. Load
it into R as a dataset called `class`.

## Consistency

Depending on how the class goes, we may find that there are inconsistencies in
how the data is formatted from student to student. Of most importance is that
if any value in a variable does not look like a number the entire column will
be considered as a categorical variable. With the time remaining we will try
to adjust this.

Next week we will dig deeper in to the specific types of data in R and how the
effect our graphics and analyses.

## New variables (time remaining)

If we have time remaining, we will add additional variables to our dataset
such as the latitude and longitude of the restaurant.
