# Comments on your Survey Comparison Plans

Dr. Love's [comments on your Survey Comparison Plan](http://bit.ly/431-2018-project-survey-plan-feedback) are in this [Google Sheet](http://bit.ly/431-2018-project-survey-plan-feedback). As usual, log into Google via CWRU in order to open the sheet. Find your Homework ID number, and read the row of comments that apply to that HWID. 

You will find the following information:

1. A summary list of which of your analyses (1, 2, 3, 4, 5 or 6) you will need to revise. Please check to be sure I've got this right, against my comments on each individual item. In many cases, even if a revision isn't necessary, you will want to read what I have to say to facilitate your implementing your plan.
    - A specification of which analyses (out of Analyses 1, 2 and 3) you will need to revise. (This just repeats the information from the previous bullet, but restricted to Analyses 1-3.)
    - A specification of which variables for Analyses 4, 5 and 6 (so variables J, K, L, M, and N) you will need to revise. (This just repeats information from the first bullet, but focued on Analyses 4-6.)
2. Dr. Love's comments on your planned Analysis 1, followed by the specifications of what you told us (in the Task D form) you were planning to do for Analysis 1, including variables **A** and **B** if you're using paired samples, and **C** and **Z** if you're using independent samples. The item numbers throughout this document match those in the [codebook](http://bit.ly/431-2018-survey-data-codebook). 
3. Dr. Love's comments on your planned Analysis 2, followed by the specifications of what you told us (in the Task D form) you were planning to do for Analysis 2, and your planned variables **D** and **Y**.
4. Dr. Love's comments on your planned Analysis 3, followed by the specifications of what you told us (in the Task D form) you were planning to do for Analysis 3, and your planned covariate **G**.
5. Dr. Love's comments on your planned factor variable **L**, followed by the specifications of what you told us you were planning to use there. Again, the item numbers throughout this document match the [codebook](http://bit.ly/431-2018-survey-data-codebook). 
6. Similar comments and plans for factor variables **M**, **J**, **K** and then **N**.
7. Comments on your backup variables, and then the specifications of what you planned to use for **Q**, **R**, **S** and **T**. You are not required to use your backup plans in revising your Analyses, but if you do intend to use them, review Dr. Love's comments first.

## What Do I Do Next?

Whether you need to revise your plan or not, you will submit a **STUDY 1 UPDATE** document no later than noon on Wednesday November 28 (the Wednesday after Thanksgiving) via Canvas, as part of what you submit for **Project Task F**.

You will build a single document in HTML, Word or PDF (which should have the filename `study1-update.html` or `study1-update.doc` or `study1-update.pdf`) to specify the changes you have made. In that document, please include the following:

1. The title "Study 1 Update for *Your Name*"
2. A list of the analyses (1, 2, 3, 4, 5 and/or 6) that you are revising here. 
    - This **must** include things that you revised because I told you to do so, but it can also include any other changes you decided to make. 
    - If you are one of the five people who did not require any revisions in Dr. Love's comments, you can stop here if you are making no changes.
3. For each analysis listed above as being revised, specify what your **new** planned analysis will be, including the appropriate item numbers from the [codebook](http://bit.ly/431-2018-survey-data-codebook).
4. Next, in a sentence of two for each revised analysis, demonstrate your understanding of Dr. Love's initial concerns, and show that your new analysis plan is appropriate for the Analysis you are writing about. In **each** analysis, if you created (through collapsing or anything else) new variables here, specify how that was done from the original data that were supplied to you.
    - In Analysis 1, show that you are appropriately using paired samples, or independent samples. Show that you do actually have appropriate types of variables (quantitative in the paired case, one quantitative and one 2-category variable in the independent case).
        - If you are doing independent samples (analysis 1b), tell us the number of people who will be in each group, and tell us the means in each group.
        - If you are doing paired samples (analysis 1a), verify that the two variables you are comparing have the same units (hours, inches, rating points, whatever) and demonstrate that the paired difference is meaningful by explaining what the result is for one of the subjects.
    - In Analysis 2, show that you do actually have a quantitative outcome, and that your factor variable has at least 3 categories. Demonstrate that each group (category) contains data on the quantitative outcome for at least 10 subjects.
    - In Analysis 3, show that you do have a quantitative outcome, and a quantitative covariate, and explain which analysis (1b or 2) you are augmenting with a covariate.
    - In Analysis 4, show the 2x2 table that comes from your choices of L and M (without any imputation) and demonstrate that there are at least 10 subjects in each row and 10 subjects in each column of your table. 
    - In Analysis 5, show the larger cross-tabulation that comes from your choices of J and K (without any imputation) and demonstrate that there are at least 10 subjects in each row and 10 subjects in each column of your table.
    - In Analysis 6, show that your variable N is categorical and has at least 3 levels, each with at least 10 subjects in it. 

# Next Steps

1. If you need to make revisions to your Analyses 1-6 that are necessary in light of Dr. Love's feedback, you need to complete those revisions as soon as possible. Do so using **THIS LINK TO BE GENERATED BY TEL**. If you have questions about completing your revisions, please ask them of the teaching assistants at office hours, or via 431-help.

2. Get to work on merging the data sets and generating the necessary tibbles in R for Project Study 1.

Let us know if you have any questions. Thank you.


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

- The [Study 1 demonstration project](https://github.com/THOMASELOVE/431-2018-project/tree/master/demo_study1) is available now.
- You are to do either Analysis 1a or 1b, but not both.

