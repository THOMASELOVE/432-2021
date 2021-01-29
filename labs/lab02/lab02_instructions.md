432 Lab 02 for Spring 2021
================

Version: 2021-01-29 10:38:42

# General Instructions

Submit your work via [Canvas](https://canvas.case.edu/). The deadline is
specified on [the Course
Calendar](https://github.com/THOMASELOVE/432/calendar.html).

Your response should include an R Markdown file and an HTML document
that is the result of applying your R Markdown file to the
`oh_counties_2020.csv` data, available on [our Data and Code
page](https://github.com/THOMASELOVE/432-data), as well as [in the data
subfolder](https://github.com/THOMASELOVE/432-2021/tree/master/labs/lab02/data)
for this Lab.

Start a separate R Project for Lab 02, as your first step, and place the
data in that project’s directory or (if you want to match what I did) in
a data sub-directory under that project’s directory.

-   There is an R Markdown **template** for Lab 02, which [you can find
    here](https://github.com/THOMASELOVE/432-2021/blob/master/labs/lab02/lab02_template.Rmd).
    Please feel encouraged to use the template as it is, or modify it to
    produce something that you like better. This template uses the
    `downcute` approach from the `rmdformats` package.
-   If you want to see how this template looks as a knitted HTML file,
    [visit this link](https://rpubs.com/TELOVE/lab02-template-432-2021).

## The `oh_counties_2020.csv` data

The `oh_counties_2020.csv` data set I have provided describes a series
of variables, pulled from the data for the 88 counties of the the State
of Ohio from the [County Health
Rankings](http://www.countyhealthrankings.org/rankings/data/oh) report
for 2020.

-   Several detailed County Health Rankings files augment these 2020
    Ohio Rankings Data. Find [those items
    here](https://www.countyhealthrankings.org/app/ohio/2020/downloads)
    if you’re interested.

The available variables are listed below. Each variable describes data
at the **COUNTY** level.

|        Variable        | Description                                                                                                                        |
|:----------------------:|------------------------------------------------------------------------------------------------------------------------------------|
|         `fips`         | Federal Information Processing Standard code                                                                                       |
|        `county`        | name of County                                                                                                                     |
|   `years_lost_rate`    | age-adjusted years of potential life lost rate (per 100,000 population)                                                            |
|    `sroh_fairpoor`     | % of adults reporting fair or poor health (via BRFSS)                                                                              |
|      `phys_days`       | mean number of reported physically unhealthy days per month                                                                        |
|      `ment_days`       | mean number of reported mentally unhealthy days per mo                                                                             |
|       `lbw_pct`        | % of births with low birth weight (&lt; 2500 grams)                                                                                |
|      `smoker_pct`      | % of adults that report currently smoking                                                                                          |
|      `obese_pct`       | % of adults that report body mass index of 30 or higher                                                                            |
|       `food_env`       | indicator of access to healthy foods, in points (0 is worst, 10 is best)                                                           |
|     `inactive_pct`     | % of adults that report no leisure-time physical activity                                                                          |
|     `exer_access`      | % of the population with access to places for physical activity                                                                    |
|      `exc_drink`       | % of adults that report excessive drinking                                                                                         |
|      `alc_drive`       | % of driving deaths with alcohol involvement                                                                                       |
|       `sti_rate`       | Chlamydia cases / Population x 100,000                                                                                             |
|     `teen_births`      | Teen births / females ages 15-19 x 1,000                                                                                           |
|      `uninsured`       | % of people under age 65 without insurance                                                                                         |
|      `pcp_ratio`       | Population to Primary Care Physicians ratio                                                                                        |
|      `prev_hosp`       | Discharges for Ambulatory Care Sensitive Conditions/Medicare Enrollees x 1,000                                                     |
|       `hsgrads`        | High School graduation rate                                                                                                        |
|      `unemployed`      | % of population age 16+ who are unemployed and looking for work                                                                    |
|      `poor_kids`       | % of children (under age 18) living in poverty                                                                                     |
|     `income_ratio`     | Ratio of household income at the 80th percentile to income at the 20th percentile                                                  |
|     `associations`     | \# of social associations / population x 10,000                                                                                    |
|        `pm2.5`         | Average daily amount of fine particulate matter in micrograms per cubic meter                                                      |
|       `h2oviol`        | Presence of a water violation: Yes or No                                                                                           |
|     `sev_housing`      | % of households with at least 1 of 4 housing problems: overcrowding, high housing costs, or lack of kitchen or plumbing facilities |
|     `drive_alone`      | % of workers who drive alone to work                                                                                               |
|  `age.adj.mortality`   | premature age-adjusted mortality                                                                                                   |
|       `dm_prev`        | % with a diabetes diagnosis                                                                                                        |
|  `freq_phys_distress`  | % in frequent physical distress                                                                                                    |
| `freq_mental_distress` | % in frequent mental distress                                                                                                      |
|    `food_insecure`     | % who are food insecure                                                                                                            |
|     `insuff_sleep`     | % who get insufficient sleep                                                                                                       |
|    `median_income`     | estimated median income                                                                                                            |
|      `population`      | population size                                                                                                                    |
|      `age65plus`       | % of population who are 65 and over                                                                                                |
|      `african-am`      | % of population who are African-American                                                                                           |
|       `hispanic`       | % of population who are of Hispanic/Latino ethnicity                                                                               |
|        `white`         | % of population who are White                                                                                                      |
|        `female`        | % of population who are Female                                                                                                     |
|        `rural`         | % of people in the county who live in rural areas                                                                                  |

## Loading the Data

Here’s a listing of the resulting tibble.

``` r
library(here)
library(tidyverse)

oh20 <- read_csv(here("data", "oh_counties_2020.csv")) 

oh20 <- oh20 %>%
    mutate(fips = as.character(fips))

oh20
```

    # A tibble: 88 x 43
       fips  state county years_lost_rate sroh_fairpoor phys_days ment_days lbw_pct
       <chr> <chr> <chr>            <dbl>         <dbl>     <dbl>     <dbl>   <dbl>
     1 39001 Ohio  Adams            12223          22.3      4.94      4.83    9.05
     2 39003 Ohio  Allen             9037          16.5      4.12      4.4     9.35
     3 39005 Ohio  Ashla~            7483          16.8      4.09      4.47    5.96
     4 39007 Ohio  Ashta~           10013          16.8      4.18      4.57    8.16
     5 39009 Ohio  Athens            7014          20.6      4.66      4.92    8.55
     6 39011 Ohio  Augla~            6454          14.6      3.73      4.27    7.32
     7 39013 Ohio  Belmo~            8289          18.0      3.87      4.46    8.34
     8 39015 Ohio  Brown            10748          17.3      4.19      4.58    7.39
     9 39017 Ohio  Butler            9429          16.5      4         4.14    7.99
    10 39019 Ohio  Carro~            8073          16.2      4.08      4.39    7.61
    # ... with 78 more rows, and 35 more variables: smoker_pct <dbl>,
    #   obese_pct <dbl>, food_env <dbl>, inactive_pct <dbl>, exer_access <dbl>,
    #   exc_drink <dbl>, alc_drive <dbl>, sti_rate <dbl>, teen_births <dbl>,
    #   uninsured <dbl>, pcp_ratio <dbl>, prev_hosp <dbl>, hsgrads <dbl>,
    #   unemployed <dbl>, poor_kids <dbl>, income_ratio <dbl>, associations <dbl>,
    #   pm2.5 <dbl>, h2oviol <chr>, sev_housing <dbl>, drive_alone <dbl>,
    #   `age-adj-mortality` <dbl>, dm_prev <dbl>, freq_phys_distress <dbl>,
    #   freq_mental_distress <dbl>, food_insecure <dbl>, insuff_sleep <dbl>,
    #   median_income <dbl>, population <dbl>, age65plus <dbl>, `african-am` <dbl>,
    #   hispanic <dbl>, white <dbl>, female <dbl>, rural <dbl>

## Question 1 (20 points)

Create a visualization (using R) based on some part of the
`oh_counties_2020.csv` data set, and share it (the visualization and the
R code you used to build it) with us.

The visualization should:

-   be of a professional quality,
-   describe information from at least three different variables listed
    above (you are welcome to transform or re-express the variables if
    that is of interest),
-   include proper labels and a meaningful title,
-   include a *caption* of no more than 75 words that highlights the key
    result.

In developing your caption, I find it helpful to think about what
question this visualization is meant to answer, and then provide a
caption which makes it clear what the question (and answer) is.

-   You are welcome to find useful tools for visualizing data in R that
    we have seen in either 431 or 432 or elsewhere. Don’t neglect ideas
    you’ve read about (in 431 in Spiegelhalter) or in Leek, for
    instance.
-   Although you may fit a model to help show patterns, your primary
    task is to show **the data** in a meaningful way, rather than to
    simply highlight the results of a model.

We will evaluate Question 1 based on the quality of the visualization,
its title and caption, in terms of being attractive, well-labeled and
useful for representing the County Health Rankings data for Ohio, and
how well it adheres to general principles for good visualizations we’ve
seen in 431 and 432.

## Question 2 (20 points)

Identify an important thing that you learned about prediction from your
reading of Nate Silver’s *The Signal and the Noise* either in Chapter 2
(about political predictions) or in chapters 3 (baseball), 4 (weather)
or 5 (earthquakes). What does this thing you learned suggest to you
about the way you build prediction models, and how can you adopt this
new way of thinking to improve the models you’ll generate in this Lab
and elsewhere in your scientific life?

Write a short essay (between 150 and 250 words is appropriate) using
clearly constructed and complete sentences to respond to this. The essay
should be composed using R Markdown.

-   We encourage you to provide brief citations or quotes from Silver
    and elsewhere as needed.
-   In citing *The Signal and the Noise* a quotation with (Silver,
    Chapter X) is sufficient.
-   In citing another work, a more detailed citation is appropriate.
    Citations and quotations do not count towards the suggested word
    limit.

## Question 3 (20 points)

Create a linear regression model to predict `obese_pct` as a function of
`food_env` adjusting for `median_income`, and treating all three
variables as quantitative. Specify and then carefully interpret the
estimated coefficient of `food_env` and a 90% uncertainty interval
around that estimate in context using nothing but complete English
sentences. A model using main effects only, entered as linear
predictors, will be sufficient.

## Question 4 (10 points)

Evaluate the quality of the model you fit in Question 3 in terms of
adherence to regression modeling assumptions, through the specification
and written evaluation of residual plots and other diagnostics. What
might be done to improve the fit of the model you’ve developed in
Question 3? Identify by name any outlying counties, in terms of the
relationship you’re studying, and explain why they are flagged as
outliers.

## Question 5 (20 points)

Create a logistic regression model to predict the presence of a water
violation (as contained in `h2oviol`) on the basis of `sev_housing` and
`pm2.5`. Specify and then carefully interpret the estimated coefficient
of `sev_housing` and a 90% uncertainty interval around that estimate in
context using nothing but complete English sentences. A model using main
effects only, entered as linear predictors, will be sufficient.

## Question 6 (10 points)

Produce and interpret the meaning of a confusion matrix which describes
the quality of the predictions you have developed in Question 5. How
well does the model you fit make predictions about `h2oviol`?

## Please add the session information.

Finally, at the end of this lab and all subsequent assignments
(including the projects), please add the session information. My
preferred way for you to do this for 432 is…

``` r
xfun::session_info()
```

the results of which are shown at the bottom of this page.

### End of Homework 2

``` r
xfun::session_info()
```

    R version 4.0.3 (2020-10-10)
    Platform: x86_64-w64-mingw32/x64 (64-bit)
    Running under: Windows 10 x64 (build 19041)

    Locale:
      LC_COLLATE=English_United States.1252 
      LC_CTYPE=English_United States.1252   
      LC_MONETARY=English_United States.1252
      LC_NUMERIC=C                          
      LC_TIME=English_United States.1252    

    Package version:
      askpass_1.1        assertthat_0.2.1   backports_1.2.1   
      base64enc_0.1.3    BH_1.75.0.0        blob_1.2.1        
      brio_1.1.1         broom_0.7.3        callr_3.5.1       
      cellranger_1.1.0   cli_2.2.0          clipr_0.7.1       
      colorspace_2.0-0   compiler_4.0.3     cpp11_0.2.5       
      crayon_1.3.4       curl_4.3           DBI_1.1.1         
      dbplyr_2.0.0       desc_1.2.0         diffobj_0.3.3     
      digest_0.6.27      dplyr_1.0.3        ellipsis_0.3.1    
      evaluate_0.14      fansi_0.4.2        farver_2.0.3      
      forcats_0.5.1      fs_1.5.0           generics_0.1.0    
      ggplot2_3.3.3      glue_1.4.2         graphics_4.0.3    
      grDevices_4.0.3    grid_4.0.3         gtable_0.3.0      
      haven_2.3.1        here_1.0.1         highr_0.8         
      hms_1.0.0          htmltools_0.5.0    httr_1.4.2        
      isoband_0.2.3      jsonlite_1.7.2     knitr_1.31        
      labeling_0.4.2     lattice_0.20.41    lifecycle_0.2.0   
      lubridate_1.7.9.2  magrittr_2.0.1     markdown_1.1      
      MASS_7.3.53        Matrix_1.2.18      methods_4.0.3     
      mgcv_1.8.33        mime_0.9           modelr_0.1.8      
      munsell_0.5.0      nlme_3.1.149       openssl_1.4.3     
      pillar_1.4.7       pkgbuild_1.2.0     pkgconfig_2.0.3   
      pkgload_1.1.0      praise_1.0.0       prettyunits_1.1.1 
      processx_3.4.5     progress_1.2.2     ps_1.5.0          
      purrr_0.3.4        R6_2.5.0           RColorBrewer_1.1.2
      Rcpp_1.0.6         readr_1.4.0        readxl_1.3.1      
      rematch_1.0.1      rematch2_2.1.2     reprex_1.0.0      
      rlang_0.4.9        rmarkdown_2.6      rprojroot_2.0.2   
      rstudioapi_0.13    rvest_0.3.6        scales_1.1.1      
      selectr_0.4.2      splines_4.0.3      stats_4.0.3       
      stringi_1.5.3      stringr_1.4.0      sys_3.4           
      testthat_3.0.1     tibble_3.0.5       tidyr_1.1.2       
      tidyselect_1.1.0   tidyverse_1.3.0    tinytex_0.29      
      tools_4.0.3        utf8_1.1.4         utils_4.0.3       
      vctrs_0.3.6        viridisLite_0.3.0  waldo_0.2.3       
      withr_2.4.1        xfun_0.19          xml2_1.3.2        
      yaml_2.2.1        