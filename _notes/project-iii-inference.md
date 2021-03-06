---
title: "Project III: Data Collection Inference"
author: "Taylor Arnold"
---

**Due**: 2018-11-20 (Tuesday, noon)

**Starter code**: <a href="https://raw.githubusercontent.com/statsmaths/stat209-f18/master/projects/project-iii.Rmd" download="project-iii.Rmd" target="_blank">project-iii.Rmd</a>

**Rubric**: [project-iii-rubric.csv](https://github.com/statsmaths/stat209-f18/blob/master/projects/project-iii-rubric.csv)

The goal of this project is collect a dataset with the intent of conducting
two questions of statistical inference.

You need to collect a dataset with at least 100 observations, which includes
three variables:

- a categorical variable (I recommend no more than 4 categories)
- two numeric variables

This will lead to three possible statistical inference tests:

- inference for the mean for each of the two numeric variables by groups in
the categorical variable
- a linear regression between the two numeric variables

Ahead of collecting the data you should have one or more research questions in
mind that the dataset will help you answer. Develop hypotheses for what the
three inference tests will tell you and, if you have more than two categorical
groups, pick which level you want to use as the reference level.

Your report will then consist of the following four sections:

1. **Introduction**: A few sentences describing what your subject of interest
is and laying out your three hypotheses for the data.
2. **Methods**: Describe, in your own words, the methods you used to collect
your dataset (and if unclear, what units were used for each variable) and the
models we are using for inference for the mean and linear regression. If you
have to set a baseline for a group, make sure you explain this. The methods
section should be very short, but is good practice for writing similar reports
in the future.
3. **Results**: Show the output of the inference and regression analysis.
Along with each model, include a graphic that describes the patterns shown in
the model. Make sure you use full sentences to describe each output, but there
is no need to write too much in this section.
4. **Conclusions**: Here, you should summarize the results of the three
hypothesis tests. Indicate whether there was (i) no significant effect, (ii)
an effect in the direction you expected, or (iii) a significant but opposite
effect than the one you expected. Write several sentences putting these
results into context (what do the confidence intervals and point estimators
mean in practice?).

As with Project II, make sure that none of the code, warnings, or data are
shown in the final report. It should contain only text, graphics, and output
from the function `reg_table`.
