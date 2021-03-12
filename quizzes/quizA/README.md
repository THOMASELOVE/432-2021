Instructions for 432 Quiz A
================
Thomas E. Love
Last Update: 2021-03-11

# Links

Here are the links for the Quiz.

-   Quiz A: [The Main Document](https://github.com/THOMASELOVE/432-2021/blob/master/quizzes/quizA/432_quizA_2021.pdf) (pdf)
    -   Complete instructions and the 26 questions you will be answering. The PDF is 25 pages long.
-   The Google Form Answer Sheet is at http://bit.ly/432-quizA-2021-answer-sheet.
    -   This is where you will put all of your answers to the 26 Quiz questions. Like a Minute Paper, you need to log into Google via CWRU to see and use the form.
-   The [Three Data
    Sets](https://github.com/THOMASELOVE/432-2021/tree/master/quizzes/quizA/data)
    ([`set01.csv`](https://raw.githubusercontent.com/THOMASELOVE/432-2021/master/quizzes/quizA/data/set01.csv),
    [`set13.csv`](https://raw.githubusercontent.com/THOMASELOVE/432-2021/master/quizzes/quizA/data/set13.csv)
    and
    [`set20.csv`](https://raw.githubusercontent.com/THOMASELOVE/432-2021/master/quizzes/quizA/data/set20.csv)),
    which are available now.
- If Dr. Love makes any changes to any of these materials before the final deadline, he will announce them via [Piazza](https://thomaselove.github.io/432/contact.html).
- Reminder: if you have questions about the Quiz, please ask them via a **Private post on Piazza** to the instructors.

The Quiz is due at 9 PM Monday 2021-03-22, as indicated on [the Course Calendar](https://thomaselove.github.io/432/calendar.html).

**Good luck!**

Note that the instructions below also appear in [the Main Document](https://github.com/THOMASELOVE/432-2021/blob/master/quizzes/quizA/432_quizA_2021.pdf) for Quiz A.

-------

# Instructions

There are 26 questions on this Quiz. It is to your advantage to answer
all 26 Questions. Your score is based on the number of correct
responses, so there’s no chance a blank response will be correct, and a
guess might be, so you should definitely answer all of the questions.

## The Google Form Answer Sheet

All of your answers should be placed in the Google Form Answer Sheet,
and we will provide a link to that sheet on 2021-03-12. All of your
answers must be submitted through the Google Form by **9 PM** on Monday
2021-03-22, without exception. The form will close at that time, and no
extensions will be made available, so please do not wait until Monday
evening to submit. We will not accept any responses except through the
Google Form.

The Google Form contains places to provide your responses to each
question, and a final affirmation where you’ll type in your name to tell
us that you followed the rules for the Quiz. You must complete that
affirmation and then submit your results. When you submit your results
(in the same way you submit a Minute Paper) you will receive an email
copy of your submission, with a link that will allow you to edit your
results.The Answer Sheet works like a Minute Paper, in that you must be
logged into Google via CWRU to access it.

If you wish to work on some of the quiz and then return later, you can
do this by \[1\] completing the final question (the affirmation) which
asks you to type in your full name, and then \[2\] submitting the quiz.
You will then receive a link at your CWRU email which will allow you to
return to the Quiz Answer Sheet as often as you like without losing your
progress.

## The Data Sets

I have provided three data sets (called `set01.csv`, `set13.csv` and
`set20.csv`) that are mentioned in the Quiz. They may be helpful to you.

## Getting Help

This is an open book, open notes quiz. You are welcome to consult the
materials provided on the course website and that we’ve been reading in
the class, but you are not allowed to discuss the questions on this quiz
with anyone other than Professor Love and the teaching assistants. You
will be required to complete a short affirmation that you have obeyed
these rules as part of submitting the Quiz.

If you need clarification on a Quiz question, you have exactly two ways
of getting help:

1.  You can ask your question in a **private** post on Piazza to the
    instructors.
2.  You can ask your question via email to **431-help at case dot edu**.

During the Quiz period (2021-03-12 through 2021-03-22) we will not
answer questions about the Quiz except through the two approaches listed
above. We promise to respond to all questions received before 5 PM on
2021-03-22 in a timely fashion.

A few cautions:

-   Specific questions are more likely to get helpful answers.
-   We will not review your code or your English for you.
-   We will not tell you if your answer is correct, or if it is
    complete.
-   We will post to Piazza in the `quiza` folder if we find an error in
    the Quiz that needs fixing.

### When Should I ask for help?

We recommend the following process.

-   If you encounter a tough question, skip it, and build up your
    confidence by tackling other questions.
-   When you return to the tough question, spend no more than 10-15
    minutes on it. If you still don’t have it, take a break (not just to
    do other questions) but an actual break.
-   When you return to the question, it may be much clearer to you. If
    so, great. If not, spend 5-10 minutes on it, at most, and if you are
    still stuck, ask us for help.
-   This is not to say that you cannot ask us sooner than this, but you
    should **never, ever** spend more than 20 minutes on any question
    without asking for help.

## Scoring and Timing

All questions are worth 3, 4 or 5 points, adding to a total of 100
points. The questions are not in any particular order, and range in
difficulty from “things Dr. Love expects everyone to get right” to
“things that are deliberately tricky”. Some questions will take more
time than others to answer. We’ll warn you that several of the “longer”
questions come early in the Quiz, in particular, we expect Questions
1-5, 8-9 and 11 to take “longer” to complete than the median question.

The Quiz is meant to take 4-5 hours to complete. I expect most students
will take 3-6 hours, and some will take as little as 2 or as many as 8.
Again, it is **not** a good idea to spend a long time on any one
question.

Dr. Love will grade the Quiz, and results (including an answer sketch)
will be available by class time on Thursday 2021-03-25.

## What does the Quiz cover?

Quiz A includes material from:

-   the first 9 classes in 432,
-   Chapters 1-14 of the 432 course notes,
-   all of Jeff Leek’s *How to be a modern scientist* and
-   Chapters 1, 2, 6 and 7 of Nate Silver’s *The Signal and the Noise*.

## Writing Code into the Answer Sheet

Occasionally, we ask you to provide R code in your response. You need
not include the `library` command at any time for any of your code.
Assume in all questions that all relevant packages have been loaded in
R. A list of R packages that Dr. Love used in building the Quiz and its
answer sketch is available in the next section.

# Packages and Settings used by Dr. Love

This doesn’t mean you need to use all of these packages, nor does it
mean that you are prevented from using other packages we’ve discussed in
class to complete the Quiz.

``` r
library(knitr)
library(janitor)
library(magrittr)
library(naniar)
library(simputation)
library(rms)
library(broom)
library(tidyverse)

# Note that all data files were downloaded onto 
# my machine into a subfolder called data below
# my main R Project directory for Quiz A.

theme_set(theme_bw())
opts_chunk$set(comment = NA)
options(dplyr.summarise.inform = FALSE)
```
