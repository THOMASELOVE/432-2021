432 Lab 03 for Spring 2021
================

Version: 2021-03-05 10:36:03

# General Instructions

Submit your work via [Canvas](https://canvas.case.edu/). The deadline is
specified on [the Course
Calendar](https://github.com/THOMASELOVE/432/calendar.html).

Your response should include an R Markdown file and an HTML document
that is the result of applying your R Markdown file to the `hbp3456.csv`
data, available on [our Data and Code
page](https://github.com/THOMASELOVE/432-data), as well as [in the data
subfolder](https://github.com/THOMASELOVE/432-2021/tree/master/labs/lab03/data)
for this Lab.

Start a separate R Project for Lab 03, as your first step, and place the
data in that project’s directory or (if you want to match what I did) in
a data sub-directory under that project’s directory.

There is no template for Lab 03. Please feel free to make use of one of
the templates we’ve provided for a previous Lab, or use an approach that
works well for you.

# The Data

This lab again uses the `hbp3456` data, from Lab 01. See [the Lab 01
materials](https://github.com/THOMASELOVE/432-2021/tree/master/labs/lab01)
for details on that data set.

## Question 1. (40 points, 10 points for each part)

Begin with the `hbp3456` data restricted to the following four
practices: Center, Elm, Plympton and Walnut, and to subjects with
complete data on the `ldl` variable. Now, build a logistic regression
model to predict whether the subject has a statin prescription based on:

-   the subject’s current LDL cholesterol level
-   which of the four practices they receive care from, along with
-   the subject’s age.

1.  Fit two models: one with and one without an interaction term between
    the practice and the LDL level. Include the age variable in each
    model using a restricted cubic spline with 4 knots, but without any
    interaction with the other predictors.

2.  For the “no interaction” model from Question 1, interpret the odds
    ratio associated with the `ldl` main effect carefully, specifying a
    90% uncertainty interval and what we can conclude from the results.

3.  Now using the “interaction” model from Question 1, please interpret
    the effect of `ldl` on the odds of a statin prescription
    appropriately, specifying again what we can conclude from the
    results. A detailed description of the point estimate(s) will be
    sufficient here.

4.  Now, compare the effectiveness of your two fitted models (the
    “interaction” and “no interaction” models) and draw a reasoned
    conclusion about which of those two models is more effective in
    describing the available set of observations (after those without
    `statin` data are removed) from these four practices. An appropriate
    response will make use of at least two different validated
    assessments of fit quality. Be sure to justify your eventual
    selection (between the “interaction” or “no interaction” model) with
    complete sentences.

### Four Hints for Question 1

-   **Hint 1**: In parts b and c, we assume you will describe the `ldl`
    main effect by considering the case of Harry and Sally. Harry has an
    `ldl` value of 142, equal to the 75th percentile `ldl` value in the
    data. Sally has an `ldl` value of 85, equal to the 25th percentile
    `ldl` value in the data. Assume Harry and Sally are the same `age`
    and receive care at the same `practice`. So the odds ratio of
    interest here compares the odds of `statin` prescription for Harry
    to the odds of `statin` prescription for Sally.
-   **Hint 2**: To obtain a 90% confidence (uncertainty) interval with a
    fit using one of the `rms` fitting functions rather than the default
    95% interval, the appropriate code would be
    `summary(modelname, conf.int = 0.9)`.
-   **Hint 3**: In part c, we want you to describe the `ldl` main effect
    by considering the case of Harry and Sally. Harry has an `ldl` value
    of 142, equal to the 75th percentile `ldl` value in the data. Sally
    has an `ldl` value of 85, equal to the 25th percentile `ldl` value
    in the data. Assume Harry and Sally are the same `age` and receive
    care at the same `practice`. So the odds ratio of interest here
    compares the odds of `statin` prescription for Harry to the odds of
    `statin` prescription for Sally. But now, you need to be able to do
    this separately for each individual level of `practice`, since
    `practice` interacts with `ldl`. There are at least two ways to
    accomplish this.
    -   In one approach, you would create predicted odds values for
        Harry and Sally, assuming a common age (40 would be a reasonable
        choice, and it’s the one used in the answer sketch) with `ldl`
        set to 142 for Harry and 85 for Sally, but creating four
        different versions of Harry and Sally (one for each practice.)
        Then use those predicted odds within each practice to obtain
        practice-specific odds ratios.
    -   In the other approach, you could convince the `rms` package to
        use a different practice as the choice for which adjustments are
        made. By default, `datadist` chooses the modal practice. To
        change this, you’d need to convince `datadist` instead to choose
        its practice based on which practice is the first one, and
        relevel the practice factor accordingly. So, if you’d releveled
        the practice data so that Elm was first and placed that into a
        tibble called `dataelm`, you could use the following adjustment
        to the `datadist` call to ensure that the adjustments made by
        `datadist` used Elm instead of the modal practice.

<!-- -->

    d_elm <- datadist(dataelm, adjto.cat = "first")
    options(datadist = "d_elm")

-   **Hint 4** The natural choices for validated assessments of fit
    quality are a bootstrap-validated C statistic and a
    bootstrap-validated Nagelkerke *R*<sup>2</sup>. In the answer
    sketch, we will use `2021` as our random seed for this work, and
    we’ll do the default amount of bootstrap replications.

## Question 2 (40 points, 10 points for each part)

For Question 2, start your work by identifying all observations in
`hbp3456` excluding the 25 subjects who have missing values of both
`hsgrad` and `income`. From that set of subjects, use `set.seed(432)` to
select a random sample of 1000 for use in your Question 2 work, which
will take advantage of tools we’ve seen in the `tidymodels` group of
packages.

Now, you will use linear regression methods (with two different engines:
`lm` and `stan`) to predict **the square root** of a subject’s estimated
(neighborhood) median income on the basis of the main effects of the
following four predictors:

-   the subject’s neighborhood high school graduation rate,
-   the subject’s race category
-   the subject’s Hispanic/Latinx ethnicity category, and
-   the subject’s current tobacco status.

You will build your model separately using each engine (first `lm` and
then `stan` with a reasonable prior), being sure to treat the
categorical predictors appropriately.

1.  Use a reasonable imputation strategy to deal with missing values on
    predictors in your sample, and display code that accomplishes this
    for each of the engines you use to fit the model (`lm` and `stan`).

2.  Select an appropriate prior for the `stan` engine, and display code
    that accomplishes this in the context of fitting the model with that
    engine.

3.  Now, fit the model to the **square root** of the subject’s `income`
    with each engine, and present the resulting regression coefficients
    using tidy tabular and graphical approaches for each engine. Compare
    the results in a sentence or two. What difference does the choice of
    engine make here?

4.  For each engine, assess the quality of fit through a summary measure
    developed using 10-fold cross validation on the sample of 1000
    subjects. Compare the results in a sentence or two. What difference
    does the choice of engine make here in terms of fit quality (based
    on an appropriate cross-validated measure)?

## Question 3 (20 points)

At this point, you should have completed reading Chapter 6 of Nate
Silver’s *The Signal and the Noise*. In that chapter, Nate discusses the
problem of using point estimates without building things like confidence
intervals in the context of making predictions about economies.

In a short essay not to exceed 200 words, tell us your thoughts about
the relative importance of point estimates vs. confidence intervals in
your life.

We’d like you to tell us about a past experience where you personally
were confronted by uncertainty and needed to make a decision about an
outcome you could only imprecisely predict. We want especially to
understand whether you considered a range of potential outcomes, or just
what you felt was the most likely one, and how do you reflect on this
now, in light of what you’ve read. Illustrate your thoughts with a
specific example from some time in the past in your work/school/home
life. Select an example that you feel comfortable sharing in enough
detail to let us understand the decision you were faced with, how you
thought about it, and whether you’d think about it differently now that
you know the outcome.

### Please add the session information.

Finally, at the end of this homework and all subsequent assignments
(including the projects), please add the session information. You can
either use the usual `sessioninfo::session_info()` approach, or else
use…

``` r
xfun::session_info()
```

### This is the end of Lab 03.
