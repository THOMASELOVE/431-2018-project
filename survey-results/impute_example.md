Imputation Example for Course Project
================
Thomas E. Love
2018-11-04

# Setup and Data Load

``` r
library(Epi)
library(simputation)
library(tidyverse)
```

Next, we’ll import the data using `read_csv` and then change the
character variables (other than `subject`) into factors.

``` r
dat <- read_csv("impute_example.csv") %>%
    mutate_if(is.character, funs(as.factor(.))) %>%
    mutate(subject = as.character(subject))

dat
```

    # A tibble: 45 x 4
       subject treatment ses    satisfaction
       <chr>   <fct>     <fct>         <int>
     1 A01     A         Middle           61
     2 A02     C         Middle           47
     3 A03     B         High             53
     4 A04     B         Middle           42
     5 A05     B         Low              55
     6 A06     B         Middle           67
     7 A07     A         High             55
     8 A08     A         Middle           53
     9 A09     C         High             43
    10 A10     A         Low              25
    # ... with 35 more rows

# How Much Missing Data is there in this `dat` data?

``` r
dat %>% summarize_all(funs(sum(is.na(.))))
```

    # A tibble: 1 x 4
      subject treatment   ses satisfaction
        <int>     <int> <int>        <int>
    1       0         0     1            1

Let’s see those subjects with missing values…

``` r
dat %>% filter(!complete.cases(.))
```

    # A tibble: 2 x 4
      subject treatment ses    satisfaction
      <chr>   <fct>     <fct>         <int>
    1 A12     A         <NA>             25
    2 A29     B         Middle           NA

# Imputing missing data in a categorical variable

Here, we’ll demonstrate the imputation of a categorical variable value
(`ses` for subject `A12`) and then perform a 2x2 table analysis with the
imputed value included, and then again without it, to see the impact.

There are several methods available for single imputation of a
categorical variable in the `simputation` package. For the project, we
will assume the data are missing at random, though perhaps not missing
completely at random. What this means here is that we have at least
three choices:

**PLEASE Save your work prior to trying any of these approaches\!**

1.  Use the variables available in your data set which have complete
    data for the people with missing data on `ses` to help predict the
    `ses` level, using a decision tree approach.

<!-- end list -->

``` r
set.seed(431)
dat_imp1 <- impute_cart(dat, ses ~ treatment + satisfaction)

dat_imp1 %>% filter(subject == "A12")
```

    # A tibble: 1 x 4
      subject treatment ses   satisfaction
      <chr>   <fct>     <fct>        <int>
    1 A12     A         Low             25

You may find this crashes your R session, or takes a long time. If so,
you might want to try method 2 instead.

2.  Use the variables available in your data set which have complete
    data for the people with missing data on `ses` to help predict the
    `ses` level, using a random hot deck approach. Here, you may need to
    convince the machine that your tibble is also a data frame, with…

<!-- end list -->

``` r
set.seed(431)
dat_imp2 <- impute_rhd(data.frame(dat), ses ~ treatment)

dat_imp2 %>% filter(subject == "A12")
```

``` 
  subject treatment ses satisfaction
1     A12         A Low           25
```

3.  If you cannot make approach 1 or 2 work, then impute the modal (most
    common) value from the existing observations in the categorical
    data. First, let’s determine the mode of `ses`:

<!-- end list -->

``` r
dat %>% count(ses)
```

    # A tibble: 4 x 2
      ses        n
      <fct>  <int>
    1 High      15
    2 Low       15
    3 Middle    14
    4 <NA>       1

Since we have the same number of High and Low values, let’s arbitrarily
pick one to serve as the mode here. We’ll pick “High”.

``` r
dat_imp3 <- dat %>% replace_na(list(ses = "High"))

dat_imp3 %>% filter(subject == "A12")
```

    # A tibble: 1 x 4
      subject treatment ses   satisfaction
      <chr>   <fct>     <fct>        <int>
    1 A12     A         High            25

## The Imputation’s Impact on a 2x2 Table

### With the imputed `ses` data

Let’s use the results from our first imputation here.

``` r
Epi::twoby2(dat_imp1$treatment, dat_imp1$ses)
```

    2 by 2 table analysis: 
    ------------------------------------------------------ 
    Outcome   : High 
    Comparing : A vs. B 
    
      High Low    P(High) 95% conf. interval
    A    6   5     0.5455    0.2681   0.7972
    B    5   4     0.5556    0.2513   0.8232
    
                                        95% conf. interval
                 Relative Risk:  0.9818    0.4432   2.1748
             Sample Odds Ratio:  0.9600    0.1633   5.6429
    Conditional MLE Odds Ratio:  0.9619    0.1161   7.7653
        Probability difference: -0.0101   -0.3786   0.3667
    
                 Exact P-value: 1 
            Asymptotic P-value: 0.964 
    ------------------------------------------------------

### Without the imputed `ses` data

Compare the results above to what we see when we look at the original,
unimputed data.

``` r
Epi::twoby2(dat$treatment, dat$ses)
```

    2 by 2 table analysis: 
    ------------------------------------------------------ 
    Outcome   : High 
    Comparing : A vs. B 
    
      High Low    P(High) 95% conf. interval
    A    6   4     0.6000    0.2974   0.8417
    B    5   4     0.5556    0.2513   0.8232
    
                                       95% conf. interval
                 Relative Risk: 1.0800    0.4985   2.3396
             Sample Odds Ratio: 1.2000    0.1935   7.4406
    Conditional MLE Odds Ratio: 1.1885    0.1365  10.5424
        Probability difference: 0.0444   -0.3402   0.4149
    
                 Exact P-value: 1 
            Asymptotic P-value: 0.8447 
    ------------------------------------------------------

# Imputing missing data in a quantitative variable

Here, we’ll demonstrate the imputation of a quantitative variable value
(`satisfaction` for subject `A29`) and then perform a two-sample
comparison of `satisfaction` by `treatment` level, with the imputed
value included, and then again without it, to see the impact.

There are even more methods available for single imputation of a
quantitative variable in the `simputation` package. For the project, we
will assume the data are missing at random, though perhaps not missing
completely at random. What this means here is that we have at least
three choices:

**PLEASE Save your work prior to trying any of these approaches\!**

1.  Use the variables available in your data set which have complete
    data for the people with missing data on `satisfaction` to help
    predict the `satisfaction` level, using a predictive mean matching
    approach. Here, you may need to convince the machine that your
    tibble is also a data frame, with…

<!-- end list -->

``` r
set.seed(431)
dat_imp4 <- impute_pmm(dat, satisfaction ~ treatment + ses)

dat_imp4 %>% filter(subject == "A29")
```

    # A tibble: 1 x 4
      subject treatment ses    satisfaction
      <chr>   <fct>     <fct>         <int>
    1 A29     B         Middle           61

This “predictive mean matching” approach can only be used with numeric
data. It will first use a model to impute a value, and then replace that
prediction with the observed value nearest to the prediction. That means
that the new value must be the same as one of the existing
`satisfaction` values.

2.  Use the variables available in your data set which have complete
    data for the people with missing data on `satisfaction` to help
    predict the `satisfaction` level, using a robust linear model
    approach.

<!-- end list -->

``` r
set.seed(431)
dat_imp5 <- impute_rlm(dat, satisfaction ~ treatment + ses)

dat_imp5 %>% filter(subject == "A29")
```

    # A tibble: 1 x 4
      subject treatment ses    satisfaction
      <chr>   <fct>     <fct>         <dbl>
    1 A29     B         Middle         59.2

This may, as in this case, produce a result that doesn’t happen in
practice (the rest of the `satisfaction` values are integers.)

3.  If you cannot make approach 1 or 2 work, then impute the median
    value from the existing observations in the quantitative data.

<!-- end list -->

``` r
dat_imp6 <- impute_median(dat, satisfaction ~ 1)

dat_imp6 %>% filter(subject == "A29")
```

    # A tibble: 1 x 4
      subject treatment ses    satisfaction
      <chr>   <fct>     <fct>         <dbl>
    1 A29     B         Middle         52.5

``` r
dat %>% filter(complete.cases(satisfaction)) %>% summarize(median(satisfaction))
```

    # A tibble: 1 x 1
      `median(satisfaction)`
                       <dbl>
    1                   52.5

If we wanted instead to group the data by `ses` level before imputing
`satisfaction`, that would be…

``` r
dat_imp7 <- impute_median(dat, satisfaction ~ ses)

dat_imp7 %>% filter(subject == "A29")
```

    # A tibble: 1 x 4
      subject treatment ses    satisfaction
      <chr>   <fct>     <fct>         <dbl>
    1 A29     B         Middle           51

## The Imputation’s Impact on an Independent Samples Comparison

### With the imputed `satisfaction` data

Let’s use the results from our first imputation here, in `dat_imp4`.

``` r
anova(lm(satisfaction ~ treatment, data = dat_imp4))
```

    Analysis of Variance Table
    
    Response: satisfaction
              Df Sum Sq Mean Sq F value  Pr(>F)   
    treatment  2 1928.6  964.29  6.3856 0.00379 **
    Residuals 42 6342.4  151.01                   
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

### Without the imputed `satisfaction` data

Compare the results above to what we see when we look at the original,
unimputed data.

``` r
anova(lm(satisfaction ~ treatment, data = dat))
```

    Analysis of Variance Table
    
    Response: satisfaction
              Df Sum Sq Mean Sq F value   Pr(>F)   
    treatment  2 1854.7  927.35  5.9961 0.005197 **
    Residuals 41 6341.0  154.66                    
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

# Imputing Both the Categorical and Quantitative Variables

I suggest the following approach. First impute one variable, then the
other. Chain them together with the pipe (`%>%`) like this:

``` r
dat_imp8 <- dat %>%
    impute_pmm(satisfaction ~ ses + treatment) %>%
    impute_cart(ses ~ satisfaction)

dat_imp8 %>%
    filter(subject %in% c("A12", "A29"))
```

    # A tibble: 2 x 4
      subject treatment ses    satisfaction
      <chr>   <fct>     <fct>         <int>
    1 A12     A         Low              25
    2 A29     B         Middle           61

Good luck\!
