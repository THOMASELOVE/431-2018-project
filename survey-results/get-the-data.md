# Getting & Managing Data from the 2018 Survey

The data for the survey are available in five separate files, which you will need to merge together to complete your analyses. These files are:

- [`sur2018_01.csv`](https://github.com/THOMASELOVE/431-2018-project/blob/master/survey-results/surv2018_01.csv), which contains items 0 - 20 for the first 25 respondents (ID01 - ID39)
- [`sur2018_02.csv`](https://github.com/THOMASELOVE/431-2018-project/blob/master/survey-results/surv2018_02.csv), which contains items 0 - 20 for the remaining 24 respondents (ID41 - ID99)
- [`sur2018_03.csv`](https://github.com/THOMASELOVE/431-2018-project/blob/master/survey-results/surv2018_03.csv), which contains items 0 and 21 - 90 for all 49 respondents (ID01 - ID99)
- [`sur2018_04.xls`](https://github.com/THOMASELOVE/431-2018-project/blob/master/survey-results/surv2018_04.xls), which is an **Excel** (`.xls`) file (and not our usual `.csv` text file) containing data on items 0 and 91 - 156 and the scales for the first 25 respondents (ID01 - ID39)
- [`sur2018_05.xls`](https://github.com/THOMASELOVE/431-2018-project/blob/master/survey-results/surv2018_05.xls), which is an Excel file containing data on items 0, 91 - 156 and the scales for the remaining 24 respondents (ID41 - ID99)

In order to merge the files, you will need to first read them into R, using appropriate tools for Comma-separated text (`.csv`) and for Excel (`.xls`) files, and then you will need to merge and combine them. Note that each of the five data files contains the respondent ID information, and that will be the key variable for your merging and combination work. 

For detailed advice on this merging and combination, 
- visit [this Results link](https://github.com/THOMASELOVE/431-2018-project/blob/master/survey-results/surv2018_combining-datasets.md) to see the results of my doing the merging and combination work for the 2018 data (so you can make sure we get the same answer). 
- You can also get the [complete R Markdown file](https://github.com/THOMASELOVE/431-2018-project/blob/master/survey-results/surv2018_combining-datasets.Rmd) I used to create those results.

## The Codebook for the Data

The [codebook for the survey data](http://bit.ly/431-2018-survey-data-codebook) has been substantially augmented by Dr. Love, to include information about missing values, frequencies and ranges, as well as advice on collapsing categories. Review the comments posted by Dr. Love for **every** item you use in building your analyses.

An important note is that there are two tabs in the [codebook](http://bit.ly/431-2018-survey-data-codebook). 

- The first tab shows the 165 items and scales that are included in the final data sets.
- The second tab shows the 4 items that were planned for but, for various reasons, were excluded from the final data sets.
    - This includes items 7, 8, 26 and 155. See the [codebook](http://bit.ly/431-2018-survey-data-codebook) for more details.

## Key Steps in Data Management for Project Study 1

1. The first step in your Portfolio (Project Task G) for Study 1 (the Course Survey) will be to create a complete data file, combining all five of these smaller files within R. The code in R that you write must take the original five files (.csv and .xls) and result in an initial tibble (which you will call `sur2018_full` in R) containing all 165 variables described in the [codebook](http://bit.ly/431-2018-survey-data-codebook), for all 49 respondents. As noted above, detailed advice on accomplishing this merging and combination work is now available. Dr. Love's [results are here](https://github.com/THOMASELOVE/431-2018-project/blob/master/survey-results/surv2018_combining-datasets.md), and his [R Markdown code is here](https://github.com/THOMASELOVE/431-2018-project/blob/master/survey-results/surv2018_combining-datasets.Rmd).

2. Having created the `sur2018_full` tibble in R, you will then use the `select` function in the tidyverse to build a new tibble containing only the variables you will actually use (plus the respondent IDs). That new tibble (which you should name something that has meaning for you) will be your analytic data set for each of the six analyses you will perform. This tibble should still have 49 rows (respondents) but it will have many fewer than 165 columns. Of course, you'll have to know exactly which columns you will be using in your Analyses, and that may well require some revision as you move along.

3. Once you have this analytic data set, in addition to whatever other modifications and cleaning (like calculating new variables, or collapsing categories) may be necessary, you will need to tackle any missingness you observe within the variables you have chosen to study. Missing data are identified by NA in the data sets. Of the 165 variables described in the [codebook](http://bit.ly/431-2018-survey-data-codebook), 13 are missing data - in each case, a single value is missing. If you have missing data on any of the variables you study in Project Study 1, then you will need to do simple imputation as part of the associated Study 1 analyses. 
    - In that case, you should show the imputation process in your code, and describe explicitly any choices you made, then run the necessary analysis on **both** the imputed sample and the sample using complete cases alone, and compare the two results in a sentence, as well as showing the results. 
    - You will be using the simple imputation procedure using the `simputation` package in R, that is described [in Chapter 50 of the Course Notes](https://thomaselove.github.io/2018-431-book/missing-data-mechanisms-and-simple-imputation.html#doing-single-imputation-with-simputation).
    - We will demonstrate and discuss simple imputation procedures for the project **before the Thanksgiving break in class USING THE GUIDANCE PROVIDED TO YOU AT TBA - THIS LINK TO BE GENERATED BY TEL**.

Once you have accomplished these steps, the remainder of your analyses can and should be modeled on the template and analyses contained in the [Study 1 demonstration project](https://github.com/THOMASELOVE/431-2018-project/tree/master/demo_study1).

Let us know if you have any questions.
