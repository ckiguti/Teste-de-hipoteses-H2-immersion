ANCOVA test for `fss`\~`dfs`+`scenario`\*`immersion`
================
Geiser C. Challco <geiser@alumni.usp.br>

-   [Initial Variables and Data](#initial-variables-and-data)
    -   [Descriptive statistics of initial
        data](#descriptive-statistics-of-initial-data)
-   [Checking of Assumptions](#checking-of-assumptions)
    -   [Assumption: Symmetry and treatment of
        outliers](#assumption-symmetry-and-treatment-of-outliers)
    -   [Assumption: Normality distribution of
        data](#assumption-normality-distribution-of-data)
    -   [Assumption: Linearity of dependent variables and covariate
        variable](#assumption-linearity-of-dependent-variables-and-covariate-variable)
    -   [Assumption: Homogeneity of data
        distribution](#assumption-homogeneity-of-data-distribution)
-   [Saving the Data with Normal Distribution Used for Performing ANCOVA
    test](#saving-the-data-with-normal-distribution-used-for-performing-ancova-test)
-   [Computation of ANCOVA test and Pairwise
    Comparison](#computation-of-ancova-test-and-pairwise-comparison)
    -   [ANCOVA test](#ancova-test)
    -   [Pairwise comparison](#pairwise-comparison)
    -   [Descriptive Statistic of Estimated Marginal
        Means](#descriptive-statistic-of-estimated-marginal-means)
    -   [Ancova plots for the dependent variable
        “fss”](#ancova-plots-for-the-dependent-variable-fss)
    -   [Textual Report](#textual-report)
-   [Tips and References](#tips-and-references)

## Initial Variables and Data

-   R-script file: [../code/ancova.R](../code/ancova.R)
-   Initial table file:
    [../data/initial-table.csv](../data/initial-table.csv)
-   Data for fss [../data/table-for-fss.csv](../data/table-for-fss.csv)
-   Table without outliers and normal distribution of data:
    [../data/table-with-normal-distribution.csv](../data/table-with-normal-distribution.csv)
-   Other data files: [../data/](../data/)
-   Files related to the presented results: [../results/](../results/)

### Descriptive statistics of initial data

| scenario     | immersion | variable |   n |  mean | median |   min |   max |    sd |    se |    ci |   iqr | symmetry | skewness | kurtosis |
|:-------------|:----------|:---------|----:|------:|-------:|------:|------:|------:|------:|------:|------:|:---------|---------:|---------:|
| gamified     | lower     | fss      |   8 | 3.486 |  3.389 | 2.778 | 4.444 | 0.582 | 0.206 | 0.486 | 0.861 | YES      |    0.290 |   -1.539 |
| gamified     | upper     | fss      |   8 | 3.569 |  3.222 | 2.667 | 4.556 | 0.845 | 0.299 | 0.706 | 1.667 | YES      |    0.274 |   -2.011 |
| non.gamified | lower     | fss      |   7 | 3.635 |  3.556 | 3.222 | 4.556 | 0.487 | 0.184 | 0.451 | 0.500 | NO       |    0.815 |   -0.968 |
| non.gamified | upper     | fss      |   5 | 3.244 |  3.444 | 2.556 | 3.889 | 0.600 | 0.269 | 0.746 | 1.000 | YES      |   -0.156 |   -2.165 |
| NA           | NA        | fss      |  28 | 3.504 |  3.444 | 2.556 | 4.556 | 0.630 | 0.119 | 0.244 | 0.944 | YES      |    0.356 |   -1.090 |

![](ancova_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

    ## [1] "P32"

## Checking of Assumptions

### Assumption: Symmetry and treatment of outliers

#### Applying transformation for skewness data when normality is not achieved

#### Dealing with outliers (performing treatment of outliers)

### Assumption: Normality distribution of data

#### Removing data that affect normality (extreme values)

``` r
non.normal <- list(
"fss" = c("P03","P11","P15")
)
sdat <- removeFromDataTable(rdat, non.normal, wid)
```

#### Result of normality test in the residual model

|     | var |   n | skewness | kurtosis | symmetry | statistic | method       |     p | p.signif | normality |
|:----|:----|----:|---------:|---------:|:---------|----------:|:-------------|------:|:---------|:----------|
| fss | fss |  25 |    0.287 |   -1.351 | YES      |     0.922 | Shapiro-Wilk | 0.058 | ns       | YES       |

#### Result of normality test in each group

This is an optional validation and only valid for groups with number
greater than 30 observations

| scenario     | immersion | variable |   n |  mean | median |   min |   max |    sd |    se |    ci |   iqr | normality | method       | statistic |     p | p.signif |
|:-------------|:----------|:---------|----:|------:|-------:|------:|------:|------:|------:|------:|------:|:----------|:-------------|----------:|------:|:---------|
| gamified     | lower     | fss      |   8 | 3.486 |  3.389 | 2.778 | 4.444 | 0.582 | 0.206 | 0.486 | 0.861 | YES       | Shapiro-Wilk |     0.953 | 0.740 | ns       |
| gamified     | upper     | fss      |   5 | 3.667 |  3.444 | 2.889 | 4.556 | 0.843 | 0.377 | 1.046 | 1.667 | NO        | NA           |        NA | 1.000 | NA       |
| non.gamified | lower     | fss      |   7 | 3.635 |  3.556 | 3.222 | 4.556 | 0.487 | 0.184 | 0.451 | 0.500 | YES       | Shapiro-Wilk |     0.846 | 0.112 | ns       |
| non.gamified | upper     | fss      |   5 | 3.244 |  3.444 | 2.556 | 3.889 | 0.600 | 0.269 | 0.746 | 1.000 | YES       | Shapiro-Wilk |     0.882 | 0.317 | ns       |

**Observation**:

As sample sizes increase, parametric tests remain valid even with the
violation of normality \[[1](#references)\]. According to the central
limit theorem, the sampling distribution tends to be normal if the
sample is large, more than (`n > 30`) observations. Therefore, we
performed parametric tests with large samples as described as follows:

-   In cases with the sample size greater than 100 (`n > 100`), we
    adopted a significance level of `p < 0.01`

-   For samples with `n > 50` observation, we adopted D’Agostino-Pearson
    test that offers better accuracy for larger samples
    \[[2](#references)\].

-   For samples’ size between `n > 100` and `n <= 200`, we ignored the
    normality test, and our decision of validating normality was based
    only in the interpretation of QQ-plots and histograms because the
    Shapiro-Wilk and D’Agostino-Pearson tests tend to be too sensitive
    with values greater than 200 observation \[[3](#references)\].

-   For samples with `n > 200` observation, we ignore the normality
    assumption based on the central theorem limit.

### Assumption: Linearity of dependent variables and covariate variable

``` r
ggscatter(sdat[["fss"]], x=covar, y="fss", facet.by=between, short.panel.labs = F) + 
 stat_smooth(method = "lm", span = 0.9)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](ancova_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

### Assumption: Homogeneity of data distribution

|       | var | method         | formula                         |   n | DFn.df1 | DFd.df2 | statistic |     p | p.signif |
|:------|:----|:---------------|:--------------------------------|----:|--------:|--------:|----------:|------:|:---------|
| fss.1 | fss | Levene’s test  | `.res`\~`scenario`\*`immersion` |  25 |       3 |      21 |     0.792 | 0.512 | ns       |
| fss.2 | fss | Anova’s slopes | `.res`\~`scenario`\*`immersion` |  25 |       3 |      17 |     2.812 | 0.071 | ns       |

## Saving the Data with Normal Distribution Used for Performing ANCOVA test

``` r
ndat <- sdat[[1]]
for (dv in names(sdat)[-1]) ndat <- merge(ndat, sdat[[dv]])
write.csv(ndat, paste0("../data/table-with-normal-distribution.csv"))
```

Descriptive statistics of data with normal distribution

|       | scenario     | immersion | variable |   n |  mean | median |   min |   max |    sd |    se |    ci |   iqr |
|:------|:-------------|:----------|:---------|----:|------:|-------:|------:|------:|------:|------:|------:|------:|
| fss.1 | gamified     | lower     | fss      |   8 | 3.486 |  3.389 | 2.778 | 4.444 | 0.582 | 0.206 | 0.486 | 0.861 |
| fss.2 | gamified     | upper     | fss      |   5 | 3.667 |  3.444 | 2.889 | 4.556 | 0.843 | 0.377 | 1.046 | 1.667 |
| fss.3 | non.gamified | lower     | fss      |   7 | 3.635 |  3.556 | 3.222 | 4.556 | 0.487 | 0.184 | 0.451 | 0.500 |
| fss.4 | non.gamified | upper     | fss      |   5 | 3.244 |  3.444 | 2.556 | 3.889 | 0.600 | 0.269 | 0.746 | 1.000 |

![](ancova_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

## Computation of ANCOVA test and Pairwise Comparison

### ANCOVA test

| var | Effect             | DFn | DFd |   SSn |   SSd |     F |     p |   ges | p.signif |
|:----|:-------------------|----:|----:|------:|------:|------:|------:|------:|:---------|
| fss | dfs                |   1 |  20 | 0.000 | 8.075 | 0.000 | 1.000 | 0.000 | ns       |
| fss | scenario           |   1 |  20 | 0.097 | 8.075 | 0.239 | 0.630 | 0.012 | ns       |
| fss | immersion          |   1 |  20 | 0.058 | 8.075 | 0.143 | 0.710 | 0.007 | ns       |
| fss | scenario:immersion |   1 |  20 | 0.290 | 8.075 | 0.719 | 0.407 | 0.035 | ns       |

### Pairwise comparison

| var | scenario     | immersion | group1   | group2       | estimate | conf.low | conf.high |    se | statistic |     p | p.adj | p.adj.signif |
|:----|:-------------|:----------|:---------|:-------------|---------:|---------:|----------:|------:|----------:|------:|------:|:-------------|
| fss | NA           | lower     | gamified | non.gamified |   -0.149 |   -1.027 |     0.729 | 0.421 |    -0.354 | 0.727 | 0.727 | ns           |
| fss | NA           | upper     | gamified | non.gamified |    0.422 |   -0.485 |     1.329 | 0.435 |     0.971 | 0.343 | 0.343 | ns           |
| fss | gamified     | NA        | lower    | upper        |   -0.181 |   -1.054 |     0.693 | 0.419 |    -0.431 | 0.671 | 0.671 | ns           |
| fss | non.gamified | NA        | lower    | upper        |    0.390 |   -0.510 |     1.291 | 0.432 |     0.905 | 0.376 | 0.376 | ns           |

### Descriptive Statistic of Estimated Marginal Means

| var | scenario     | immersion |   n | emmean |  mean | conf.low | conf.high |    sd | sd.emms | se.emms |
|:----|:-------------|:----------|----:|-------:|------:|---------:|----------:|------:|--------:|--------:|
| fss | gamified     | lower     |   8 |  3.486 | 3.486 |    2.951 |     4.022 | 0.582 |   0.726 |   0.257 |
| fss | gamified     | upper     |   5 |  3.667 | 3.667 |    3.047 |     4.286 | 0.843 |   0.664 |   0.297 |
| fss | non.gamified | lower     |   7 |  3.635 | 3.635 |    3.057 |     4.213 | 0.487 |   0.733 |   0.277 |
| fss | non.gamified | upper     |   5 |  3.244 | 3.244 |    2.628 |     3.861 | 0.600 |   0.660 |   0.295 |

### Ancova plots for the dependent variable “fss”

``` r
plots <- twoWayAncovaPlots(sdat[["fss"]], "fss", between
, aov[["fss"]], pwc[["fss"]], addParam = c("jitter"), font.label.size=16, step.increase=0.25)
```

#### Plot for: `fss` \~ `scenario`

``` r
plots[["scenario"]]
```

![](ancova_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

#### Plot for: `fss` \~ `immersion`

``` r
plots[["immersion"]]
```

![](ancova_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

### Textual Report

After controlling the linearity of covariance “dfs”, ANCOVA tests with
independent between-subjects variables “scenario” (gamified,
non.gamified) and “immersion” (lower, upper) were performed to determine
statistically significant difference on the dependent varibles “fss”.
For the dependent variable “fss”, there was not statistically
significant effects.

## Tips and References

-   Use the site <https://www.tablesgenerator.com> to convert the HTML
    tables into Latex format

-   \[2\]: Miot, H. A. (2017). Assessing normality of data in clinical
    and experimental trials. J Vasc Bras, 16(2), 88-91.

-   \[3\]: Bárány, Imre; Vu, Van (2007). “Central limit theorems for
    Gaussian polytopes”. Annals of Probability. Institute of
    Mathematical Statistics. 35 (4): 1593–1621.
