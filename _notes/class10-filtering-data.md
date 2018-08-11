---
title: "Class 10: Filtering Data"
author: "Taylor Arnold"
output: html
---





## Learning Objectives

- 


Today we will discuss how to construct new datasets as a subset of a current
dataset.

## NYC Flights Data

Often is will be useful to take a subset of a dataset. This can be useful
if we are only interested in a particular part of the dataset; it can also
be used if we want to create one visualization layers that highlights where
one subset of data lies within another.

To illustrate how this works, today we will explore a dataset of the every
commercial flight that departed from New York City in 2013 (we'll use this
many times as it is a great teaching sample):


{% highlight r %}
flights <- read_csv("https://statsmaths.github.io/stat_data/flights.csv")
flights
{% endhighlight %}



{% highlight text %}
## # A tibble: 327,346 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <int>    <int>
##  1  2013     1     1      517            515         2      830
##  2  2013     1     1      533            529         4      850
##  3  2013     1     1      542            540         2      923
##  4  2013     1     1      544            545        -1     1004
##  5  2013     1     1      554            600        -6      812
##  6  2013     1     1      554            558        -4      740
##  7  2013     1     1      555            600        -5      913
##  8  2013     1     1      557            600        -3      709
##  9  2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 327,336 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <int>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <int>, distance <int>, hour <int>,
## #   minute <int>, time_hour <dttm>
{% endhighlight %}

We will discuss a few different ways of taking a subset of the data, termed
**filtering**, before showing how this approach can integrated into other
analyses.

## Filtering data

The general syntax for filtering data in R is to use the following, where
expression is a logical statement about variables in the dataset `OLD`:


{% highlight r %}
NEW <- filter(OLD, EXPRESSION)
{% endhighlight %}

For example, to find flights where the departure time is greater than
2300 (the times are in a 24 hour format):


{% highlight r %}
flights_new <- filter(flights, dep_time > 2300)
flights_new
{% endhighlight %}



{% highlight text %}
## # A tibble: 2,573 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <int>    <int>
##  1  2013     1     1     2302           2200        62     2342
##  2  2013     1     1     2306           2245        21       28
##  3  2013     1     1     2307           2245        22       32
##  4  2013     1     1     2310           2255        15       24
##  5  2013     1     1     2312           2000       192       21
##  6  2013     1     1     2323           2200        83       22
##  7  2013     1     1     2326           2130       116      131
##  8  2013     1     1     2327           2250        37       32
##  9  2013     1     1     2343           1724       379      314
## 10  2013     1     1     2353           2359        -6      425
## # ... with 2,563 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <int>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <int>, distance <int>, hour <int>,
## #   minute <int>, time_hour <dttm>
{% endhighlight %}

Notice that the new dataset has only 2573 rows, much smaller than the starting
dataset. Similar expressions exist for other numeric comparisons: `<` (less
than), `>=` (greater than or equal), and `<=` (less than or equal). Similarly
we can compare whether one variable is exactly equal to a particular value.
For this we need to use `==`, not a single equal sign:


{% highlight r %}
flights_exact <- filter(flights, dep_time == 2300)
flights_exact
{% endhighlight %}



{% highlight text %}
## # A tibble: 64 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <int>    <int>
##  1  2013     1     5     2300           2229        31      352
##  2  2013     1     6     2300           2245        15       14
##  3  2013     1    13     2300           2039       141      357
##  4  2013     1    16     2300           2253         7       19
##  5  2013     1    30     2300           2030       150       23
##  6  2013    10     9     2300           2159        61       15
##  7  2013    10    18     2300           2245        15        7
##  8  2013    10    22     2300           2255         5       17
##  9  2013    11     8     2300           2245        15       16
## 10  2013    11    25     2300           2145        75      155
## # ... with 54 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <int>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <int>, distance <int>, hour <int>,
## #   minute <int>, time_hour <dttm>
{% endhighlight %}

Here we have flights that only take off *exactly* at 11pm. The symbol `!=`
detects whether a value is **not** equal to a particular value:


{% highlight r %}
flights_not_december <- filter(flights, month != 12)
flights_not_december
{% endhighlight %}



{% highlight text %}
## # A tibble: 300,326 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <int>    <int>
##  1  2013     1     1      517            515         2      830
##  2  2013     1     1      533            529         4      850
##  3  2013     1     1      542            540         2      923
##  4  2013     1     1      544            545        -1     1004
##  5  2013     1     1      554            600        -6      812
##  6  2013     1     1      554            558        -4      740
##  7  2013     1     1      555            600        -5      913
##  8  2013     1     1      557            600        -3      709
##  9  2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 300,316 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <int>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <int>, distance <int>, hour <int>,
## #   minute <int>, time_hour <dttm>
{% endhighlight %}

The `==` and `!=` symbols also work for character and date variables, however
you'll need to make sure to enclose the comparison value (not the variable) in
quotation marks:


{% highlight r %}
flights_to_rva <- filter(flights, dest == "RIC")
flights_to_rva
{% endhighlight %}



{% highlight text %}
## # A tibble: 2,346 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <int>    <int>
##  1  2013     1     1     1505           1310       115     1638
##  2  2013     1     1     1859           1900        -1     2012
##  3  2013     1     1     1938           1703       155     2109
##  4  2013     1     1     2115           2110         5     2240
##  5  2013     1     2      859            845        14     1035
##  6  2013     1     2     1352           1310        42     1509
##  7  2013     1     2     1525           1525         0     1723
##  8  2013     1     2     1810           1703        67     1948
##  9  2013     1     2     1940           1900        40     2052
## 10  2013     1     2     2119           2110         9     2227
## # ... with 2,336 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <int>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <int>, distance <int>, hour <int>,
## #   minute <int>, time_hour <dttm>
{% endhighlight %}

We can detect whether a variable is equal to a set of values using the `%in%`
and `c` functions:


{% highlight r %}
flights_summer <- filter(flights, month %in% c(7, 8, 9))
flights_dc_metro <- filter(flights, dest %in% c("DCA", "IAD", "BWI"))
{% endhighlight %}

These approaches here should get you through most of your needs in filtering
datasets. Anything else can be gotten by making use of the `&` (and),
`|` (or), and `!` (negation). Do not worry about these now; if you have a
need to use them on a project or lab I will show you then.

## Graphing filtered data

If you want to just use the filtered data, this can be done straightforwardly
in R by simply specifying the correct dataset in the first parameter of the
`ggplot` command. But what if you want to use a subset of the data in only
one plot?

Within `geom` layer we can override the `data = ` option to use a different
dataset than specified in the first line. I recommend putting this as the end
of the geom layer:


{% highlight r %}
ggplot(flights, aes(air_time, distance)) +
  geom_point() +
  geom_point(color = "red", data = flights_to_rva)
{% endhighlight %}

<img src="../assets/class10-filtering-data/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" width="100%" />

This shows all of the Richmond flights in red on top of the remainder of the
flights. Combined with annotations, these techniques can create very
professional looking graphics.

## Filtering dates

To filter dates and date time objects we can also use the numeric comparison
operators such as `>` and `<`. However, we have to convert the thing we are
comparing to a date object using either `as.Date` (for just date data) or
`as.POSIXct` (for date time data).

For example, here is a way of filtering the flights dataset using the
`time_hour` variable:


{% highlight r %}
flights_post_oct <- filter(flights, time_hour >= as.POSIXct("2013-11-01"))
{% endhighlight %}



{% highlight text %}
## Warning in strptime(xx, f <- "%Y-%m-%d %H:%M:%OS", tz = tz): unknown
## timezone 'zone/tz/2018c.1.0/zoneinfo/UTC'
{% endhighlight %}



{% highlight r %}
flights_post_oct
{% endhighlight %}



{% highlight text %}
## # A tibble: 53,991 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <int>    <int>
##  1  2013    11     1        5           2359         6      352
##  2  2013    11     1       35           2250       105      123
##  3  2013    11     1      455            500        -5      641
##  4  2013    11     1      539            545        -6      856
##  5  2013    11     1      542            545        -3      831
##  6  2013    11     1      549            600       -11      912
##  7  2013    11     1      550            600       -10      705
##  8  2013    11     1      554            600        -6      659
##  9  2013    11     1      554            600        -6      826
## 10  2013    11     1      554            600        -6      749
## # ... with 53,981 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <int>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <int>, distance <int>, hour <int>,
## #   minute <int>, time_hour <dttm>
{% endhighlight %}

The special function `between` allows us to grab a range of dates:


{% highlight r %}
flights_xmas <- filter(flights, between(time_hour,
                                        as.POSIXct("2013-12-25"),
                                        as.POSIXct("2013-12-26")))
flights_xmas
{% endhighlight %}



{% highlight text %}
## # A tibble: 715 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <int>    <int>
##  1  2013    12    25      456            500        -4      649
##  2  2013    12    25      524            515         9      805
##  3  2013    12    25      542            540         2      832
##  4  2013    12    25      546            550        -4     1022
##  5  2013    12    25      556            600        -4      730
##  6  2013    12    25      557            600        -3      743
##  7  2013    12    25      557            600        -3      818
##  8  2013    12    25      559            600        -1      855
##  9  2013    12    25      559            600        -1      849
## 10  2013    12    25      600            600         0      850
## # ... with 705 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <int>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <int>, distance <int>, hour <int>,
## #   minute <int>, time_hour <dttm>
{% endhighlight %}

Or, similarly, as a date time object:



{% highlight r %}
flights_xmas_night <- filter(flights, between(time_hour,
                                              as.POSIXct("2013-12-25 18:00"),
                                              as.POSIXct("2013-12-25 23:59")))
flights_xmas_night
{% endhighlight %}



{% highlight text %}
## # A tibble: 162 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <int>    <int>
##  1  2013    12    25     1748           1800       -12     1919
##  2  2013    12    25     1753           1800        -7     1935
##  3  2013    12    25     1757           1805        -8     1946
##  4  2013    12    25     1800           1805        -5     1914
##  5  2013    12    25     1803           1805        -2     2024
##  6  2013    12    25     1804           1802         2     2052
##  7  2013    12    25     1805           1810        -5     1917
##  8  2013    12    25     1805           1805         0     1949
##  9  2013    12    25     1807           1810        -3     2058
## 10  2013    12    25     1809           1815        -6     2018
## # ... with 152 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <int>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <int>, distance <int>, hour <int>,
## #   minute <int>, time_hour <dttm>
{% endhighlight %}

This will be useful if you want to subset the data you had from the first
project.


