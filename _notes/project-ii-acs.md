---
title: "Project II: Visualizing Communities"
author: "Taylor Arnold"
---

**Due**: 2018-10-23 (start of class)

**Draft**: 2018-10-18 (three finished graphs; start of class)

**Starter code**: <a href="https://raw.githubusercontent.com/statsmaths/stat209-f18/master/projects/project-ii.Rmd" download="project-ii.Rmd" target="_blank">project-ii.Rmd</a>

**Data dictionary**: [acs-data-dictionary](https://statsmaths.github.io/stat209-f18/notes/acs-dictionary)

**Rubric**: [project-ii-rubric.csv](https://github.com/statsmaths/stat209-f18/blob/master/projects/project-ii-rubric.csv)

The overarching goal of this project is to tell an interesting narrative about
the demographics of a particular metropolitan area in the United States. The
structure of the report is much more open ended compared to the first project.

For this project we will all be working off of the same master dataset. The
data gives demographic information about [census tracts](https://en.wikipedia.org/wiki/Census_tract). You will each, however, be looking at different metropolitan
areas in the United States.

Your task is to write a short essay in the style of a 538 news article.
The essay should describe one or more interesting elements you discovered
while investigating the metropolitan area that you have been assigned. Keep
in mind that you will want to draw on one or more of the following tasks
from exploratory data analysis:

- anomaly detection: identify areas that seem to behave differently than the
rest of the data
- perspective: pick a particular area of interest and compare it to the rest
of the data
- pattern recognition: understand the basic patterns present in your dataset

The final report should contain **exactly three visualizations**. This means
that you should take care to make each visualization as information dense as
possible. Aim to have a final report around 250-500 words. The word length is
not a hard limit; it is just a guidelines to indicate the expected length
of the report. All of the plots should be integrated into the essay in a
meaningful way rather than all included at the start or end of the essay.

The grade for the assignment depends primarily on the effectiveness of the
graphics in conveying information, the quality of the writing, and execution
of how the writing and visualizations are integrated together.

## Code Examples

You may find it very helpful to make maps of the data from your tract. These
are great for exploratory work, but don't overuse them in the report. To make
a nice map, use the **ggmap** package

```{r}
library(ggmap)
acs_rva <- filter(acs, cbsa == "Richmond, VA")

qmplot(lon, lat, data = acs_rva, geom = "blank")
```

The `qmplot` function replaces the typical `ggplot()` function in the first
line of a graphic. You can add other layers just as before. The code here adds
points to the plot (notice that I set the alpha parameter to make sure the
points do not cover up the rest of the plot).

```{r}
qmplot(lon, lat, data = acs_rva, geom = "blank") +
  geom_point(aes(color = median_rent), alpha = 0.8) +
  scale_color_viridis()
```

You may also find it useful to construct new variables that aggregate the
granular ones I have provided. For example, if you want to find what percentage
of people have a commute of over 45 minutes you can do this:

```{r}
acs$ctime_45_plus <- acs$ctime_45_59 + acs$ctime_60_89 + acs$ctime_90_99
```

The name of the new variable (here, `ctime_45_plus`) is entirely up to you.

Also, some students have wanted to create a variable that shows, for each tract, the maximum
category from a group of variables. You can do this by the following code (replace `acs`
with the name of your dataset) for the race variables:

```{r}
temp <- select(acs, starts_with("race_"))
acs$max_race_category <- names(temp)[apply(temp, 1, which.max)]
```

It should be clear how to modify this for other variables (but if not, please ask!).

## ggmap

Many of you, myself included, ran into errors with the **ggmap** package. If you
want to make maps, I suggest running this command in RStudio (just once):

```{r}
devtools::install_github("dkahle/ggmap")
```

Then, remove this line and restart R. You should be able to make plots as given in
the course notes.

## Hints

Here are some hints if you are still stuck on telling a story:

1. Try looking at the household income variables. These are in dollars, not
percentages, and often have stronger more obvious relationships to the other
variables.
2. Try to create a max category for one of the clusters of variables. Try plotting
using categories as colors on a map. If almost all of the points have the same
category, try to instead use the percentage of this max category as a measurement.
For example, if almost all tracts have `race_white` as the largest category, then
try to look at the `race_white` variable. If the there is a nice mix of categories,
try to use the variable directly.
3. If you have a variable of interest, try creating a confidence interval (`geom_confint`)
plot by counties. You could also try using the `max_` categories if they are interesting.
4. Generally, you don't want to use multiple variables from the same section (not including
the income variables). Either collapse categories, use the `max_` categories trick, or
pick just one that interests you.
5. You do not need to have three different kinds of plots, but thinking about three
types can make the project easier. A map, a scatter plot, and a confidence interval plot
can often be used to together to great effect.






