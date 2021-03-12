432 Lab 03 for Spring 2021
================

Version: 2021-03-12 12:01:03

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

## Question 2 (40 points, 4 points per part)

In Question 2, we will walk through the process of fitting and
evaluating two linear regression fits (one fit with `lm` and the other
with `stan`) to predict **the square root** of a subject’s estimated
(neighborhood) median income on the basis of the main effects of the
following four predictors:

-   the subject’s neighborhood high school graduation rate,
-   the subject’s race category
-   the subject’s Hispanic/Latinx ethnicity category, and
-   the subject’s current tobacco status.

## Preliminary work

Start your work by completing the following tasks to create a tibble
that we’ll call `hbpq2` in the answer sketch:

1.  Exclude the 25 subjects in `hbp3456` who have missing values of
    either `hsgrad` or `income`.
2.  Restrict your data to the variables we’ll use in our models for
    Question 2 (the four predictors listed above, and the estimated
    neighborhood income) and the subject identifying code (the
    `record`).
3.  Ensure that all character variables (other than `record`) in your
    tibble are recognized as factors.
4.  Create a new variable called `sqrtinc` which will serve as your
    response (outcome) for your regression modeling, within your tibble.
5.  Use `set.seed(432)` and `slice_sample()` to select a random sample
    of 1000 subjects from the tibble.

Your resulting `hbpq2` tibble should look like this:

``` r
hbpq2
```

    # A tibble: 1,000 x 7
       record income hsgrad race     eth_hisp tobacco sqrtinc
       <chr>   <int>  <dbl> <fct>    <fct>    <fct>     <dbl>
     1 903574  34800   94.9 White    No       Current    187.
     2 926837  24700   74.2 AA_Black No       Current    157.
     3 929198  14700   40   AA_Black No       Never      121.
     4 932367  24700   74.2 AA_Black No       Never      157.
     5 925592  65600   92.2 <NA>     <NA>     Never      256.
     6 932404  18500   67.8 AA_Black No       Never      136.
     7 933953  21500   84.4 White    No       Never      147.
     8 911527  23000   83.6 White    No       Never      152.
     9 918228  13400   70.3 AA_Black No       Current    116.
    10 930262  48300   90   <NA>     <NA>     Never      220.
    # ... with 990 more rows

The ten questions (Questions 2a - 2j) below will walk you through the
process of comparing models fit with the `lm` and `stan` engines for
these data. The steps are meant to be completed in the specified order.

## Question 2a.

How many missing values do you have in each of the important variables
in your `hbpq2` tibble? The important variables are your outcome (square
root of estimated neighborhood income) and the four predictors.

## Question 2b.

Use an appropriate method from `tidymodels` to split the data into
training and testing samples, with 70% of the data in the training
sample. Use `set.seed(2021)` to create your split. We’ll call the
training sample `q2_train` in the sketch, and the testing sample
`q2_test`.

## Question 2c.

Build a recipe for your model, which we’ll call `q2_rec` in the sketch.
This recipe should work for either of the models you fit and include all
necessary pre-processing, which includes the following four elements:

-   specifying the roles of the outcome and the predictors,
-   using imputation via bagged tree models for all predictors with
    missing data,
-   creating dummy (indicator) variables for all categorical predictors,
-   normalizing all quantitative predictors

## Question 2d.

Specify modeling engines, separately, for the `lm` and `stan` fits you
will create. For the Bayesian fit using `stan`, use as a prior Student’s
t distribution with one degree of freedom for all parameters.

## Question 2e.

Create a workflow for the `lm` fit, and then use it to fit the `lm`
model to the training data. Summarize the fit with `tidy`.

## Question 2f.

Create a workflow for the `stan` fit, and then use it to fit the `stan`
model to the training data. Summarize the fit with `tidy`. If you get an
error message, use `broom.mixed::tidy()` to do the tidying.

## Question 2g.

Graph the tidied point estimates and 95% confidence intervals for the
coefficients (excluding the intercept) in the two models in an
attractive `ggplot` for comparison.

## Question 2h.

Interpret the results from the tidied summaries you generated in
Question 2f, and the graph you generated in Question 2g. Which
coefficients change substantially between the two fits (and in what
direction do they change), and which do not?

## Question 2i.

Assess performance in the training sample using the two fits and three
performance measures, specifically the root mean squared error, the
*R*<sup>2</sup>, and the mean absolute error. Which model appears to
perform better within the training sample? Is there a substantial
difference between the models in terms of performance on these metrics?

## Question 2j.

Finally, make predictions into the test sample using the two fits and
the same three performance measures discussed in Question 2i. Now, which
model appears to perform better within the test sample? Is there a
substantial difference between the models in terms of performance on
these metrics?

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
