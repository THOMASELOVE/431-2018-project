# Project Survey Results

In Project Study 1, you will be responsible for completing six analyses of data from the 49 respondents to the [Course Project Survey](http://bit.ly/431-2018-survey-data-codebook). The data are now available for you to begin your work, as are Dr. Love's [comments on your survey comparison plans](http://bit.ly/431-2018-project-survey-plan-feedback) that you submitted as Project Task D.

## Key Points - What's on this page?

1. The data files you will need are now available, and this page details how you will get them, and provides additional insight into what you should do with the data once you have it, adding to the material in the [Project Instructions](https://thomaselove.github.io/431-2018-project/) and the [Project Study 1 Demonstration](https://github.com/THOMASELOVE/431-2018-project/tree/master/demo_study1). Be sure you've read ALL of this page carefully, along with those other instructions.

2. 5 of the 49 students who submitted a Survey Comparison Plan for Analyses 1-6 had all six of their analyses approved by Dr. Love on their first try. Congratulations to those five people. For everyone else, you will need to make some changes to address Dr. Love's concerns. Details on how to find out what Dr. Love is worried about in your case, and directions as to how to revise your plan, and communicate those revisions back to us, are also available below.

# The Data

The data for the survey are available on this page, in five separate files, which you will need to merge together to complete your analyses. These files are:

- `sur2018_01.csv`, which is a .csv file containing data on items 0 - 20 for the first 25 respondents
- `sur2018_02.csv`, which is a .csv file containing data on items 0 - 20 for the remaining 24 respondents
- `sur2018_03.csv`, which is a .csv file containing data on items 0, 21 - 90 for all 49 respondents
- `sur2018_04.xls`, which is an Excel file containing data on items 0, 91 - 156 and the scales for the first 25 respondents
- `sur2018_05.xls`, which is an Excel file containing data on items 0, 91 - 156 and the scales for the remaining 24 respondents

In order to merge the files, you will need to first read them into R, using appropriate tools for Comma-separated text (`.csv`) and for Excel (`.xls`) files, and then you will need to merge and combine them. Note that each file contains the respondent ID information, and that will be the key variable for your merging and combination work. Tips on accomplishing this merging and combination will be **MADE AVAILABLE TO YOU ON TUESDAY 2018-11-06 AT TBA**.

## The Codebook for the Data

The [codebook for the survey data](http://bit.ly/431-2018-survey-data-codebook) has been substantially augmented by Dr. Love, to include information about missing values, frequencies and ranges for each variable. Review the comments posted by Dr. Love for every item you use in building your analyses.

An important note is that there are two tabs in the [codebook](http://bit.ly/431-2018-survey-data-codebook). 

- The first tab shows the 165 items and scales that are included in the final data sets.
- The second tab shows the 4 items that were planned for but, for various reasons, were excluded from the final data sets.
    - This includes items 7, 8, 26 and 155. See the [codebook](http://bit.ly/431-2018-survey-data-codebook) for more details.

## Key Steps in Data Management for Project Study 1

1. The first step in your Portfolio (Project Task G) for Study 1 (the Course Survey) will be to create a complete data file, combining all five of these smaller files within R. The code in R that you write must take the original five files (.csv and .xls) and result in an initial tibble (which you will call `sur2018_full` in R) containing all 165 variables described in the [codebook](http://bit.ly/431-2018-survey-data-codebook), for all 49 respondents. As noted above, tips on accomplishing this merging and combination work will be **MADE AVAILABLE TO YOU ON TUESDAY 2018-11-06 AT TBA**.

2. Having created the `sur2018_full` tibble in R, you will then use the `select` function in the tidyverse to build a new tibble containing only the variables you will actually use (plus the respondent IDs). That new tibble (which you should name something that has meaning for you) will be your analytic data set for each of the six analyses you will perform. This tibble should still have 49 rows (respondents) but it will have many fewer than 165 columns.

3. Once you have this analytic data set, in addition to whatever other modifications you deem necessary, you will also need to tackle any missingness you observe within the variables you have chosen to study. Missing data are identified by NA in the data sets. Of the 165 variables described in the [codebook](http://bit.ly/431-2018-survey-data-codebook), 13 are missing data - in each case, a single value. If you have missing data on any of the variables you study in Project Study 1, then you will need to do simple imputation as part of the associated Study 1 analyses. 
    - In that case, you should show the imputation process in your code, and describe explicitly any choices you made, then run the necessary analysis on both the imputed sample and the sample using complete cases alone, and compare the two results. 
    - You will be using the simple imputation procedure using the `simputation` package in R, that is described [in Chapter 50 of the Course Notes](https://thomaselove.github.io/2018-431-book/missing-data-mechanisms-and-simple-imputation.html#doing-single-imputation-with-simputation).
    - We will demonstrate and discuss simple imputation procedures for the project **before the Thanksgiving break in class USING THE GUIDANCE PROVIDED TO YOU AT TBA**.

Once you have accomplished these steps, the remainder of your analyses can be modeled on the template and analyses contained in the [Study 1 demonstration project](https://github.com/THOMASELOVE/431-2018-project/tree/master/demo_study1).

# Comments on your Survey Comparison Plans

Dr. Love's [comments on your Survey Comparison Plan](http://bit.ly/431-2018-project-survey-plan-feedback) are in the Google Sheet at [http://bit.ly/431-2018-project-survey-plan-feedback](http://bit.ly/431-2018-project-survey-plan-feedback). As usual, log into Google via CWRU in order to open the sheet.

In that sheet, find your Homework ID number, and read the row of comments that apply to that HWID. You will find the following information:

- A specification of which analyses (out of Analyses 1, 2 and 3) you will need to revise.
- A specification of which variables for Analyses 4, 5 and 6 (so variables J, K, L, M, and N) you will need to revise.
- Dr. Love's comments on your planned Analysis 1, followed by the specifications of what you told us (in the Task D form) you were planning to do for Analysis 1, including variables **A** and **B** if you're using paired samples, and **C** and **Z** if you're using independent samples. The item numbers throughout this document match those in the [codebook](http://bit.ly/431-2018-survey-data-codebook). 
- Dr. Love's comments on your planned Analysis 2, followed by the specifications of what you told us (in the Task D form) you were planning to do for Analysis 2, and your planned variables **D** and **Y**.
- Dr. Love's comments on your planned Analysis 3, followed by the specifications of what you told us (in the Task D form) you were planning to do for Analysis 3, and your planned covariate **G**.
- Dr. Love's comments on your planned factor variable **L**, followed by the specifications of what you told us you were planning to use there. Again, the item numbers throughout this document match the [codebook](http://bit.ly/431-2018-survey-data-codebook). 
- Similar comments and plans for factor variables **M**, **J**, **K** and then **N**.
- Comments on your backup variables, and then the specifications of what you planned to use for **Q**, **R**, **S** and **T**.


### What You Specified in Task D (The Survey Comparison Plan)

Analysis | Items Described in Your Plan
:----: | --------------------------------------------------------------------------------
[1a](https://thomaselove.github.io/431-2018-project/taskD.html#analysis-1-comparing-the-means-of-two-populations) | **A** and **B** (each quantitative)
[1b](https://thomaselove.github.io/431-2018-project/taskD.html#analysis-1-comparing-the-means-of-two-populations) | **C** (quantitative), **Z** (two categories)
[2](https://thomaselove.github.io/431-2018-project/taskD.html#analysis-2-comparing-the-means-of-three-or-more-populations) | **D** (quantitative), **Y** (3-6 categories)
[3](https://thomaselove.github.io/431-2018-project/taskD.html#analysis-3-regression-model-with-one-covariate) | **E** (quantitative, same as C or D), **X** (same as Y or Z), **G** (quantitative, not C or D)
[4](https://thomaselove.github.io/431-2018-project/taskD.html#analysis-4-comparing-two-population-proportions) | **L** and **M** (each two categories)
[5](https://thomaselove.github.io/431-2018-project/taskD.html#analysis-5-a-larger-two-way-table) | **J** (2-6 categories), **K** (3-6 categories)
[6](https://thomaselove.github.io/431-2018-project/taskD.html#analysis-6-comparing-population-proportions-in-a-2x2xn-contingency-table) | **L**, **M**, and **N** (3-6 categories)
[Backups](https://thomaselove.github.io/431-2018-project/taskD.html#backups) | *Q* and *R* (quantitative), *S* (2 categories), *T* (3-6 categories)

### Reminders

- The Study 1 demonstration project is available at https://github.com/THOMASELOVE/431-2018-project/tree/master/demo_study1.
- You are to do either Analysis 1a or 1b, but not both.

# Next Steps

