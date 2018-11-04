431 Project Advice: Combining Data Sets
================
Thomas E. Love
2018-11-04

# R Packages We’ll Use

I’ll load three packages here.

``` r
library(Hmisc)
library(readxl)
library(tidyverse)
```

# Read in the data

## Read in the data from the comma-separated text (`.csv`) files

Note that I’m using `read_csv` here deliberately, rather than any other
approach. This will make the merging easier later, even though it will
leave us with the problem of (eventually) changing the class of many
variables from `character` to `factor`.

  - Note that I used `message = FALSE` in this chunk heading to suppress
    some very detailed (but not critical for us to look at) output.

<!-- end list -->

``` r
surv2018_01 <- read_csv("surv2018_01.csv")
surv2018_02 <- read_csv("surv2018_02.csv")
surv2018_03 <- read_csv("surv2018_03.csv")
```

Each of these results in a `tibble` of data. The numbers of rows and
columns in each of these can be obtained, to check your work so far.

``` r
dim(surv2018_01)
```

    [1] 25 19

``` r
dim(surv2018_02)
```

    [1] 24 19

``` r
dim(surv2018_03)
```

    [1] 49 70

Make sure that your dimensions match mine.

## Read in the data from the Excel files

Note that I’m using `read_xls` here, which is part of the `readxl`
package, which is why I loaded that earlier. This will make the merging
with the things I’ve already created easier later, even though it will
also leave us with the problem of (eventually) changing the class of
many variables from `character` to `factor`.

  - Note that I had to specify here that a value of either a blank or
    `NA` in the Excel sheet means “missing”, which I didn’t have to do
    for `read_csv`. `read_csv`, by default, recognizes both blank cells
    and NA cells as missing, but `read_xls` only recognizes blanks by
    default.
  - Note that I again used `message = FALSE` in this chunk heading to
    suppress some very detailed (but not critical for us to look at)
    output.

<!-- end list -->

``` r
surv2018_04 <- read_xls("surv2018_04.xls", na = c("", "NA"))
surv2018_05 <- read_xls("surv2018_05.xls", na = c("", "NA"))
```

Each of these results in a `tibble` of data. The numbers of rows and
columns in each of these can be obtained, to check your work so far.

``` r
dim(surv2018_04)
```

    [1] 25 78

``` r
dim(surv2018_05)
```

    [1] 24 78

Make sure that your dimensions match mine.

# Merge the tibbles you have created

## Merge the data in `surv2018_01` with `surv2018_02`, forming `temp12`

This next bit of code will produce a data frame containing what we need
from data sets 1 and 2, putting together the first 25 respondents (in
`surv2018_01`) and the remaining 24 (in `surv2018_02`) all together.
Since `surv2018_01` and `surv2018_02` contain the same columns of
information, and just different rows, we can do this easily with the
`bind_rows` function from the `tidyverse`.

Again, I’ll output the dimensions of the result, so that we can check
that you and I get the same results.

``` r
temp12 <- bind_rows(surv2018_01, surv2018_02)

dim(temp12)
```

    [1] 49 19

## Merge the data in `surv2018_04` with `surv2018_05`, forming `temp45`

This will produce a data frame containing what we need from data sets 4
and 5, putting together the first 25 respondents (in `surv2018_04`) and
the remaining 24 (in `surv2018_05`) all together. Since `surv2018_04`
and `surv2018_05` contain the same columns of information, and just
different rows, we can do this easily with the `bind_rows` function from
the `tidyverse`.

As before, I’ll output the dimensions of the result, so that we can
check that you and I get the same results.

``` r
temp45 <- bind_rows(surv2018_04, surv2018_05)
dim(temp45)
```

    [1] 49 78

## Merge the data in `temp12` with `surv2018_03`

This will produce a data frame containing what we need from data sets 1,
2 and 3, all together.

``` r
temp123 <- left_join(temp12, surv2018_03, by = "response_id")
dim(temp123)
```

    [1] 49 88

## Merge the data in `temp123` and `temp45`

This will produce a data frame containing what we need from data sets 1,
2, 3, 4 and 5 all together.

``` r
temp12345 <- left_join(temp123, temp45, by = "response_id")
dim(temp12345)
```

    [1]  49 165

# Create the `surv2018_full` tibble we all need

## Create the `surv2018_full` tibble

Here, we change all of the variables of “character” class to factor so
they will do what we expect in analyses, and then back out of that for
`response_id` which we’d like to keep as a “character”.

``` r
surv2018_full <- temp12345 %>%
    mutate_if(is.character, funs(as.factor(.))) %>%
    mutate(response_id = as.character(response_id))
```

## Remove the temporary data sets you built from your environment

``` r
rm(temp12, temp45, temp123, temp12345)
```

## Remove the raw data sets you initially imported from your environment

``` r
rm(surv2018_01, surv2018_02, surv2018_03, surv2018_04, surv2018_05)
```

We won’t need these again.

# What your version of `surv2018_full` should look like

The rest of this output can be used to check to see that what you have
and what I have are the same. Once your results match mine, then I
encourage you to move on to selecting the actual variables you will need
for you analysis, and building your analytic data set.

``` r
Hmisc::describe(surv2018_full)
```

    surv2018_full 
    
     165  Variables      49  Observations
    ---------------------------------------------------------------------------
    response_id 
           n  missing distinct 
          49        0       49 
    
    lowest : ID01 ID03 ID04 ID06 ID07, highest: ID86 ID90 ID95 ID96 ID99
    ---------------------------------------------------------------------------
    usborn_001 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     19    30
    Proportion 0.388 0.612
    ---------------------------------------------------------------------------
    english_002 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     13    36
    Proportion 0.265 0.735
    ---------------------------------------------------------------------------
    female_003 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     21    28
    Proportion 0.429 0.571
    ---------------------------------------------------------------------------
    glasses_004 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     10    39
    Proportion 0.204 0.796
    ---------------------------------------------------------------------------
    r_pre431_005 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     22    27
    Proportion 0.449 0.551
    ---------------------------------------------------------------------------
    domestic_006 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     30    19
    Proportion 0.612 0.388
    ---------------------------------------------------------------------------
    yearborn_009 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       20    0.987     1990    7.352     1975     1979 
         .25      .50      .75      .90      .95 
        1987     1994     1995     1996     1997 
                                                                          
    Value       1971  1974  1976  1977  1980  1982  1983  1985  1986  1987
    Frequency      2     1     1     1     1     1     2     1     2     1
    Proportion 0.041 0.020 0.020 0.020 0.020 0.020 0.041 0.020 0.041 0.020
                                                                          
    Value       1988  1989  1990  1992  1993  1994  1995  1996  1997  1998
    Frequency      1     1     4     2     2     9     8     6     2     1
    Proportion 0.020 0.020 0.082 0.041 0.041 0.184 0.163 0.122 0.041 0.020
    ---------------------------------------------------------------------------
    sroh_010 
           n  missing distinct 
          49        0        4 
                                                      
    Value      Excellent      Fair      Good Very Good
    Frequency         10         3        21        15
    Proportion     0.204     0.061     0.429     0.306
    ---------------------------------------------------------------------------
    neohio_011 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       37    0.995    94.35    123.8      3.0      4.0 
         .25      .50      .75      .90      .95 
        11.0     38.0    118.0    266.2    320.4 
    
    lowest :   2   3   4  11  12, highest: 271 297 336 463 573
    ---------------------------------------------------------------------------
    height_012 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          48        1       15    0.989    66.02    4.714    61.00    61.70 
         .25      .50      .75      .90      .95 
       63.00    65.50    70.00    71.00    72.65 
                                                                          
    Value         54    60    61    62    63    64    65    66    67    68
    Frequency      1     1     3     3     9     3     4     2     4     5
    Proportion 0.021 0.021 0.062 0.062 0.188 0.062 0.083 0.042 0.083 0.104
                                            
    Value         70    71    72    73    74
    Frequency      5     4     1     1     2
    Proportion 0.104 0.083 0.021 0.021 0.042
    ---------------------------------------------------------------------------
    weight_013 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          48        1       33    0.999    148.4    33.52    105.0    109.1 
         .25      .50      .75      .90      .95 
       131.5    145.0    168.5    190.0    196.9 
    
    lowest :  95 100 105 107 110, highest: 190 195 198 200 218
    ---------------------------------------------------------------------------
    pulse_014 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          48        1       22    0.987    72.29    14.63    54.35    58.80 
         .25      .50      .75      .90      .95 
       64.00    68.00    80.00    92.60    98.60 
    
    lowest :  45  54  55  56  60, highest:  92  94  96 100 112
    ---------------------------------------------------------------------------
    exercise_015 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.952    1.939    2.043 
                                                        
    Value          0     1     2     3     4     5     7
    Frequency     16     7     7     9     5     4     1
    Proportion 0.327 0.143 0.143 0.184 0.102 0.082 0.020
    ---------------------------------------------------------------------------
    sleep_016 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       14    0.979    6.904    1.845      3.6      5.0 
         .25      .50      .75      .90      .95 
         6.0      7.0      8.0      8.6      9.0 
                                                                          
    Value        2.0   3.0   4.5   5.0   5.5   6.0   7.0   7.5   7.7   8.0
    Frequency      1     2     1     3     1    10    10     3     1     7
    Proportion 0.020 0.041 0.020 0.061 0.020 0.204 0.204 0.061 0.020 0.143
                                      
    Value        8.1   8.5   9.0  10.0
    Frequency      1     4     3     2
    Proportion 0.020 0.082 0.061 0.041
    ---------------------------------------------------------------------------
    sleepq_017 
           n  missing distinct 
          49        0        5 
                                                                
    Value      Excellent      Fair      Good      Poor Very Good
    Frequency          2        13        18         4        12
    Proportion     0.041     0.265     0.367     0.082     0.245
    ---------------------------------------------------------------------------
    checkup_018 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     26    23
    Proportion 0.531 0.469
    ---------------------------------------------------------------------------
    kids_019 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     40     9
    Proportion 0.816 0.184
    ---------------------------------------------------------------------------
    workhrs_020 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       17    0.935    18.95     21.8        0        0 
         .25      .50      .75      .90      .95 
           0       12       40       46       50 
                                                                          
    Value        0.0   3.0   8.0   9.0  10.0  12.0  15.0  16.0  20.0  22.5
    Frequency     19     1     1     1     1     3     1     1     2     1
    Proportion 0.388 0.020 0.020 0.020 0.020 0.061 0.020 0.020 0.041 0.020
                                                        
    Value       25.0  35.0  40.0  44.0  45.0  50.0  60.0
    Frequency      1     1     9     1     1     4     1
    Proportion 0.020 0.020 0.184 0.020 0.020 0.082 0.020
    ---------------------------------------------------------------------------
    politic_021 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     14    35
    Proportion 0.286 0.714
    ---------------------------------------------------------------------------
    student_022 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     13    36
    Proportion 0.265 0.735
    ---------------------------------------------------------------------------
    redmeat_023 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     10    39
    Proportion 0.204 0.796
    ---------------------------------------------------------------------------
    seafood_024 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     15    34
    Proportion 0.306 0.694
    ---------------------------------------------------------------------------
    vegetar_025 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     42     7
    Proportion 0.857 0.143
    ---------------------------------------------------------------------------
    flushot_027 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     18    31
    Proportion 0.367 0.633
    ---------------------------------------------------------------------------
    bodysat_028 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     17    32
    Proportion 0.347 0.653
    ---------------------------------------------------------------------------
    ecigever_029 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     42     7
    Proportion 0.857 0.143
    ---------------------------------------------------------------------------
    votereg_030 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     15    34
    Proportion 0.306 0.694
    ---------------------------------------------------------------------------
    voteprez_031 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     18    31
    Proportion 0.367 0.633
    ---------------------------------------------------------------------------
    voteint_032 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     14    35
    Proportion 0.286 0.714
    ---------------------------------------------------------------------------
    sleepok_033 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     21    28
    Proportion 0.429 0.571
    ---------------------------------------------------------------------------
    steps_034 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     29    20
    Proportion 0.592 0.408
    ---------------------------------------------------------------------------
    shoppol_035 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     32    17
    Proportion 0.653 0.347
    ---------------------------------------------------------------------------
    retire_036 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     29    20
    Proportion 0.592 0.408
    ---------------------------------------------------------------------------
    carloan_037 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     36    13
    Proportion 0.735 0.265
    ---------------------------------------------------------------------------
    sickdays_038 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       11     0.98    3.571    3.821      0.0      0.0 
         .25      .50      .75      .90      .95 
         1.0      3.0      5.0      7.6     10.0 
                                                                          
    Value          0     1     2     3     4     5     6     7    10    12
    Frequency     10     6     6     9     4     5     2     2     3     1
    Proportion 0.204 0.122 0.122 0.184 0.082 0.102 0.041 0.041 0.061 0.020
                    
    Value         21
    Frequency      1
    Proportion 0.020
    ---------------------------------------------------------------------------
    selfies_039 
           n  missing distinct     Info     Mean      Gmd 
          49        0        9    0.769    1.959    3.253 
                                                                    
    Value          0     1     2     3     4     5     6    10    36
    Frequency     30     3     5     3     3     2     1     1     1
    Proportion 0.612 0.061 0.102 0.061 0.061 0.041 0.020 0.020 0.020
    ---------------------------------------------------------------------------
    credits_040 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       17    0.982    9.806    5.357      3.0      3.9 
         .25      .50      .75      .90      .95 
         7.0      9.0     13.0     18.0     18.0 
                                                                          
    Value        3.0   3.5   4.0   5.0   6.0   7.0   8.0   9.0  10.0  11.0
    Frequency      4     1     4     2     1     2     3    12     3     3
    Proportion 0.082 0.020 0.082 0.041 0.020 0.041 0.061 0.245 0.061 0.061
                                                        
    Value       12.0  13.0  14.0  15.0  16.0  18.0  21.0
    Frequency      1     2     1     3     1     5     1
    Proportion 0.020 0.041 0.020 0.061 0.020 0.102 0.020
    ---------------------------------------------------------------------------
    hrs431_041 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       20    0.994    8.327    5.383      3.5      3.5 
         .25      .50      .75      .90      .95 
         4.5      7.0     10.0     15.2     18.4 
                                                                          
    Value       3.00  3.50  4.00  4.50  4.52  5.00  6.00  7.00  8.00  8.50
    Frequency      2     4     5     2     1     4     5     3     5     1
    Proportion 0.041 0.082 0.102 0.041 0.020 0.082 0.102 0.061 0.102 0.020
                                                                          
    Value       9.00 10.00 11.00 12.00 14.00 15.00 16.00 20.00 25.00 26.00
    Frequency      2     5     1     2     1     1     2     1     1     1
    Proportion 0.041 0.102 0.020 0.041 0.020 0.020 0.041 0.020 0.020 0.020
    ---------------------------------------------------------------------------
    hrsclass_042 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       18    0.993    12.33    12.34        0        0 
         .25      .50      .75      .90      .95 
           4       10       20       30       30 
                                                                          
    Value          0     1     2     3     4     5     6     8    10    11
    Frequency      6     1     3     1     3     4     3     3     4     1
    Proportion 0.122 0.020 0.061 0.020 0.061 0.082 0.061 0.061 0.082 0.020
                                                              
    Value         12    15    16    20    22    30    34    50
    Frequency      1     5     1     4     1     6     1     1
    Proportion 0.020 0.102 0.020 0.082 0.020 0.122 0.020 0.020
    ---------------------------------------------------------------------------
    device_043 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       14    0.848    4.592    7.299      0.0      0.0 
         .25      .50      .75      .90      .95 
         0.0      0.0      5.0     14.8     22.8 
                                                                          
    Value          0     1     3     4     5     7     8    10    14    18
    Frequency     26     7     1     2     1     2     1     1     3     1
    Proportion 0.531 0.143 0.020 0.041 0.020 0.041 0.020 0.020 0.061 0.020
                                      
    Value         21    24    28    37
    Frequency      1     1     1     1
    Proportion 0.020 0.020 0.020 0.020
    ---------------------------------------------------------------------------
    relax_044 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          48        1       10    0.869    2.083        3     0.00     0.00 
         .25      .50      .75      .90      .95 
        0.00     0.50     3.00     5.30     8.95 
                                                                          
    Value          0     1     2     3     4     5     6     7    10    14
    Frequency     24     4     4     8     2     1     1     1     1     2
    Proportion 0.500 0.083 0.083 0.167 0.042 0.021 0.021 0.021 0.021 0.042
    ---------------------------------------------------------------------------
    computer_045 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       29    0.997       50    28.71     15.8     19.6 
         .25      .50      .75      .90      .95 
        30.0     41.0     70.0     86.0     94.8 
    
    lowest :   9  15  17  18  20, highest:  80  85  90  98 108
    ---------------------------------------------------------------------------
    tvmovies_046 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       22    0.994    10.32    7.713      2.0      3.0 
         .25      .50      .75      .90      .95 
         5.0      8.0     14.0     20.0     20.6 
    
    lowest :  0.0  2.0  3.0  4.5  5.0, highest: 18.0 20.0 21.0 30.0 35.0
    ---------------------------------------------------------------------------
    polmedia_047 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       14    0.982    3.449    3.662        0        0 
         .25      .50      .75      .90      .95 
           1        2        5        7       10 
                                                                          
    Value        0.0   0.5   1.0   2.0   2.5   3.0   4.0   4.5   5.0   6.0
    Frequency      6     2    10     9     1     4     2     1     2     3
    Proportion 0.122 0.041 0.204 0.184 0.020 0.082 0.041 0.020 0.041 0.061
                                      
    Value        7.0  10.0  12.0  18.0
    Frequency      5     2     1     1
    Proportion 0.102 0.041 0.020 0.020
    ---------------------------------------------------------------------------
    hobby_048 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          48        1       18    0.986    5.562    6.347    0.000    0.000 
         .25      .50      .75      .90      .95 
       1.375    4.000    7.125   14.000   17.650 
                                                                          
    Value        0.0   1.0   1.5   2.0   3.0   4.0   5.0   6.0   7.0   7.5
    Frequency     10     2     1     4     6     6     4     2     1     1
    Proportion 0.208 0.042 0.021 0.083 0.125 0.125 0.083 0.042 0.021 0.021
                                                              
    Value        8.0  10.0  12.0  14.0  17.0  18.0  21.0  31.0
    Frequency      1     2     2     2     1     1     1     1
    Proportion 0.021 0.042 0.042 0.042 0.021 0.021 0.021 0.021
    ---------------------------------------------------------------------------
    inperson_049 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       24    0.997    14.76    15.87      1.0      2.0 
         .25      .50      .75      .90      .95 
         5.0      8.0     18.0     40.0     44.8 
    
    lowest :  0  1  2  3  4, highest: 35 40 48 55 84
    ---------------------------------------------------------------------------
    playing_050 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       13    0.964    2.898    3.165      0.0      0.0 
         .25      .50      .75      .90      .95 
         0.0      2.0      4.0      7.1      8.0 
                                                                          
    Value        0.0   1.0   1.5   2.0   3.0   4.0   5.0   6.0   7.0   7.5
    Frequency     14     2     1    10     7     5     2     1     2     1
    Proportion 0.286 0.041 0.020 0.204 0.143 0.102 0.041 0.020 0.041 0.020
                                
    Value        8.0  10.0  14.0
    Frequency      2     1     1
    Proportion 0.041 0.020 0.020
    ---------------------------------------------------------------------------
    cooking_051 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       13    0.989    4.327    3.627      0.0      0.0 
         .25      .50      .75      .90      .95 
         2.0      4.0      7.0      8.4     10.0 
                                                                          
    Value       0.00  1.00  2.00  3.00  3.75  4.00  5.00  5.25  6.00  7.00
    Frequency      6     6     4     5     1     5     5     1     2     7
    Proportion 0.122 0.122 0.082 0.102 0.020 0.102 0.102 0.020 0.041 0.143
                                
    Value       8.00 10.00 12.00
    Frequency      2     4     1
    Proportion 0.041 0.082 0.020
    ---------------------------------------------------------------------------
    sports_052 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       12    0.865    2.357    3.509      0.0      0.0 
         .25      .50      .75      .90      .95 
         0.0      0.0      3.0      7.2      8.0 
                                                                          
    Value        0.0   0.5   1.0   2.0   3.0   4.0   5.0   6.0   7.0   8.0
    Frequency     25     1     5     2     4     2     2     2     1     3
    Proportion 0.510 0.020 0.102 0.041 0.082 0.041 0.041 0.041 0.020 0.061
                          
    Value       15.0  18.0
    Frequency      1     1
    Proportion 0.020 0.020
    ---------------------------------------------------------------------------
    emedia_053 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       31    0.996    61.98    29.37     20.0     24.8 
         .25      .50      .75      .90      .95 
        40.0     61.0     75.0     98.0    103.0 
    
    lowest :  13  15  20  24  25, highest:  98 100 105 110 115
    ---------------------------------------------------------------------------
    employ_054 
           n  missing distinct 
          49        0        3 
                                                                       
    Value      Employed full-time Employed part-time       Not employed
    Frequency                  21                  8                 20
    Proportion              0.429              0.163              0.408
    ---------------------------------------------------------------------------
    redblue_055 
           n  missing distinct 
          49        0        7 
    
    Conservative (3, 0.061), Liberal (13, 0.265), Moderate (7, 0.143),
    Somewhat conservative (4, 0.082), Somewhat liberal (8, 0.163), Very
    conservative (1, 0.020), Very liberal (13, 0.265)
    ---------------------------------------------------------------------------
    fluvax_056 
           n  missing distinct 
          49        0        4 
                                                                  
    Value        Every Year   Most Years        Never Occasionally
    Frequency            23            9            2           15
    Proportion        0.469        0.184        0.041        0.306
    ---------------------------------------------------------------------------
    os_057 
           n  missing distinct 
          49        0        3 
                                            
    Value          Linux Macintosh   Windows
    Frequency          1        19        29
    Proportion     0.020     0.388     0.592
    ---------------------------------------------------------------------------
    ecigyear_058 
           n  missing distinct 
          49        0        4 
                                                                    
    Value        Somewhat likely Somewhat unlikely       Very likely
    Frequency                  2                 1                 2
    Proportion             0.041             0.020             0.041
                                
    Value          Very unlikely
    Frequency                 44
    Proportion             0.898
    ---------------------------------------------------------------------------
    uspol_059 
           n  missing distinct 
          49        0        3 
                                                                       
    Value      Not closely at all   Somewhat closely       Very closely
    Frequency                  12                 19                 18
    Proportion              0.245              0.388              0.367
    ---------------------------------------------------------------------------
    rallies_060 
           n  missing distinct 
          49        0        3 
    
    No, I have not, and I am not interested in doing so in the future (15,
    0.306), No, I have not, but I am interested in doing so in the future.
    (15, 0.306), Yes, I have. (19, 0.388)
    ---------------------------------------------------------------------------
    degree_061 
           n  missing distinct 
          49        0        4 
                                                                              
    Value                            Doctorate                        Master's
    Frequency                               16                              24
    Proportion                           0.327                           0.490
                                                                              
    Value      Not currently pursuing a degree                   Undergraduate
    Frequency                                4                               5
    Proportion                           0.082                           0.102
    ---------------------------------------------------------------------------
    eatlate_062 
           n  missing distinct 
          49        0        5 
                                                                              
    Value          Every night     Most nights           Never Once in a while
    Frequency                1               9               3              25
    Proportion           0.020           0.184           0.061           0.510
                              
    Value            Sometimes
    Frequency               11
    Proportion           0.224
    ---------------------------------------------------------------------------
    fruitveg_063 
           n  missing distinct 
          49        0        4 
                                                                  
    Value             Daily   Frequently Occasionally       Seldom
    Frequency            11           20           13            5
    Proportion        0.224        0.408        0.265        0.102
    ---------------------------------------------------------------------------
    vitamin_064 
           n  missing distinct 
          49        0        5 
                                                                  
    Value             Daily   Frequently        Never Occasionally
    Frequency            11           19            2           11
    Proportion        0.224        0.388        0.041        0.224
                           
    Value            Seldom
    Frequency             6
    Proportion        0.122
    ---------------------------------------------------------------------------
    fried_065 
           n  missing distinct 
          49        0        5 
                                                                  
    Value             Daily   Frequently        Never Occasionally
    Frequency             1            3            5           21
    Proportion        0.020        0.061        0.102        0.429
                           
    Value            Seldom
    Frequency            19
    Proportion        0.388
    ---------------------------------------------------------------------------
    icecream_066 
           n  missing distinct 
          49        0        4 
                                                                  
    Value        Frequently        Never Occasionally       Seldom
    Frequency             3            9           14           23
    Proportion        0.061        0.184        0.286        0.469
    ---------------------------------------------------------------------------
    living_067 
           n  missing distinct 
          49        0        3 
                                      
    Value      Neither     Own    Rent
    Frequency        5      12      32
    Proportion   0.102   0.245   0.653
    ---------------------------------------------------------------------------
    resil_068 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       32    0.998    76.69    24.13     34.0     41.0 
         .25      .50      .75      .90      .95 
        67.0     84.0     94.0     99.2    100.0 
    
    lowest :  13  19  34  41  46, highest:  95  96  98  99 100
    ---------------------------------------------------------------------------
    anxiety_069 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       35    0.999    57.84    28.46     14.4     23.0 
         .25      .50      .75      .90      .95 
        38.0     62.0     77.0     88.4     92.6 
    
    lowest :  7 12 18 23 24, highest: 88 90 92 93 95
    ---------------------------------------------------------------------------
    headache_070 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       34    0.997    24.98    27.94      0.0      0.0 
         .25      .50      .75      .90      .95 
         6.0     14.0     34.0     68.6     79.6 
    
    lowest :  0  1  2  5  6, highest: 71 73 84 86 88
    ---------------------------------------------------------------------------
    fluimp_071 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       33    0.998    63.49    38.89      1.0     11.4 
         .25      .50      .75      .90      .95 
        36.0     74.0     95.0    100.0    100.0 
    
    lowest :   0   1   5  13  18, highest:  95  96  97  99 100
    ---------------------------------------------------------------------------
    healthimp_072 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       28    0.996    79.65    20.23     44.8     56.0 
         .25      .50      .75      .90      .95 
        69.0     85.0     92.0    100.0    100.0 
    
    lowest :   9  34  44  46  52, highest:  93  94  96  98 100
    ---------------------------------------------------------------------------
    bigthree_073 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       28    0.994    80.71    19.46     46.2     61.0 
         .25      .50      .75      .90      .95 
        73.0     83.0     97.0    100.0    100.0 
    
    lowest :  24  36  45  48  53, highest:  95  97  98  99 100
    ---------------------------------------------------------------------------
    takecare_074 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       28    0.997    82.24    14.43     53.6     66.0 
         .25      .50      .75      .90      .95 
        77.0     83.0     91.0     98.4    100.0 
    
    lowest :  43  47  52  56  66, highest:  94  96  97  98 100
    ---------------------------------------------------------------------------
    prevent_075 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       34    0.999    65.98    25.73     26.6     37.4 
         .25      .50      .75      .90      .95 
        53.0     67.0     86.0     93.4     99.2 
    
    lowest :   7  13  21  35  38, highest:  92  93  95  98 100
    ---------------------------------------------------------------------------
    allican_076 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       38    0.999    50.63    31.21     12.4     14.8 
         .25      .50      .75      .90      .95 
        26.0     55.0     74.0     86.0     87.2 
    
    lowest :  1  2 12 13 14, highest: 82 86 88 90 95
    ---------------------------------------------------------------------------
    coder_077 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       25    0.981    14.76     19.1      0.0      0.0 
         .25      .50      .75      .90      .95 
         0.0      7.0     22.0     50.4     53.6 
    
    lowest :  0  1  2  4  6, highest: 52 53 54 58 59
    ---------------------------------------------------------------------------
    toomuch_078 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          48        1       35    0.998    36.06    31.92     0.00     2.00 
         .25      .50      .75      .90      .95 
       11.75    29.00    60.00    73.30    75.30 
    
    lowest :  0  2  3  4  6, highest: 73 74 76 83 95
    ---------------------------------------------------------------------------
    carddebt_079 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       27    0.995    21.96    28.27      0.0      0.0 
         .25      .50      .75      .90      .95 
         3.0      6.0     47.0     64.2     75.0 
    
    lowest :   0   1   2   3   4, highest:  69  72  77  87 100
    ---------------------------------------------------------------------------
    indebt_080 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       26    0.984     22.8    30.84      0.0      0.0 
         .25      .50      .75      .90      .95 
         1.0      7.0     33.0     78.8     98.4 
    
    lowest :   0   1   2   3   4, highest:  69  76  90  96 100
    ---------------------------------------------------------------------------
    stress_081 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       38    0.999    56.86    31.33     15.4     22.0 
         .25      .50      .75      .90      .95 
        35.0     62.0     83.0     88.0     93.0 
    
    lowest :  5  8 15 16 22, highest: 87 88 93 94 97
    ---------------------------------------------------------------------------
    later_082 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       34    0.999     45.1    33.28      3.0      6.8 
         .25      .50      .75      .90      .95 
        21.0     50.0     66.0     80.0     90.6 
    
    lowest :   0   2   3   6   7, highest:  84  87  93  98 100
    ---------------------------------------------------------------------------
    ecigquit_083 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       38    0.998     41.9    34.09      0.0      0.0 
         .25      .50      .75      .90      .95 
        16.0     43.0     63.0     81.2     91.0 
    
    lowest :   0   3   5  12  13, highest:  86  88  93  95 100
    ---------------------------------------------------------------------------
    ecigadd_084 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       32    0.998    30.67    27.57      0.0      3.2 
         .25      .50      .75      .90      .95 
         7.0     27.0     51.0     66.2     73.4 
    
    lowest :  0  4  5  6  7, highest: 65 71 75 76 78
    ---------------------------------------------------------------------------
    ecigsec_085 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       39    0.999    31.37    27.82      0.0      2.8 
         .25      .50      .75      .90      .95 
        15.0     25.0     46.0     65.0     79.8 
    
    lowest :  0  2  3  4  5, highest: 65 75 83 89 95
    ---------------------------------------------------------------------------
    workout_086 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       40    0.999     44.8    35.73      2.6      7.4 
         .25      .50      .75      .90      .95 
        18.0     44.0     74.0     83.0     91.8 
    
    lowest :  0  1  5  8  9, highest: 87 90 93 94 97
    ---------------------------------------------------------------------------
    bedtime_087 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       36    0.999    38.63    36.02      0.4      1.8 
         .25      .50      .75      .90      .95 
        13.0     25.0     64.0     82.2     95.2 
    
    lowest :   0   1   2   3   4, highest:  81  87  94  96 100
    ---------------------------------------------------------------------------
    labels_088 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       34    0.997    58.14    38.34      0.8      4.6 
         .25      .50      .75      .90      .95 
        34.0     67.0     87.0    100.0    100.0 
    
    lowest :   0   2   3   5  14, highest:  93  94  95  98 100
    ---------------------------------------------------------------------------
    beliefs_089 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       37    0.998    33.96    32.99      0.0      0.0 
         .25      .50      .75      .90      .95 
         6.0     27.0     55.0     67.8     84.8 
    
    lowest :   0   2   3   4   5, highest:  71  74  92  96 100
    ---------------------------------------------------------------------------
    satis_090 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       37    0.999    64.18    25.72     16.4     25.2 
         .25      .50      .75      .90      .95 
        55.0     68.0     80.0     88.0     93.6 
    
    lowest :  12  13  16  17  18, highest:  88  93  94  98 100
    ---------------------------------------------------------------------------
    s_dgs 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          48        1       22    0.996    54.12    7.417    41.05    46.70 
         .25      .50      .75      .90      .95 
       50.75    55.50    59.00    60.60    63.65 
    
    lowest : 37 40 43 46 47, highest: 62 63 64 65 66
    ---------------------------------------------------------------------------
    dgs01_091 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7     0.96    3.837    2.068 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      2    15     6     8     7     6     5
    Proportion 0.041 0.306 0.122 0.163 0.143 0.122 0.102
    ---------------------------------------------------------------------------
    dgs02_092 
           n  missing distinct     Info     Mean      Gmd 
          49        0        6    0.918    5.735    1.412 
                                                  
    Value          2     3     4     5     6     7
    Frequency      2     3     1    11    15    17
    Proportion 0.041 0.061 0.020 0.224 0.306 0.347
    ---------------------------------------------------------------------------
    dgs03_093 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.947    5.122    1.721 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      1     4     3     4    15    12    10
    Proportion 0.020 0.082 0.061 0.082 0.306 0.245 0.204
    ---------------------------------------------------------------------------
    dgs04_094 
           n  missing distinct     Info     Mean      Gmd 
          49        0        5    0.905    5.857    1.294 
                                            
    Value          3     4     5     6     7
    Frequency      4     3     7    17    18
    Proportion 0.082 0.061 0.143 0.347 0.367
    ---------------------------------------------------------------------------
    dgs05_095 
           n  missing distinct     Info     Mean      Gmd 
          49        0        5    0.831    1.837    1.145 
                                            
    Value          1     2     3     4     5
    Frequency     26    13     5     2     3
    Proportion 0.531 0.265 0.102 0.041 0.061
    ---------------------------------------------------------------------------
    dgs06_096 
           n  missing distinct     Info     Mean      Gmd 
          49        0        6    0.791    1.735    1.043 
                                                  
    Value          1     2     3     4     5     6
    Frequency     27    17     1     1     1     2
    Proportion 0.551 0.347 0.020 0.020 0.020 0.041
    ---------------------------------------------------------------------------
    dgs07_097 
           n  missing distinct     Info     Mean      Gmd 
          48        1        6    0.915    2.354    1.641 
                                                  
    Value          1     2     3     4     5     7
    Frequency     19    13     5     4     6     1
    Proportion 0.396 0.271 0.104 0.083 0.125 0.021
    ---------------------------------------------------------------------------
    dgs08_098 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.955    4.735    1.949 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      2     5     7     2    15    10     8
    Proportion 0.041 0.102 0.143 0.041 0.306 0.204 0.163
    ---------------------------------------------------------------------------
    dgs09_099 
           n  missing distinct     Info     Mean      Gmd 
          49        0        6     0.96    3.367    1.801 
                                                  
    Value          1     2     3     4     5     6
    Frequency      7    11     7     8    13     3
    Proportion 0.143 0.224 0.143 0.163 0.265 0.061
    ---------------------------------------------------------------------------
    dgs10_100 
           n  missing distinct     Info     Mean      Gmd 
          49        0        6    0.911    5.857    1.279 
                                                  
    Value          2     3     4     5     6     7
    Frequency      1     1     5     8    16    18
    Proportion 0.020 0.020 0.102 0.163 0.327 0.367
    ---------------------------------------------------------------------------
    s_pressure 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       27    0.997    28.57    8.391     18.4     19.0 
         .25      .50      .75      .90      .95 
        23.0     29.0     34.0     37.2     39.6 
    
    lowest : 13 17 18 19 20, highest: 38 39 40 41 43
    ---------------------------------------------------------------------------
    press1_101 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7     0.85    1.959    1.299 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency     25    11     9     1     1     1     1
    Proportion 0.510 0.224 0.184 0.020 0.020 0.020 0.020
    ---------------------------------------------------------------------------
    press2_102 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.962    4.714    2.043 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      1     9     4     4    10    13     8
    Proportion 0.020 0.184 0.082 0.082 0.204 0.265 0.163
    ---------------------------------------------------------------------------
    press3_103 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7     0.97    4.449    1.978 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      2     7     5     9    12     7     7
    Proportion 0.041 0.143 0.102 0.184 0.245 0.143 0.143
    ---------------------------------------------------------------------------
    press4_104 
           n  missing distinct     Info     Mean      Gmd 
          49        0        6    0.955    5.082    1.592 
                                                  
    Value          2     3     4     5     6     7
    Frequency      2     6     8    11    14     8
    Proportion 0.041 0.122 0.163 0.224 0.286 0.163
    ---------------------------------------------------------------------------
    press5_105 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.953    4.531    1.964 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      2     6     9     1    15    10     6
    Proportion 0.041 0.122 0.184 0.020 0.306 0.204 0.122
    ---------------------------------------------------------------------------
    press6_106 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.964    3.327    2.243 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency     11    12     6     2     8     8     2
    Proportion 0.224 0.245 0.122 0.041 0.163 0.163 0.041
    ---------------------------------------------------------------------------
    press7_107 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.954    4.347    1.896 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      1    10     5     5    15     9     4
    Proportion 0.020 0.204 0.102 0.102 0.306 0.184 0.082
    ---------------------------------------------------------------------------
    s_ambition 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       16    0.992    21.37    5.432       15       15 
         .25      .50      .75      .90      .95 
          17       22       26       27       28 
                                                                          
    Value         11    13    15    16    17    18    19    20    21    22
    Frequency      1     1     4     6     3     1     2     2     3     2
    Proportion 0.020 0.020 0.082 0.122 0.061 0.020 0.041 0.041 0.061 0.041
                                                  
    Value         23    24    25    26    27    28
    Frequency      2     6     3     7     2     4
    Proportion 0.041 0.122 0.061 0.143 0.041 0.082
    ---------------------------------------------------------------------------
    amb1_108 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.948    4.816    2.109 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      6     3     3     2    11    16     8
    Proportion 0.122 0.061 0.061 0.041 0.224 0.327 0.163
    ---------------------------------------------------------------------------
    amb2_109 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.948    5.367    1.651 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      1     1     5     5    10    14    13
    Proportion 0.020 0.020 0.102 0.102 0.204 0.286 0.265
    ---------------------------------------------------------------------------
    amb3_110 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.948    5.306    1.764 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      2     2     3     5    10    14    13
    Proportion 0.041 0.041 0.061 0.102 0.204 0.286 0.265
    ---------------------------------------------------------------------------
    amb4_111 
           n  missing distinct     Info     Mean      Gmd 
          49        0        5    0.907    5.878     1.15 
                                            
    Value          3     4     5     6     7
    Frequency      2     3    10    18    16
    Proportion 0.041 0.061 0.204 0.367 0.327
    ---------------------------------------------------------------------------
    s_happy 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       17    0.995     18.9    6.398      9.4     10.8 
         .25      .50      .75      .90      .95 
        15.0     20.0     23.0     26.0     26.6 
                                                                          
    Value          9    10    11    12    15    16    17    18    19    20
    Frequency      3     2     2     4     2     1     5     2     3     3
    Proportion 0.061 0.041 0.041 0.082 0.041 0.020 0.102 0.041 0.061 0.061
                                                        
    Value         21    22    23    25    26    27    28
    Frequency      4     4     4     3     4     2     1
    Proportion 0.082 0.082 0.082 0.061 0.082 0.041 0.020
    ---------------------------------------------------------------------------
    hap1_112 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.938    5.082    1.565 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      1     2     5     4    16    14     7
    Proportion 0.020 0.041 0.102 0.082 0.327 0.286 0.143
    ---------------------------------------------------------------------------
    hap2_113 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.944    4.612    1.641 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      1     5     6     6    15    14     2
    Proportion 0.020 0.102 0.122 0.122 0.306 0.286 0.041
    ---------------------------------------------------------------------------
    hap3_114 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7     0.97    4.429    1.893 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      2     5     7    11    10     8     6
    Proportion 0.041 0.102 0.143 0.224 0.204 0.163 0.122
    ---------------------------------------------------------------------------
    hap4_115 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.958    3.224    2.073 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      9    15     5     5     8     5     2
    Proportion 0.184 0.306 0.102 0.102 0.163 0.102 0.041
    ---------------------------------------------------------------------------
    s_selfeff 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       12    0.986       23     3.18     19.0     20.0 
         .25      .50      .75      .90      .95 
        21.0     23.0     25.0     26.0     26.6 
                                                                          
    Value         14    16    19    20    21    22    23    24    25    26
    Frequency      1     1     2     4     7     5     6     6     7     7
    Proportion 0.020 0.020 0.041 0.082 0.143 0.102 0.122 0.122 0.143 0.143
                          
    Value         27    28
    Frequency      1     2
    Proportion 0.020 0.041
    ---------------------------------------------------------------------------
    self1_116 
           n  missing distinct     Info     Mean      Gmd 
          49        0        5    0.899    5.816     1.08 
                                            
    Value          3     4     5     6     7
    Frequency      2     2    12    20    13
    Proportion 0.041 0.041 0.245 0.408 0.265
    ---------------------------------------------------------------------------
    self2_117 
           n  missing distinct     Info     Mean      Gmd 
          49        0        5    0.872    5.714    1.002 
                                            
    Value          3     4     5     6     7
    Frequency      2     2    13    23     9
    Proportion 0.041 0.041 0.265 0.469 0.184
    ---------------------------------------------------------------------------
    self3_118 
           n  missing distinct     Info     Mean      Gmd 
          49        0        5    0.886    5.714    1.061 
                                            
    Value          3     4     5     6     7
    Frequency      2     3    12    22    10
    Proportion 0.041 0.061 0.245 0.449 0.204
    ---------------------------------------------------------------------------
    self4_119 
           n  missing distinct     Info     Mean      Gmd 
          49        0        5     0.85    5.755   0.9745 
                                            
    Value          2     4     5     6     7
    Frequency      1     3    11    25     9
    Proportion 0.020 0.061 0.224 0.510 0.184
    ---------------------------------------------------------------------------
    s_ess 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          48        1       16    0.991     7.25    4.585     1.00     2.00 
         .25      .50      .75      .90      .95 
        4.75     7.50     9.00    12.00    13.65 
                                                                          
    Value          0     1     2     3     4     5     6     7     8     9
    Frequency      1     3     3     2     3     5     2     5     8     5
    Proportion 0.021 0.062 0.062 0.042 0.062 0.104 0.042 0.104 0.167 0.104
                                                  
    Value         10    11    12    13    14    23
    Frequency      3     2     2     1     2     1
    Proportion 0.062 0.042 0.042 0.021 0.042 0.021
    ---------------------------------------------------------------------------
    ess1_120 
           n  missing distinct     Info     Mean      Gmd 
          49        0        4    0.892    1.163   0.9796 
                                      
    Value          0     1     2     3
    Frequency     12    21    12     4
    Proportion 0.245 0.429 0.245 0.082
    ---------------------------------------------------------------------------
    ess2_121 
           n  missing distinct     Info     Mean      Gmd 
          49        0        4    0.902    1.204    1.027 
                                      
    Value          0     1     2     3
    Frequency     12    20    12     5
    Proportion 0.245 0.408 0.245 0.102
    ---------------------------------------------------------------------------
    ess3_122 
           n  missing distinct     Info     Mean      Gmd 
          49        0        4     0.71   0.5306   0.7993 
                                      
    Value          0     1     2     3
    Frequency     32    11     3     3
    Proportion 0.653 0.224 0.061 0.061
    ---------------------------------------------------------------------------
    ess4_123 
           n  missing distinct     Info     Mean      Gmd 
          49        0        4    0.914    1.306     1.07 
                                      
    Value          0     1     2     3
    Frequency     12    15    17     5
    Proportion 0.245 0.306 0.347 0.102
    ---------------------------------------------------------------------------
    ess5_124 
           n  missing distinct     Info     Mean      Gmd 
          48        1        4    0.898    2.021    1.047 
                                      
    Value          0     1     2     3
    Frequency      3    12    14    19
    Proportion 0.062 0.250 0.292 0.396
    ---------------------------------------------------------------------------
    ess6_125 
           n  missing distinct     Info     Mean      Gmd 
          49        0        2     0.06  0.04082  0.08163 
                        
    Value         0    2
    Frequency    48    1
    Proportion 0.98 0.02
    ---------------------------------------------------------------------------
    ess7_126 
           n  missing distinct     Info     Mean      Gmd 
          49        0        4    0.852   0.7551   0.8946 
                                      
    Value          0     1     2     3
    Frequency     23    17     7     2
    Proportion 0.469 0.347 0.143 0.041
    ---------------------------------------------------------------------------
    ess8_127 
           n  missing distinct     Info     Mean      Gmd 
          49        0        4    0.324   0.1837   0.3401 
                                      
    Value          0     1     2     3
    Frequency     43     4     1     1
    Proportion 0.878 0.082 0.020 0.020
    ---------------------------------------------------------------------------
    s_rea 
           n  missing distinct     Info     Mean      Gmd 
          48        1        8    0.969    2.354    2.125 
                                                              
    Value          0     1     2     3     4     5     6     7
    Frequency      8    11    10     6     7     2     2     2
    Proportion 0.167 0.229 0.208 0.125 0.146 0.042 0.042 0.042
    ---------------------------------------------------------------------------
    rea01_128 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                23                9               17
    Proportion            0.469            0.184            0.347
    ---------------------------------------------------------------------------
    rea02_129 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                30                8               11
    Proportion            0.612            0.163            0.224
    ---------------------------------------------------------------------------
    rea03_130 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                21               19                9
    Proportion            0.429            0.388            0.184
    ---------------------------------------------------------------------------
    rea04_131 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                18               15               16
    Proportion            0.367            0.306            0.327
    ---------------------------------------------------------------------------
    rea05_132 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                18               26                5
    Proportion            0.367            0.531            0.102
    ---------------------------------------------------------------------------
    rea06_133 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                20               16               13
    Proportion            0.408            0.327            0.265
    ---------------------------------------------------------------------------
    rea07_134 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                22               16               11
    Proportion            0.449            0.327            0.224
    ---------------------------------------------------------------------------
    rea08_135 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                30               14                5
    Proportion            0.612            0.286            0.102
    ---------------------------------------------------------------------------
    rea09_136 
           n  missing distinct 
          48        1        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                28               17                3
    Proportion            0.583            0.354            0.062
    ---------------------------------------------------------------------------
    rea10_137 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                23               21                5
    Proportion            0.469            0.429            0.102
    ---------------------------------------------------------------------------
    rea11_138 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                22               12               15
    Proportion            0.449            0.245            0.306
    ---------------------------------------------------------------------------
    rea12_139 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                31               16                2
    Proportion            0.633            0.327            0.041
    ---------------------------------------------------------------------------
    rea13_140 
           n  missing distinct 
          49        0        3 
                                                                 
    Value       Rarely or Never        Sometimes Usually or Often
    Frequency                39                8                2
    Proportion            0.796            0.163            0.041
    ---------------------------------------------------------------------------
    tipi_extra 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       13    0.991    3.561    1.934      1.2      1.5 
         .25      .50      .75      .90      .95 
         2.5      3.5      4.5      6.0      6.5 
                                                                          
    Value        1.0   1.5   2.0   2.5   3.0   3.5   4.0   4.5   5.0   5.5
    Frequency      3     6     3     5     7     4     4     6     1     3
    Proportion 0.061 0.122 0.061 0.102 0.143 0.082 0.082 0.122 0.020 0.061
                                
    Value        6.0   6.5   7.0
    Frequency      3     3     1
    Proportion 0.061 0.061 0.020
    ---------------------------------------------------------------------------
    tipi_agree 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       11    0.984    4.888    1.707      2.0      2.0 
         .25      .50      .75      .90      .95 
         4.0      5.0      6.0      6.5      6.8 
                                                                          
    Value        1.5   2.0   3.0   3.5   4.0   4.5   5.0   5.5   6.0   6.5
    Frequency      2     4     1     1     6     7     6     6     4     9
    Proportion 0.041 0.082 0.020 0.020 0.122 0.143 0.122 0.122 0.082 0.184
                    
    Value        7.0
    Frequency      3
    Proportion 0.061
    ---------------------------------------------------------------------------
    tipi_cons 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       10    0.974    5.276    1.584      3.0      3.0 
         .25      .50      .75      .90      .95 
         4.5      5.5      6.5      6.6      7.0 
                                                                          
    Value        2.5   3.0   3.5   4.0   4.5   5.0   5.5   6.0   6.5   7.0
    Frequency      2     6     1     3     5     3     6     5    13     5
    Proportion 0.041 0.122 0.020 0.061 0.102 0.061 0.122 0.102 0.265 0.102
    ---------------------------------------------------------------------------
    tipi_stab 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       12     0.97    4.418    1.498      2.5      2.5 
         .25      .50      .75      .90      .95 
         3.5      4.5      5.5      6.1      6.5 
                                                                          
    Value        1.5   2.0   2.5   3.0   3.5   4.0   4.5   5.0   5.5   6.0
    Frequency      1     1     4     5     2     7    14     1     4     5
    Proportion 0.020 0.020 0.082 0.102 0.041 0.143 0.286 0.020 0.082 0.102
                          
    Value        6.5   7.0
    Frequency      4     1
    Proportion 0.082 0.020
    ---------------------------------------------------------------------------
    tipi_open 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          49        0       10    0.986    5.194    1.446      3.2      3.5 
         .25      .50      .75      .90      .95 
         4.5      5.5      6.0      7.0      7.0 
                                                                          
    Value        1.5   3.0   3.5   4.0   4.5   5.0   5.5   6.0   6.5   7.0
    Frequency      1     2     3     6     6     6     7     6     6     6
    Proportion 0.020 0.041 0.061 0.122 0.122 0.122 0.143 0.122 0.122 0.122
    ---------------------------------------------------------------------------
    tipi01_141 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.969    3.816    2.226 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      7     6    13     4     6     8     5
    Proportion 0.143 0.122 0.265 0.082 0.122 0.163 0.102
    ---------------------------------------------------------------------------
    tipi02_142 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.969    3.694    2.146 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      7    11     4     7    11     6     3
    Proportion 0.143 0.224 0.082 0.143 0.224 0.122 0.061
    ---------------------------------------------------------------------------
    tipi03_143 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.943    5.449    1.667 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      1     2     3     5    10    13    15
    Proportion 0.020 0.041 0.061 0.102 0.204 0.265 0.306
    ---------------------------------------------------------------------------
    tipi04_144 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.952        4    1.849 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      3     9     7     6    16     6     2
    Proportion 0.061 0.184 0.143 0.122 0.327 0.122 0.041
    ---------------------------------------------------------------------------
    tipi05_145 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.944    5.429    1.638 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      1     1     4     5    12    11    15
    Proportion 0.020 0.020 0.082 0.102 0.245 0.224 0.306
    ---------------------------------------------------------------------------
    tipi06_146 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.961    4.694    1.913 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      2     4     8     5    10    14     6
    Proportion 0.041 0.082 0.163 0.102 0.204 0.286 0.122
    ---------------------------------------------------------------------------
    tipi07_147 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.926    5.469    1.738 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      1     5     1     3     7    17    15
    Proportion 0.020 0.102 0.020 0.061 0.143 0.347 0.306
    ---------------------------------------------------------------------------
    tipi08_148 
           n  missing distinct     Info     Mean      Gmd 
          49        0        6    0.941    2.898    1.679 
                                                  
    Value          1     2     3     4     5     6
    Frequency      8    17    10     3     8     3
    Proportion 0.163 0.347 0.204 0.061 0.163 0.061
    ---------------------------------------------------------------------------
    tipi09_149 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.953    4.837    1.777 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      1     5     5     5    13    14     6
    Proportion 0.020 0.102 0.102 0.102 0.265 0.286 0.122
    ---------------------------------------------------------------------------
    tipi10_150 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.959    3.041    1.813 
                                                        
    Value          1     2     3     4     5     6     7
    Frequency      9    14     7     8     8     2     1
    Proportion 0.184 0.286 0.143 0.163 0.163 0.041 0.020
    ---------------------------------------------------------------------------
    cards_151 
           n  missing distinct     Info     Mean      Gmd 
          49        0        7    0.938    2.265    2.316 
                                                        
    Value          0     1     2     3     8    10    12
    Frequency      7    16    13     9     1     1     2
    Proportion 0.143 0.327 0.265 0.184 0.020 0.020 0.041
    ---------------------------------------------------------------------------
    cwrusom_152 
           n  missing distinct 
          49        0        2 
                          
    Value         No   Yes
    Frequency     14    35
    Proportion 0.286 0.714
    ---------------------------------------------------------------------------
    edmom_153 
           n  missing distinct 
          49        0        5 
    
    Four-year college degree (14, 0.286), High school graduate (5, 0.102),
    Less than high school graduate (3, 0.061), Post-bachelor's degree (17,
    0.347), Some college (10, 0.204)
    ---------------------------------------------------------------------------
    eddad_154 
           n  missing distinct 
          49        0        5 
    
    Four-year college degree (13, 0.265), High school graduate (5, 0.102),
    Less than high school graduate (3, 0.061), Post-bachelor's degree (18,
    0.367), Some college (10, 0.204)
    ---------------------------------------------------------------------------
    ecigok_156 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
          48        1       34    0.999    47.81    33.85     0.70     3.00 
         .25      .50      .75      .90      .95 
       22.75    51.00    68.25    86.00    87.00 
    
    lowest :  0  2  3  5  8, highest: 78 85 86 87 98
    ---------------------------------------------------------------------------

If you have any questions, please direct them to `431-help`.
