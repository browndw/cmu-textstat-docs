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

<table>
<thead>
<tr>
<th style="text-align:left;">
doc_id
</th>
<th style="text-align:right;">
f_01_past_tense
</th>
<th style="text-align:right;">
f_02_perfect_aspect
</th>
<th style="text-align:right;">
f_03_present_tense
</th>
<th style="text-align:right;">
f_04_place_adverbials
</th>
<th style="text-align:right;">
f_05_time_adverbials
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
BIO.G0.01.1.txt
</td>
<td style="text-align:right;">
17.678709
</td>
<td style="text-align:right;">
8.070715
</td>
<td style="text-align:right;">
52.45965
</td>
<td style="text-align:right;">
3.074558
</td>
<td style="text-align:right;">
4.035357
</td>
</tr>
<tr>
<td style="text-align:left;">
BIO.G0.02.1.txt
</td>
<td style="text-align:right;">
11.477762
</td>
<td style="text-align:right;">
10.043042
</td>
<td style="text-align:right;">
61.33429
</td>
<td style="text-align:right;">
2.152080
</td>
<td style="text-align:right;">
6.097561
</td>
</tr>
<tr>
<td style="text-align:left;">
BIO.G0.02.2.txt
</td>
<td style="text-align:right;">
3.875969
</td>
<td style="text-align:right;">
0.000000
</td>
<td style="text-align:right;">
62.01550
</td>
<td style="text-align:right;">
0.000000
</td>
<td style="text-align:right;">
1.291990
</td>
</tr>
<tr>
<td style="text-align:left;">
BIO.G0.02.3.txt
</td>
<td style="text-align:right;">
1.700680
</td>
<td style="text-align:right;">
3.401361
</td>
<td style="text-align:right;">
64.62585
</td>
<td style="text-align:right;">
0.000000
</td>
<td style="text-align:right;">
0.000000
</td>
</tr>
<tr>
<td style="text-align:left;">
BIO.G0.02.4.txt
</td>
<td style="text-align:right;">
1.531394
</td>
<td style="text-align:right;">
4.594181
</td>
<td style="text-align:right;">
70.44410
</td>
<td style="text-align:right;">
1.531394
</td>
<td style="text-align:right;">
0.000000
</td>
</tr>
<tr>
<td style="text-align:left;">
BIO.G0.02.5.txt
</td>
<td style="text-align:right;">
14.844804
</td>
<td style="text-align:right;">
7.759784
</td>
<td style="text-align:right;">
47.57085
</td>
<td style="text-align:right;">
3.373819
</td>
<td style="text-align:right;">
2.024291
</td>
</tr>
</tbody>
</table>

\[…\]

<table>
<thead>
<tr>
<th style="text-align:right;">
f_62_split_infinitve
</th>
<th style="text-align:right;">
f_63_split_auxiliary
</th>
<th style="text-align:right;">
f_64_phrasal_coordination
</th>
<th style="text-align:right;">
f_65_clausal_coordination
</th>
<th style="text-align:right;">
f_66_neg_synthetic
</th>
<th style="text-align:right;">
f_67_neg_analytic
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
0.1921599
</td>
<td style="text-align:right;">
4.611837
</td>
<td style="text-align:right;">
8.262875
</td>
<td style="text-align:right;">
5.956956
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
4.227517
</td>
</tr>
<tr>
<td style="text-align:right;">
0.7173601
</td>
<td style="text-align:right;">
5.021521
</td>
<td style="text-align:right;">
6.097561
</td>
<td style="text-align:right;">
3.945481
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
2.869441
</td>
</tr>
<tr>
<td style="text-align:right;">
0.0000000
</td>
<td style="text-align:right;">
3.875969
</td>
<td style="text-align:right;">
6.459948
</td>
<td style="text-align:right;">
1.291990
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
3.875969
</td>
</tr>
<tr>
<td style="text-align:right;">
0.0000000
</td>
<td style="text-align:right;">
1.700680
</td>
<td style="text-align:right;">
18.707483
</td>
<td style="text-align:right;">
0.000000
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0.000000
</td>
</tr>
<tr>
<td style="text-align:right;">
3.0627871
</td>
<td style="text-align:right;">
6.125574
</td>
<td style="text-align:right;">
7.656968
</td>
<td style="text-align:right;">
0.000000
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
7.656968
</td>
</tr>
<tr>
<td style="text-align:right;">
0.6747638
</td>
<td style="text-align:right;">
4.723347
</td>
<td style="text-align:right;">
3.036437
</td>
<td style="text-align:right;">
1.686910
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
5.398111
</td>
</tr>
</tbody>
</table>

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

<table>
<thead>
<tr>
<th style="text-align:left;">
group
</th>
<th style="text-align:right;">
Factor1
</th>
<th style="text-align:right;">
Factor2
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
BIO
</td>
<td style="text-align:right;">
-7.2662917
</td>
<td style="text-align:right;">
-0.5964011
</td>
</tr>
<tr>
<td style="text-align:left;">
CEE
</td>
<td style="text-align:right;">
-9.7881051
</td>
<td style="text-align:right;">
-0.1625027
</td>
</tr>
<tr>
<td style="text-align:left;">
CLS
</td>
<td style="text-align:right;">
0.9138877
</td>
<td style="text-align:right;">
-0.5898260
</td>
</tr>
<tr>
<td style="text-align:left;">
ECO
</td>
<td style="text-align:right;">
-5.4596175
</td>
<td style="text-align:right;">
0.3063591
</td>
</tr>
<tr>
<td style="text-align:left;">
EDU
</td>
<td style="text-align:right;">
5.5817808
</td>
<td style="text-align:right;">
-0.7646039
</td>
</tr>
<tr>
<td style="text-align:left;">
ENG
</td>
<td style="text-align:right;">
8.6689423
</td>
<td style="text-align:right;">
1.0436717
</td>
</tr>
</tbody>
</table>

### Factor loadings

``` r
attributes(m)$loadings %>% arrange(-Factor1)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
Factor1
</th>
<th style="text-align:right;">
Factor2
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
f_42_adverbs
</td>
<td style="text-align:right;">
0.7032851
</td>
<td style="text-align:right;">
0.1006736
</td>
</tr>
<tr>
<td style="text-align:left;">
f_65_clausal_coordination
</td>
<td style="text-align:right;">
0.6562988
</td>
<td style="text-align:right;">
-0.0899704
</td>
</tr>
<tr>
<td style="text-align:left;">
f_67_neg_analytic
</td>
<td style="text-align:right;">
0.6549001
</td>
<td style="text-align:right;">
0.0748763
</td>
</tr>
<tr>
<td style="text-align:left;">
f_19_be_main_verb
</td>
<td style="text-align:right;">
0.6540826
</td>
<td style="text-align:right;">
0.0730070
</td>
</tr>
<tr>
<td style="text-align:left;">
f_11_indefinite_pronouns
</td>
<td style="text-align:right;">
0.6371959
</td>
<td style="text-align:right;">
0.0135475
</td>
</tr>
<tr>
<td style="text-align:left;">
f_06_first_person_pronouns
</td>
<td style="text-align:right;">
0.5876115
</td>
<td style="text-align:right;">
-0.2495020
</td>
</tr>
</tbody>
</table>

\[…\]

<table>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
Factor1
</th>
<th style="text-align:right;">
Factor2
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
f_14_nominalizations
</td>
<td style="text-align:right;">
-0.3921936
</td>
<td style="text-align:right;">
0.1912403
</td>
</tr>
<tr>
<td style="text-align:left;">
f_27_past_participle_whiz
</td>
<td style="text-align:right;">
-0.4121017
</td>
<td style="text-align:right;">
-0.1071897
</td>
</tr>
<tr>
<td style="text-align:left;">
f_39_prepositions
</td>
<td style="text-align:right;">
-0.4465220
</td>
<td style="text-align:right;">
0.0176608
</td>
</tr>
<tr>
<td style="text-align:left;">
f_40_adj_attr
</td>
<td style="text-align:right;">
-0.5517525
</td>
<td style="text-align:right;">
0.1099862
</td>
</tr>
<tr>
<td style="text-align:left;">
f_44_mean_word_length
</td>
<td style="text-align:right;">
-0.6853363
</td>
<td style="text-align:right;">
0.0966360
</td>
</tr>
<tr>
<td style="text-align:left;">
f_16_other_nouns
</td>
<td style="text-align:right;">
-0.7416364
</td>
<td style="text-align:right;">
-0.0990779
</td>
</tr>
</tbody>
</table>

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

<table>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
Dimension 1
</th>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
Dimension 2
</th>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
</td>
<td style="text-align:left;">
DF
</td>
<td style="text-align:left;">
Sum Sq
</td>
<td style="text-align:left;">
Mean Sq
</td>
<td style="text-align:left;">
F value
</td>
<td style="text-align:left;">
Pr(\>F)
</td>
<td style="text-align:left;">
*R*<sup>2</sup>
</td>
<td style="text-align:left;">
DF
</td>
<td style="text-align:left;">
Sum Sq
</td>
<td style="text-align:left;">
Mean Sq
</td>
<td style="text-align:left;">
F value
</td>
<td style="text-align:left;">
Pr(\>F)
</td>
<td style="text-align:left;">
*R*<sup>2</sup>
</td>
</tr>
<tr>
<td style="text-align:left;">
group
</td>
<td style="text-align:left;">
16
</td>
<td style="text-align:left;">
57132.78
</td>
<td style="text-align:left;">
3570.8
</td>
<td style="text-align:left;">
30.04
</td>
<td style="text-align:left;">
0
</td>
<td style="text-align:left;">
37.21
</td>
<td style="text-align:left;">
16
</td>
<td style="text-align:left;">
615.63
</td>
<td style="text-align:left;">
38.48
</td>
<td style="text-align:left;">
14.22
</td>
<td style="text-align:left;">
0
</td>
<td style="text-align:left;">
21.91
</td>
</tr>
<tr>
<td style="text-align:left;">
Residuals
</td>
<td style="text-align:left;">
811
</td>
<td style="text-align:left;">
96414.81
</td>
<td style="text-align:left;">
118.88
</td>
<td style="text-align:left;">
–
</td>
<td style="text-align:left;">
–
</td>
<td style="text-align:left;">
–
</td>
<td style="text-align:left;">
811
</td>
<td style="text-align:left;">
2194.8
</td>
<td style="text-align:left;">
2.71
</td>
<td style="text-align:left;">
–
</td>
<td style="text-align:left;">
–
</td>
<td style="text-align:left;">
–
</td>
</tr>
</tbody>
</table>

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
