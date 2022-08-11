# Introduction to the mda.biber R package

## Load the mda.biber package

Load the package, as well as others that we’ll use in this vignette.

``` r
library(mda.biber)
library(corrplot)
library(tidyverse)
library(kableExtra)
```

## Multi-Dimensional Analysis (MDA)

Multi-Dimensional Analysis (MDA) is a [complex statistical
procedure](https://www.uni-bamberg.de/fileadmin/eng-ling/fs/Chapter_21/Index.html?Multidimensionalanalysis.html)
developed by Douglas Biber. It is largely used to describe language as
it varies by [genre, register, and
use](https://www.uni-bamberg.de/fileadmin/eng-ling/fs/Chapter_21/Index.html?21Summary.html).

MDA is based on the fundamental linguistic principle that some
linguistic variables co-occur (nouns and adjectives, for example) while
others inversely co-occur (think nouns and pronouns).

MDA in conducted in the following stages:

1.  Identification of relevant variables
2.  Extraction of factors from variables
3.  Functional interpretation of factors as dimensions
4.  Placement of categories on the dimensions

The initial steps are carried out using well-established procures for
factor analysis.

## Inspect the data

First, we inspect the data that comes with the package – counts of
features tagged using the [psuedobibeR
package](https://github.com/browndw/pseudobibeR).

| doc_id          | f\_01\_past\_tense | f\_02\_perfect\_aspect | f\_03\_present\_tense | f\_04\_place\_adverbials | f\_05\_time\_adverbials |
|-----------------|-----------------|---------------------|--------------------|-----------------------|----------------------|
| BIO.G0.01.1.txt | 17.678709       | 8.070715            | 52.45965           | 3.074558              | 4.035357             |
| BIO.G0.02.1.txt | 11.477762       | 10.043042           | 61.33429           | 2.152080              | 6.097561             |
| BIO.G0.02.2.txt | 3.875969        | 0.000000            | 62.01550           | 0.000000              | 1.291990             |
| BIO.G0.02.3.txt | 1.700680        | 3.401361            | 64.62585           | 0.000000              | 0.000000             |
| BIO.G0.02.4.txt | 1.531394        | 4.594181            | 70.44410           | 1.531394              | 0.000000             |
| BIO.G0.02.5.txt | 14.844804       | 7.759784            | 47.57085           | 3.373819              | 2.024291             |

\[…\]

| f\_62\_split\_infinitve | f\_63\_split\_auxiliary | f\_64\_phrasal\_coordination | f\_65\_clausal\_coordination | f\_66\_neg\_synthetic | f\_67\_neg\_analytic |
|----------------------|----------------------|---------------------------|---------------------------|--------------------|-------------------|
| 0.1921599            | 4.611837             | 8.262875                  | 5.956956                  | 0                  | 4.227517          |
| 0.7173601            | 5.021521             | 6.097561                  | 3.945481                  | 0                  | 2.869441          |
| 0.0000000            | 3.875969             | 6.459948                  | 1.291990                  | 0                  | 3.875969          |
| 0.0000000            | 1.700680             | 18.707483                 | 0.000000                  | 0                  | 0.000000          |
| 3.0627871            | 6.125574             | 7.656968                  | 0.000000                  | 0                  | 7.656968          |
| 0.6747638            | 4.723347             | 3.036437                  | 1.686910                  | 0                  | 5.39811           |

Note that the first column is the name of a document from the [Michigan
Corpus of Upper-Level Student Papers
(MICUSP)](https://elicorpora.info/), which includes various kinds of the
meta-data. The beginning of the file, for example, has a series of
upper-case letters that identify the discipline the paper was written
for (BIO = Biology).

Because MDA involves a specific application of factor analysis, the
first thing we need to do is to extract that string and covert the
column to a factor (or categorical variable).

``` r
d <- micusp_biber %>%
  mutate(doc_id = str_extract(doc_id, "^[A-Z]+")) %>%
  mutate(doc_id = as.factor(doc_id)) %>%
  select(where(~ any(. != 0))) # removes any columns containing all zeros as required for corrplot
```

## Determining the number of factors

The package comes with a wrapper for the `Scree()` function in
**nFactors**. However, you can easily use any scree plotting function
(or alternative method) for determining the number of factors to be
calculated.

``` r
screeplot_mda(d)
```

![](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/mda_introduction_files/figure-gfm/scree_plot-1.png)<!-- -->

## Inspecting a correlation matrix

Next, we can create a correlation matrix and plot it using corrplot.
Note the first line of code that omits any columns that contain only
zeros.

``` r

# create a correlation matrix
cor_m <- cor(d[,-1], method = "pearson")

# plot the matrix
corrplot(cor_m, type = "upper", order = "hclust", tl.col = "black", tl.srt = 45, diag = F, tl.cex = 0.5)
```

![](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/mda_introduction_files/figure-gfm/corr_plot-1.png)<!-- -->

## The **mda_loadings( )** function

As the plot makes clear, there are groupings of features that positively
and negatively correlate, making our data a good candidate for MDA. To
carry out the MDA procedure use the `mda_loadings()` function.

The function requires a data.frame with 1 factor (or categorical
variable) and more than 2 numeric variables.

Ideally, need at least 5 times as many texts as variables (i.e. 5 times
more rows than columns).

Variables that don’t correlate with any other variable are dropped. The
default threshold for dropping variables is **0.20**, but that can be
changed in the function arguments.

Also, MDA use a **promax** rotation.

For this demonstration, we’ll return 2 factors `n_factors = 2`:

``` r
m <- mda_loadings(d, n_factors = 2)
```

## MDA data structure

The `mda_loadings()` function returns a data frame containing the
dimension score for each document/text. The score is calculated by:

1.  Standardizing data by converting to z-scores
2.  For each text, summing all of the high-positive variables and
    subtracting all of the high-negative variables.

The high and high-negative are specified by a threshold value, which is
conventionally **0.35**, but can be set in the function arguments.

Also included in the data structure are the means-by-group, which is
used for plotting, and the factor loadings. The values are stored as
attributes.

### Means

``` r
attributes(m)$group_means
```

| group | Factor1    | Factor2    |
|-------|------------|------------|
| BIO   | -7.2662917 | -0.5964011 |
| CEE   | -9.7881051 | -0.1625027 |
| CLS   | 0.9138877  | -0.5898260 |
| ECO   | -5.4596175 | 0.3063591  |
| EDU   | 5.5817808  | -0.7646039 |
| ENG   | 8.6689423  | 1.0436717  |


### Factor loadings

``` r
attributes(m)$loadings %>% arrange(-Factor1)
```


|                      | Factor1   | Factor2    |
|----------------------------|-----------|------------|
| f\_42\_adverbs               | 0.7032851 | 0.1006736  |
| f\_65\_clausal_coordination  | 0.6562988 | -0.0899704 |
| f\_67\_neg_analytic          | 0.6549001 | 0.0748763  |
| f\_19\_be_main_verb          | 0.6540826 | 0.0730070  |
| f\_11\_indefinite_pronouns   | 0.6371959 | 0.0135475  |
| f\_06\_first\_person_pronouns | 0.5876115 | -0.2495020 |

\[…\]

|                         | Factor1    | Factor2    |
|-------------------------------|------------|------------|
| f\_14\_nominalizations        | -0.3921936 | 0.1912403  |
| f\_27\_past\_participle\_whiz | -0.4121017 | -0.1071897 |
| f\_39\_prepositions           | -0.4465220 | 0.0176608  |
| f\_40\_adj\_attr              | -0.5517525 | 0.1099862  |
| f\_44\_mean\_word\_length     | -0.6853363 | 0.0966360  |
| f\_16\_other\_nouns           | -0.7416364 | -0.0990779 |
## Ploting the results

One conventional way of plotting the results is to [place the means
along a cline in a kind of stick
plot](https://www.uni-bamberg.de/fileadmin/eng-ling/fs/Chapter_21/Index.html?23DimensionsofEnglish.html).

The package contains a convenience function for making these kinds of
plots. As with all plotting functions, it is easy enough to tweak the
code and customize your own plots.

``` r
stickplot_mda(m, n_factor=1)
```

![](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/mda_introduction_files/figure-gfm/stickplot_mda-1.png)<!-- -->

Along this particular dimension, Philosophy is positioned at the extreme
positive end, while History and various Engineering specialties are
positioned at the negative end.

You can also generate a plot that combines the stick plot with a heatmap
of the relevant variables and their factor loadings.

``` r
heatmap_mda(m, n_factor=1)
```

![](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/mda_introduction_files/figure-gfm/heatmap_mda-1.png)<!-- -->

The plot highlights the variables that contribute to the positive end of
cline like adverbs, *to be* as a main verb, and first-person pronouns.
At the other end, are nouns, longer words, more attributive adjectives,
and prepositions.

Alternatively, you can generate a plot of the kind that is common in
reporting PCA, which combines scaled vectors of the relevant variable
loadings and boxplots of the dimension scores organized by group.

``` r
boxplot_mda(m, n_factor=1)
```

![](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/mda_introduction_files/figure-gfm/boxplot_mda-1.png)<!-- -->

## Evaluating the dimensions

Finally, the variation explained by each dimension can measured using
linear regression.

We will hack together a table that uses ANOVA to evaluate the dimensions
and includes the *R*<sup>2</sup> value.

``` r

# Carry out regression
f1_lm <- lm(Factor1 ~ group, data = m)
f2_lm <- lm(Factor2 ~ group, data = m)

# Convert ANOVA results into data.frames allows for easier name manipulation
f1_aov <- data.frame(anova(f1_lm), r.squared = c(summary(f1_lm)$r.squared*100, NA))
f2_aov <- data.frame(anova(f2_lm), r.squared = c(summary(f2_lm)$r.squared*100, NA))

# Putting all into one data.frame/table
anova_results <- data.frame(rbind(c("DF", "Sum Sq", "Mean Sq", "F value", "Pr(>F)", "*R*^2^", 
                                    "DF", "Sum Sq", "Mean Sq", "F value", "Pr(>F)", "*R*^2^"), 
                                   cbind(round(f1_aov, 2), round(f2_aov, 2)))) 
colnames(anova_results) <- c("Dimension 1", "", "", "", "", "", "Dimension 2", "", "", "", "", "")
row.names(anova_results)[1] <- ""
anova_results[is.na(anova_results)] <- "--"
```

And output the results:

``` r
kable(anova_results)
```

| –         | Dimension 1 | –        | –       | –       | –       | –     | Dimension 2 | –      | –       | –       | –       | –     |
|-----------|-------------|----------|---------|---------|---------|-------|-------------|--------|---------|---------|---------|-------|
| –         | DF          | Sum Sq   | Mean Sq | F value | Pr(\>F) | *R*<sup>2</sup>  | DF          | Sum Sq | Mean Sq | F value | Pr(\>F) | *R*<sup>2</sup>  |
| group     | 16          | 57132.78 | 3570.8  | 30.04   | 0       | 37.21 | 16          | 615.63 | 38.48   | 14.22   | 0       | 21.91 |
| Residuals | 811         | 96414.81 | 118.88  | –       | –       | –     | 811         | 2194.8 | 2.71    | –       | –       | –     |

## Bibliography

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-biber1991variation" class="csl-entry">

Biber, Douglas. 1991. *Variation Across Speech and Writing*. Cambridge
University Press.
<https://books.google.com/books?id=CVTPaSSYEroC&printsec=frontcover#v=onepage&q&f=false>.

</div>

<div id="ref-biber2002speaking" class="csl-entry">

Biber, Douglas, Susan Conrad, Randi Reppen, Pat Byrd, and Marie Helt.
2002. “Speaking and Writing in the University: A Multidimensional
Comparison.” *Tesol Quarterly* 36 (1): 9–48.
<https://onlinelibrary.wiley.com/doi/abs/10.2307/3588359>.

</div>

<div id="ref-douglas1992multi" class="csl-entry">

Douglas, Douglas. 1992. “The Multi-Dimensional Approach to Linguistic
Analyses of Genre Variation: An Overview of Methodology and Findings.”
*Computers and the Humanities* 26 (5): 331–45.
<https://link.springer.com/article/10.1007/BF00136979>.

</div>

</div>
