# Introduction to dispersion functions in the quanteda.extras R package

## Load the quanteda.extras package

Load the package, as well as others that we’ll use in this vignette.

``` r
library(quanteda.extras)
library(quanteda)
library(tidyverse)
```

## Prepare the data

First, we’ll use to `preprocess_text()` function to “clean” the text data. See the **[preprocess vignette](https://cmu-textstat-docs.readthedocs.io/en/latest/quanteda.extras/vignettes/preprocess_introduction.html)** for more information.

``` r
sc <- sample_corpus %>%
  mutate(text = preprocess_text(text))
```

Next, create a corpus and tokenize the data:

``` r
sc_tokens <- sc %>%
  corpus() %>% # create a corpus object
  tokens(what="fastestword", remove_numbers=TRUE) # tokenize the data
```

## Frequency table

The `frequency_table()` function aggregates useful descriptive
measures: absolute frequency, relative frequency, average reduced
frequency, and deviation of proportions.

The relatvie frequency (**Per_10.x**) the relatvie frequency is
automatically calibrated to a normaizing factor, where here is per million tokens.

[Average reduced frequency](https://www.tandfonline.com/doi/abs/10.1076/jqul.9.3.215.14124) (**ARF**) combines dispersion and frequency into a single measure by de-emphasizing occurrences of a token that appear clustered in close proximity.

[Deviation of proportions](https://www.researchgate.net/publication233685362_Dispersions_and_adjusted_frequencies_in_corpora) (**DP**) is a dispersion statistic proposed by Greis, which measures dispersion on a scale of 0 to 1 such that tokens with DP close to zero are **more** dispersed while those closer to 1 are **less** dispersed.

``` r
ft <- frequency_table(sc_tokens)
```

Check the result:

| Token | AF    | Per_10.6 | ARF      | DP        |
|-------|-------|----------|----------|-----------|
| the   | 51046 | 50368.10 | 31828.29 | 0.1429544 |
| to    | 25942 | 25597.48 | 16269.94 | 0.0883799 |
| and   | 25344 | 25007.43 | 15936.60 | 0.1251384 |
| of    | 24331 | 24007.88 | 14706.63 | 0.1772988 |
| a     | 22694 | 22392.62 | 13958.30 | 0.1066691 |
| in    | 16959 | 16733.78 | 10391.13 | 0.1462487 |

## Dispersion statstics for all tokens

A data.frame of common dispersion measures can be generated using the `dispersions_all()` function. The table includes:

- Carroll’s *D*<sub>2</sub>
- Rosengren’s *S*
- Lyne’s *D*<sub>3</sub>
- Distributional Consistency (DC)
- Juilland’s *D*
- Deviation of Proportions
- Deviation of Proportions *Norm*

To create the table, the the **quanteda tokens** must first be converted into a **[dfm](http://quanteda.io/reference/dfm.html)**:

``` r
sc_dfm <- sc_tokens %>% dfm()
dt <- dispersions_all(sc_dfm)
```

Check the result:

| Token | AF    | Per_10.6 | Carrolls_D2 | Rosengrens_S | Lynes_D3  | DC        | Juillands_D | DP        | DP_norm   |
|-------|-------|----------|-------------|--------------|-----------|-----------|-------------|-----------|-----------|
| the   | 51046 | 50368.10 | 0.9886214   | 0.9643705    | 0.9675432 | 0.9629224 | 0.9817403   | 0.1429544 | 0.1430920 |
| to    | 25942 | 25597.48 | 0.9937932   | 0.9812724    | 0.9841870 | 0.9768320 | 0.9876936   | 0.0883799 | 0.0884650 |
| and   | 25344 | 25007.43 | 0.9902678   | 0.9687633    | 0.9732413 | 0.9670243 | 0.9832963   | 0.1251384 | 0.1252588 |
| of    | 24331 | 24007.88 | 0.9830139   | 0.9460955    | 0.9490047 | 0.9465552 | 0.9769251   | 0.1772988 | 0.1774694 |
| a     | 22694 | 22392.62 | 0.9922448   | 0.9772786    | 0.9792415 | 0.9727416 | 0.9858140   | 0.1066691 | 0.1067717 |
| in    | 16959 | 16733.78 | 0.9878687   | 0.9619502    | 0.9642931 | 0.9611510 | 0.9805055   | 0.1462487 | 0.1463894 |

## Dispersion statistics for a singe token

The `dispersions_token()` function calculates the dispersion measures for a single token. It returns a named list with all of the dispersion measures discussed by [S.T. Gries](http://www.stgries.info/research/dispersion/links.html) and the function is baesed on a script he originally authored.

``` r
a <- dispersions_token(sc_dfm, "data")
```
| Measure                                                                   | Dispersion   |
|---------------------------------------------------------------------------|-------------:|
| Absolute frequency                                                        | 199.0000000  |
| Per_10.6                                                                  | 196.3572281  |
| Relative entropy of all sizes of the corpus parts                         | 0.9996961    |
| Range                                                                     | 63.0000000   |
| Maxmin                                                                    | 19.0000000   |
| Standard deviation                                                        | 1.8078064    |
| Variation coefficient                                                     | 3.6337818    |
| Chi-square                                                                | 2660.6578664 |
| Juilland et al.’s D (based on equally-sized corpus parts)                 | 0.8180834    |
| Juilland et al.’s D (not requiring equally-sized corpus parts)            | 0.8183566    |
| Carroll’s D2                                                              | 0.6203020    |
| Rosengren’s S (based on equally-sized corpus parts)                       | 0.1279004    |
| Rosengren’s S (not requiring equally-sized corpus parts)                  | 0.1260951    |
| Lyne’s D3 (not requiring equally-sized corpus parts)                      | -2.2928398   |
| Distributional consistency DC                                             | 0.1279004    |
| Inverse document frequency IDF                                            | 2.6665763    |
| Engvall’s measure                                                         | 31.3425000   |
| Juilland et al.’s U (based on equally-sized corpus parts)                 | 162.7985909  |
| Juilland et al.’s U (not requiring equally-sized corpus parts)            | 162.8529550  |
| Carroll’s Um (based on equally sized corpus parts)                        | 123.6290008  |
| Rosengren’s Adjusted Frequency (based on equally sized corpus parts)      | 25.4521852   |
| Rosengren’s Adjusted Frequency (not requiring equally sized corpus parts) | 25.0929244   |
| Kromer’s Ur                                                               | 100.8473290  |
| Deviation of proportions DP                                               | 0.8444693    |
| Deviation of proportions DP (normalized)                                  | 0.8452817    |

## Bibliography

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-gries2008dispersions" class="csl-entry">

Gries, Stefan Th. 2008. “Dispersions and Adjusted Frequencies in
Corpora.” *International Journal of Corpus Linguistics* 13 (4): 403–37.
<https://www.jbe-platform.com/content/journals/10.1075/ijcl.13.4.02gri>.

</div>

<div id="ref-gries2010dispersions" class="csl-entry">

———. 2010. “Dispersions and Adjusted Frequencies in Corpora: Further
Explorations.” In *Corpus-Linguistic Applications*, 197–212. Brill.
<https://www.researchgate.net/profile/Stefan-Gries-2/publication/233650934_Dispersions_and_adjusted_frequencies_in_corpora_further_explorations/links/0a85e52fe8f1de61fc000000/Dispersions-and-adjusted-frequencies-in-corpora-further-explorations.pdf>.

</div>

</div>
