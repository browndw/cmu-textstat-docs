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
data. See the **[preprocess vignette](https://cmu-textstat-docs.readthedocs.io/en/latest/quanteda.extras/vignettes/preprocess_introduction.html)** for more information.

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

The `keyness_table()` takes a target and a reference **[dfm](http://quanteda.io/reference/dfm.html)**. You can also apply the [“Yates correction”](https://influentialpoints.com/Training/g-likelihood_ratio_test.htm) by setting `yates=TRUE`.

``` r
kt <- keyness_table(sc_fict, sc_acad)
```

We can look at the first few rows of the table:

| Token | LL        | LR       | PV | AF\_Tar | AF\_Ref | Per\_10.5\_Tar | Per\_10.5\_Ref | DP\_Tar    | DP\_Ref    |
|-------|-----------|----------|----|--------|--------|--------------|--------------|-----------|-----------|
| i     | 2336.3687 | 4.006427 | 0  | 2428   | 143    | 1867.1322    | 116.17704    | 0.3242046 | 0.5431349 |
| she   | 1855.0335 | 4.575279 | 0  | 1763   | 70     | 1355.7471    | 56.86988     | 0.3747662 | 0.7893475 |
| he    | 1691.4745 | 3.461181 | 0  | 1978   | 170    | 1521.0821    | 138.11257    | 0.2638247 | 0.5816552 |
| her   | 1448.8023 | 3.826711 | 0  | 1559   | 104    | 1198.8711    | 84.49240     | 0.3796376 | 0.7746659 |
| you   | 1358.5514 | 4.605564 | 0  | 1286   | 50     | 988.9341     | 40.62134     | 0.2354467 | 0.7578357 |
| n’t   | 928.6952  | 4.330531 | 0  | 914    | 43     | 702.8661     | 34.93436     | 0.2028021 | 0.7417701 |

The columns are as follows:

1.  **LL**: the keyness value or
    [**log-likelihood**](http://ucrel.lancs.ac.uk/llwizard.html), also
    know as a G2 or goodness-of-fit test.
2.  **LR**: the effect size, which here is the [**log
    ratio**](http://cass.lancs.ac.uk/log-ratio-an-informal-introduction/)
3.  **PV**: the *p*-value associated with the log-likelihood
4.  **AF\_Tar**: the absolute frequency in the target corpus
5.  **AF\_Ref**: the absolute frequency in the reference corpus
6.  **Per\_10.x\_Tar**: the relative frequency in the target corpus (automatically calibrated to a normaizing factor, where here is per 100,000 tokens)
7.  **Per\_10.x\_Ref**: the relative frequency in the reference corpus (automatically calibrated to a normaizing factor, where here is per 100,000 tokens)
8.  **DP\_Tar**: the [**deviation of proportions**](https://www.researchgate.net/publication/233685362_Dispersions_and_adjusted_frequencies_in_corpora) (a dispersion measure) in the target corpus
9.  **DP\_Ref**: the deviation of proportions in the reference corpus

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

| token | key_range |  key_mean |    key_sd | effect_mean |
|-------|----------:|----------:|----------:|------------:|
| i     |        92 | 187.15374 | 177.06824 |    3.375994 |
| she   |        78 | 163.11509 | 179.51970 |    3.446611 |
| he    |        90 | 124.38986 | 104.13721 |    3.012114 |
| her   |        78 | 119.87934 | 131.16379 |    2.773899 |
| you   |        96 | 110.76772 |  94.19243 |    4.298598 |
| n’t   |        94 |  72.22032 |  47.96801 |    4.087920 |

## Keyness pairs

There is also a function for quickly generating pair-wise keyness
comparisions among multiple sub-corpora. To demonstrate, create a third **[dfm](http://quanteda.io/reference/dfm.html)**, this time containing news articles.

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

| Token |     LL    |    LR    | PV | AF_Tar | AF_Ref | Per_10.5_Tar | Per_10.5_Ref |   DP_Tar  |   DP_Ref  |
|:-----:|:---------:|:--------:|:--:|:------:|:------:|:------------:|:------------:|:---------:|:---------:|
| i     | 2336.3687 | 4.006427 | 0  | 2428   | 143    | 1867.1322    | 116.17704    | 0.3242046 | 0.5431349 |
| she   | 1855.0335 | 4.575279 | 0  | 1763   | 70     | 1355.7471    | 56.86988     | 0.3747662 | 0.7893475 |
| he    | 1691.4745 | 3.461181 | 0  | 1978   | 170    | 1521.0821    | 138.11257    | 0.2638247 | 0.5816552 |
| her   | 1448.8023 | 3.826711 | 0  | 1559   | 104    | 1198.8711    | 84.49240     | 0.3796376 | 0.7746659 |
| you   | 1358.5514 | 4.605564 | 0  | 1286   | 50     | 988.9341     | 40.62134     | 0.2354467 | 0.7578357 |
| n’t   | 928.6952  | 4.330531 | 0  | 914    | 43     | 702.8661     | 34.93436     | 0.2028021 | 0.7417701 |

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
