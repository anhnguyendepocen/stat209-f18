---
title: "Class 06: Tidy Data"
author: "Taylor Arnold"
output: html
---





## Learning Objectives

- Identify the unit of observation of a data set
- Describe the 'tidy data' format
- Collect data as a structured tidy dataset
- Use `left_join` to combine linked tables in a tidy data schema

## Unit of Observation

By now you have had several opportunities to practice building a structured
tabular dataset. Today we will see one more extension of the methods we have
so far developed. This change will allow us to accurately store and describe
nearly any structured dataset. Before we get there, it will be useful to define
a concept called the **unit of observation**.

In short, the unit of observation describes what type of element is described
by a single row of the data. Here is a dataset that records specific commercial
flights from one of the three NYC airports in 2013:


{% highlight r %}
flights
{% endhighlight %}



{% highlight text %}
## # A tibble: 272,870 x 8
##     year month   day dep_time arr_time tailnum origin dest 
##    <int> <int> <int>    <int>    <int> <chr>   <chr>  <chr>
##  1  2013     1     1      517      830 N14228  EWR    IAH  
##  2  2013     1     1      533      850 N24211  LGA    IAH  
##  3  2013     1     1      542      923 N619AA  JFK    MIA  
##  4  2013     1     1      554      812 N668DN  LGA    ATL  
##  5  2013     1     1      554      740 N39463  EWR    ORD  
##  6  2013     1     1      555      913 N516JB  EWR    FLL  
##  7  2013     1     1      557      709 N829AS  LGA    IAD  
##  8  2013     1     1      557      838 N593JB  JFK    MCO  
##  9  2013     1     1      558      849 N793JB  JFK    PBI  
## 10  2013     1     1      558      853 N657JB  JFK    TPA  
## # ... with 272,860 more rows
{% endhighlight %}

The unit of observation here is a **flight**. Likewise, here is a dataset
describing information about specific planes:


{% highlight r %}
planes
{% endhighlight %}



{% highlight text %}
## # A tibble: 3,322 x 8
##    tailnum type          manufacturer   model  engines seats speed engine 
##    <chr>   <chr>         <chr>          <chr>    <int> <int> <int> <chr>  
##  1 N10156  Fixed wing m… EMBRAER        EMB-1…       2    55    NA Turbo-…
##  2 N102UW  Fixed wing m… AIRBUS INDUST… A320-…       2   182    NA Turbo-…
##  3 N103US  Fixed wing m… AIRBUS INDUST… A320-…       2   182    NA Turbo-…
##  4 N104UW  Fixed wing m… AIRBUS INDUST… A320-…       2   182    NA Turbo-…
##  5 N10575  Fixed wing m… EMBRAER        EMB-1…       2    55    NA Turbo-…
##  6 N105UW  Fixed wing m… AIRBUS INDUST… A320-…       2   182    NA Turbo-…
##  7 N107US  Fixed wing m… AIRBUS INDUST… A320-…       2   182    NA Turbo-…
##  8 N108UW  Fixed wing m… AIRBUS INDUST… A320-…       2   182    NA Turbo-…
##  9 N109UW  Fixed wing m… AIRBUS INDUST… A320-…       2   182    NA Turbo-…
## 10 N110UW  Fixed wing m… AIRBUS INDUST… A320-…       2   182    NA Turbo-…
## # ... with 3,312 more rows
{% endhighlight %}

The unit of observation of this dataset is an individual plane. Or, in this
dataset:


{% highlight r %}
airports
{% endhighlight %}



{% highlight text %}
## # A tibble: 100 x 8
##    faa   name                     lat    lon   alt    tz dst   tzone      
##    <chr> <chr>                  <dbl>  <dbl> <int> <int> <chr> <chr>      
##  1 ABQ   Albuquerque Internati…  35.0 -107.   5355    -7 A     America/De…
##  2 ACK   Nantucket Mem           41.3  -70.1    48    -5 A     America/Ne…
##  3 ALB   Albany Intl             42.7  -73.8   285    -5 A     America/Ne…
##  4 ANC   Ted Stevens Anchorage…  61.2 -150.    152    -9 A     America/An…
##  5 ATL   Hartsfield Jackson At…  33.6  -84.4  1026    -5 A     America/Ne…
##  6 AUS   Austin Bergstrom Intl   30.2  -97.7   542    -6 A     America/Ch…
##  7 AVL   Asheville Regional Ai…  35.4  -82.5  2165    -5 A     America/Ne…
##  8 BDL   Bradley Intl            41.9  -72.7   173    -5 A     America/Ne…
##  9 BGR   Bangor Intl             44.8  -68.8   192    -5 A     America/Ne…
## 10 BHM   Birmingham Intl         33.6  -86.8   644    -6 A     America/Ch…
## # ... with 90 more rows
{% endhighlight %}

The unit of observation is an **airport**.

## Tidy data

Ideally, a data table should only contain information about its unit of
observation. So, for example, the flights dataset above should not (and does
not) contain metadata about the plane, destination airport, or arrival airport.
It **does** indicate which plane was flying, where it started, and where it
landed. These are okay because these values are about the flight.

A structured dataset that adheres to this principal is called "tidy data".
Benefits of this approach include:

- no repeated information
- internal consistency
- easy to modify

Take the flights dataset and the location of each airport. If these values
were stored in the flight dataset the locations would be repeated each time
there is a flight. This is cumbersome to enter and might lead to someone
accidentally inputting the wrong value on some rows. Also, if we later
determined that a particular airport's exact coordinates have changed it would
be difficult to change the values in the table. Instead, storing  the location
in its own table required only storing the value once and making it easy to
modify and impossible to be inconsistent.

## Course schedule dataset

Now, in groups, you will open a spreadsheet and collect data about the classes
you are taking this semester. Include:

- name of the student
- student's primary major (or intended primary major if undeclared)
- does the class fulfil major requirements for the student
- course number
- course name
- course meeting building abbreviation

The twist is that you need to devise a way to collect this data in a tidy
format.

## Combining tidy data

When you create a tidy dataset, typically the data is stored in multiple
tables. In order to do analysis with the dataset we need a way of combining the
tables back together.

The diagram below shows all of the connections between tables in the NYC Flight
datasets (as well as two other tables I did not initially show):

![](../assets/img/relational-nycflights.png)

Typically a relation maps a single row in one dataset to many rows in another.
For example, each flight has one origin, but each origin has many flights.
To combine tables that share a relation, we can use the function `left_join`
in R.


{% highlight r %}
flights_new <- left_join(flights, planes, by = "tailnum")
flights_new
{% endhighlight %}



{% highlight text %}
## # A tibble: 272,870 x 15
##     year month   day dep_time arr_time tailnum origin dest  type 
##    <int> <int> <int>    <int>    <int> <chr>   <chr>  <chr> <chr>
##  1  2013     1     1      517      830 N14228  EWR    IAH   Fixe…
##  2  2013     1     1      533      850 N24211  LGA    IAH   Fixe…
##  3  2013     1     1      542      923 N619AA  JFK    MIA   Fixe…
##  4  2013     1     1      554      812 N668DN  LGA    ATL   Fixe…
##  5  2013     1     1      554      740 N39463  EWR    ORD   Fixe…
##  6  2013     1     1      555      913 N516JB  EWR    FLL   Fixe…
##  7  2013     1     1      557      709 N829AS  LGA    IAD   Fixe…
##  8  2013     1     1      557      838 N593JB  JFK    MCO   Fixe…
##  9  2013     1     1      558      849 N793JB  JFK    PBI   Fixe…
## 10  2013     1     1      558      853 N657JB  JFK    TPA   Fixe…
## # ... with 272,860 more rows, and 6 more variables: manufacturer <chr>,
## #   model <chr>, engines <int>, seats <int>, speed <int>, engine <chr>
{% endhighlight %}

Typically we put the larger table first and the smaller (metadata) table
second.

## Project I

You now have all the tools you need to take on Project I. See the main course
page for instructions. We will work on the project in class during our next
meeting.

