# Introduction to keyness functions in the quanteda.extras R package

## Load the quanteda.extras package

Load the package, as well as others that we’ll use in this vignette.

``` r
library(quanteda.extras)
library(quanteda)
library(tidyverse)
```

## Prepare the data

## Prepare the data

First, we’ll use to `preprocess_text()` function to “clean” the text
data. See the **preprocess vignette** for more information.

``` r
sc <- sample_corpus %>%
  mutate(text = preprocess_text(text))
```

Next, we’ll subset the data and create two sub-corpora: one of fiction
texts and one of academic.

``` r
sc_fict <- sc %>%
  filter(str_detect(doc_id, "fic")) %>% # select the texts
  corpus() %>% # create a corpus object
  tokens(what="fastestword", remove_numbers=TRUE) %>% # tokenize
  dfm() # create a document-feature matrix (dfm)

sc_acad <- sc %>%
  filter(str_detect(doc_id, "acad")) %>% # select the texts
  corpus() %>% # create a corpus object
  tokens(what="fastestword", remove_numbers=TRUE) %>% # tokenize
  dfm() # create a document-feature matrix (dfm)
```

There are a couple of important issues to be aware of:

1.  The **quanteda** package has it’s own native keyness function as part of [**quanteda.textstats**](https://cran.r-project.org/web/packages/quanteda.textstats/index.html): `textstat_keyness()`.
2.  Using the `textstat_keyness()` function requires a slightly different workflow, but is perfectly fine if you only want to generate a basic keyness statistic.
3.  The keyness functions here expand that basic functionality by adding effect sizes and other measures, as well as an implementation of **“key key words,”** which accounts for how distributed key words are in the target corpus.

## Generate a keyness table

The `keyness_table()` takes a target and a reference **dfm**. You can also apply the [“Yates correction”](https://influentialpoints.com/Training/g-likelihood_ratio_test.htm) by setting `yates=TRUE`.

``` r
kt <- keyness_table(sc_fict, sc_acad)
```

We can look at the first few rows of the table:

<table>
<thead>
<tr>
<th style="text-align:left;">
Token
</th>
<th style="text-align:right;">
LL
</th>
<th style="text-align:right;">
LR
</th>
<th style="text-align:right;">
PV
</th>
<th style="text-align:right;">
AF_Tar
</th>
<th style="text-align:right;">
AF_Ref
</th>
<th style="text-align:right;">
Per_10.5_Tar
</th>
<th style="text-align:right;">
Per_10.5_Ref
</th>
<th style="text-align:right;">
DP_Tar
</th>
<th style="text-align:right;">
DP_Ref
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
i
</td>
<td style="text-align:right;">
2336.3687
</td>
<td style="text-align:right;">
4.006427
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
2428
</td>
<td style="text-align:right;">
143
</td>
<td style="text-align:right;">
1867.1322
</td>
<td style="text-align:right;">
116.17704
</td>
<td style="text-align:right;">
0.3242046
</td>
<td style="text-align:right;">
0.5431349
</td>
</tr>
<tr>
<td style="text-align:left;">
she
</td>
<td style="text-align:right;">
1855.0335
</td>
<td style="text-align:right;">
4.575279
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1763
</td>
<td style="text-align:right;">
70
</td>
<td style="text-align:right;">
1355.7471
</td>
<td style="text-align:right;">
56.86988
</td>
<td style="text-align:right;">
0.3747662
</td>
<td style="text-align:right;">
0.7893475
</td>
</tr>
<tr>
<td style="text-align:left;">
he
</td>
<td style="text-align:right;">
1691.4745
</td>
<td style="text-align:right;">
3.461181
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1978
</td>
<td style="text-align:right;">
170
</td>
<td style="text-align:right;">
1521.0821
</td>
<td style="text-align:right;">
138.11257
</td>
<td style="text-align:right;">
0.2638247
</td>
<td style="text-align:right;">
0.5816552
</td>
</tr>
<tr>
<td style="text-align:left;">
her
</td>
<td style="text-align:right;">
1448.8023
</td>
<td style="text-align:right;">
3.826711
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1559
</td>
<td style="text-align:right;">
104
</td>
<td style="text-align:right;">
1198.8711
</td>
<td style="text-align:right;">
84.49240
</td>
<td style="text-align:right;">
0.3796376
</td>
<td style="text-align:right;">
0.7746659
</td>
</tr>
<tr>
<td style="text-align:left;">
you
</td>
<td style="text-align:right;">
1358.5514
</td>
<td style="text-align:right;">
4.605564
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1286
</td>
<td style="text-align:right;">
50
</td>
<td style="text-align:right;">
988.9341
</td>
<td style="text-align:right;">
40.62134
</td>
<td style="text-align:right;">
0.2354467
</td>
<td style="text-align:right;">
0.7578357
</td>
</tr>
<tr>
<td style="text-align:left;">
n’t
</td>
<td style="text-align:right;">
928.6952
</td>
<td style="text-align:right;">
4.330531
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
914
</td>
<td style="text-align:right;">
43
</td>
<td style="text-align:right;">
702.8661
</td>
<td style="text-align:right;">
34.93436
</td>
<td style="text-align:right;">
0.2028021
</td>
<td style="text-align:right;">
0.7417701
</td>
</tr>
</tbody>
</table>

The columns are as follows:

1.  **LL**: the keyness value or
    [**log-likelihood**](http://ucrel.lancs.ac.uk/llwizard.html), also
    know as a G2 or goodness-of-fit test.
2.  **LR**: the effect size, which here is the [**log
    ratio**](http://cass.lancs.ac.uk/log-ratio-an-informal-introduction/)
3.  **PV**: the *p*-value associated with the log-likelihood
4.  **AF_Tar**: the absolute frequency in the target corpus
5.  **AF_Ref**: the absolute frequency in the reference corpus
6.  **Per_10.x_Tar**: the relative frequency in the target corpus (automatically calibrated to a normaizing factor, where here is per 100,000 tokens)
7.  **Per_10.x_Ref**: the relative frequency in the reference corpus (automatically calibrated to a normaizing factor, where here is per 100,000 tokens)
8.  **DP_Tar**: the [**deviation of proportions**](https://www.researchgate.net/publication/233685362_Dispersions_and_adjusted_frequencies_in_corpora) (a dispersion measure) in the target corpus
9.  **DP_Ref**: the deviation of proportions in the reference corpus

## Key key words

The concept of [“**key key
words**”](https://lexically.net/downloads/version5/HTML/index.html?keykeyness_definition.htm) was introduced by Mike Smith for the WordSmith concordancer. The process compares each text in the target corpus to the reference corpus. Log-likelihood is calculated for each comparison. Then a mean is calculated for keyness and effect size. In addition, a range is provided
for the number of texts in which keyness reaches significance for a given threshold. (The default is *p* \< 0.05.) That range is returned as
a percentage.

In this way, **key key words** accounts for the dispersion of key words by indicating whether a keyness value is driven by a relatively high frequency in a few target texts or many.

``` r
kk <- key_keys(sc_fict, sc_acad)
```

Again, we can look at the first few rows of the table:

<table>
<thead>
<tr>
<th style="text-align:left;">
token
</th>
<th style="text-align:right;">
key_range
</th>
<th style="text-align:right;">
key_mean
</th>
<th style="text-align:right;">
key_sd
</th>
<th style="text-align:right;">
effect_mean
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
i
</td>
<td style="text-align:right;">
92
</td>
<td style="text-align:right;">
187.15374
</td>
<td style="text-align:right;">
177.06824
</td>
<td style="text-align:right;">
3.375994
</td>
</tr>
<tr>
<td style="text-align:left;">
she
</td>
<td style="text-align:right;">
78
</td>
<td style="text-align:right;">
163.11509
</td>
<td style="text-align:right;">
179.51970
</td>
<td style="text-align:right;">
3.446611
</td>
</tr>
<tr>
<td style="text-align:left;">
he
</td>
<td style="text-align:right;">
90
</td>
<td style="text-align:right;">
124.38986
</td>
<td style="text-align:right;">
104.13721
</td>
<td style="text-align:right;">
3.012114
</td>
</tr>
<tr>
<td style="text-align:left;">
her
</td>
<td style="text-align:right;">
78
</td>
<td style="text-align:right;">
119.87934
</td>
<td style="text-align:right;">
131.16379
</td>
<td style="text-align:right;">
2.773899
</td>
</tr>
<tr>
<td style="text-align:left;">
you
</td>
<td style="text-align:right;">
96
</td>
<td style="text-align:right;">
110.76772
</td>
<td style="text-align:right;">
94.19243
</td>
<td style="text-align:right;">
4.298598
</td>
</tr>
<tr>
<td style="text-align:left;">
n’t
</td>
<td style="text-align:right;">
94
</td>
<td style="text-align:right;">
72.22032
</td>
<td style="text-align:right;">
47.96801
</td>
<td style="text-align:right;">
4.087920
</td>
</tr>
</tbody>
</table>

## Keyness pairs

There is also a function for quickly generating pair-wise keyness
comparisions among multiple sub-corpora. To demonstrate, create a third **dfm**, this time containing news articles.

``` r
sc_news <- sc %>%
  filter(str_detect(doc_id, "news")) %>% # select the texts
  corpus() %>% # create a corpus object
  tokens(what="fastestword", remove_numbers=TRUE) %>% # tokenize
  dfm() # create a document-feature matrix (dfm)
```

To produce a data.frame comparing more than two sup-corpora, use the `keyness_pairs()` function:

``` r
kp <- keyness_pairs(sc_news, sc_acad, sc_fict)
```

Check the result:

<table>
<thead>
<tr>
<th style="text-align:left;">
Token
</th>
<th style="text-align:right;">
A_v_B_LL
</th>
<th style="text-align:right;">
A_v_B_LR
</th>
<th style="text-align:right;">
A_v_B_PV
</th>
<th style="text-align:right;">
A_v_C_LL
</th>
<th style="text-align:right;">
A_v_C_LR
</th>
<th style="text-align:right;">
A_v_C_PV
</th>
<th style="text-align:right;">
B_v_C_LL
</th>
<th style="text-align:right;">
B_v_C_LR
</th>
<th style="text-align:right;">
B_v_C_PV
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
he
</td>
<td style="text-align:right;">
493.5483
</td>
<td style="text-align:right;">
2.326763
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
-394.963211
</td>
<td style="text-align:right;">
-1.1344187
</td>
<td style="text-align:right;">
0.0000000
</td>
<td style="text-align:right;">
-1691.47454
</td>
<td style="text-align:right;">
-3.461181
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:left;">
said
</td>
<td style="text-align:right;">
456.3626
</td>
<td style="text-align:right;">
3.691213
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1.925817
</td>
<td style="text-align:right;">
0.1296518
</td>
<td style="text-align:right;">
0.1652169
</td>
<td style="text-align:right;">
-415.45402
</td>
<td style="text-align:right;">
-3.561561
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:left;">
i
</td>
<td style="text-align:right;">
432.3058
</td>
<td style="text-align:right;">
2.360219
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
-854.228933
</td>
<td style="text-align:right;">
-1.6462082
</td>
<td style="text-align:right;">
0.0000000
</td>
<td style="text-align:right;">
-2336.36875
</td>
<td style="text-align:right;">
-4.006427
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:left;">
n’t
</td>
<td style="text-align:right;">
333.8795
</td>
<td style="text-align:right;">
3.231694
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
-173.379610
</td>
<td style="text-align:right;">
-1.0988371
</td>
<td style="text-align:right;">
0.0000000
</td>
<td style="text-align:right;">
-928.69517
</td>
<td style="text-align:right;">
-4.330531
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:left;">
you
</td>
<td style="text-align:right;">
328.3448
</td>
<td style="text-align:right;">
3.064351
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
-411.300060
</td>
<td style="text-align:right;">
-1.5412134
</td>
<td style="text-align:right;">
0.0000000
</td>
<td style="text-align:right;">
-1358.55143
</td>
<td style="text-align:right;">
-4.605564
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:left;">
mr
</td>
<td style="text-align:right;">
237.0320
</td>
<td style="text-align:right;">
5.098339
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
75.198530
</td>
<td style="text-align:right;">
1.6128090
</td>
<td style="text-align:right;">
0.0000000
</td>
<td style="text-align:right;">
-61.08761
</td>
<td style="text-align:right;">
-3.485530
</td>
<td style="text-align:right;">
0
</td>
</tr>
</tbody>
</table>

## Bibliography

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-hlavavcova2006new" class="csl-entry">

Hlaváčová, J. 2006. “New Approach to Frequency Dictionaries—Czech
Example. 2006.” In *Paper till the International Conference on Language
Resources*.

</div>

<div id="ref-rayson2000comparing" class="csl-entry">

Rayson, Paul, and Roger Garside. 2000. “Comparing Corpora Using
Frequency Profiling.” In *The Workshop on Comparing Corpora*, 1–6.
<https://aclanthology.org/W00-0901.pdf>.

</div>

<div id="ref-savicky2002measures" class="csl-entry">

Savickỳ, Petr, and Jaroslava Hlavácová. 2002. “Measures of Word
Commonness.” *Journal of Quantitative Linguistics* 9 (3): 215–31.
<https://www.tandfonline.com/doi/abs/10.1076/jqul.9.3.215.14124>.

</div>

<div id="ref-scott1997pc" class="csl-entry">

Scott, Mike. 1997. “PC Analysis of Key Words—and Key Key Words.”
*System* 25 (2): 233–45.
<https://www.sciencedirect.com/science/article/abs/pii/S0346251X97000110>.

</div>

</div>
