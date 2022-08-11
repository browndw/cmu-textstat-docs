# Introduction to dispersion functions in the quanteda.extras R package

## Load the quanteda.extras package

Load the package, as well as others that we’ll use in this vignette.

``` r
library(quanteda.extras)
library(quanteda)
library(tidyverse)
```

## Prepare the data

First, we’ll use to `preprocess_text()` function to “clean” the text
data. See the **preprocess vignette** for more information.

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

The **frequency_table( )** function aggregates useful descriptive
measures: absolute frequency, relative frequency, average reduced
frequency, and deviation of proportions.

The relatvie frequency (**Per_10.x**) the relatvie frequency is
automatically calibrated to a normaizing factor, where here is per million tokens.

Average reduced frequency (**ARF**) combines dispersion and frequency into a single measure by de-emphasizing occurrences of a token that appear clustered in close proximity.

[Deviation of proportions](https://www.researchgate.net/publication233685362_Dispersions_and_adjusted_frequencies_in_corpora) (**DP**) is a dispersion statistic proposed by Greis, which measures dispersion on a scale of 0 to 1 such that tokens with DP close to zero are **more** dispersed while those closer to 1 are **less** dispersed.

``` r
ft <- frequency_table(sc_tokens)
```

Check the result:

<table>
<thead>
<tr>
<th style="text-align:left;">
Token
</th>
<th style="text-align:right;">
AF
</th>
<th style="text-align:right;">
Per_10.6
</th>
<th style="text-align:right;">
ARF
</th>
<th style="text-align:right;">
DP
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
the
</td>
<td style="text-align:right;">
51046
</td>
<td style="text-align:right;">
50368.10
</td>
<td style="text-align:right;">
31828.29
</td>
<td style="text-align:right;">
0.1429544
</td>
</tr>
<tr>
<td style="text-align:left;">
to
</td>
<td style="text-align:right;">
25942
</td>
<td style="text-align:right;">
25597.48
</td>
<td style="text-align:right;">
16269.94
</td>
<td style="text-align:right;">
0.0883799
</td>
</tr>
<tr>
<td style="text-align:left;">
and
</td>
<td style="text-align:right;">
25344
</td>
<td style="text-align:right;">
25007.43
</td>
<td style="text-align:right;">
15936.60
</td>
<td style="text-align:right;">
0.1251384
</td>
</tr>
<tr>
<td style="text-align:left;">
of
</td>
<td style="text-align:right;">
24331
</td>
<td style="text-align:right;">
24007.88
</td>
<td style="text-align:right;">
14706.63
</td>
<td style="text-align:right;">
0.1772988
</td>
</tr>
<tr>
<td style="text-align:left;">
a
</td>
<td style="text-align:right;">
22694
</td>
<td style="text-align:right;">
22392.62
</td>
<td style="text-align:right;">
13958.30
</td>
<td style="text-align:right;">
0.1066691
</td>
</tr>
<tr>
<td style="text-align:left;">
in
</td>
<td style="text-align:right;">
16959
</td>
<td style="text-align:right;">
16733.78
</td>
<td style="text-align:right;">
10391.13
</td>
<td style="text-align:right;">
0.1462487
</td>
</tr>
</tbody>
</table>

## Dispersion statstics for all tokens

A data.frame of common dispersion measures can be generated using the `dispersions_all()` function. The table includes:

- Carroll’s *D*<sub>2</sub>
- Rosengren’s *S*
- Lyne’s *D*<sub>3</sub>
- Distributional Consistency (DC)
- Juilland’s *D*
- Deviation of Proportions
- Deviation of Proportions *Norm*

To create the table, the the **quanteda tokens** must first be converted into a **dfm**:

``` r
sc_dfm <- sc_tokens %>% dfm()
dt <- dispersions_all(sc_dfm)
```

Check the result:

<table>
<thead>
<tr>
<th style="text-align:left;">
Token
</th>
<th style="text-align:right;">
AF
</th>
<th style="text-align:right;">
Per_10.6
</th>
<th style="text-align:right;">
Carrolls_D2
</th>
<th style="text-align:right;">
Rosengrens_S
</th>
<th style="text-align:right;">
Lynes_D3
</th>
<th style="text-align:right;">
DC
</th>
<th style="text-align:right;">
Juillands_D
</th>
<th style="text-align:right;">
DP
</th>
<th style="text-align:right;">
DP_norm
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
the
</td>
<td style="text-align:right;">
51046
</td>
<td style="text-align:right;">
50368.10
</td>
<td style="text-align:right;">
0.9886214
</td>
<td style="text-align:right;">
0.9643705
</td>
<td style="text-align:right;">
0.9675432
</td>
<td style="text-align:right;">
0.9629224
</td>
<td style="text-align:right;">
0.9817403
</td>
<td style="text-align:right;">
0.1429544
</td>
<td style="text-align:right;">
0.1430920
</td>
</tr>
<tr>
<td style="text-align:left;">
to
</td>
<td style="text-align:right;">
25942
</td>
<td style="text-align:right;">
25597.48
</td>
<td style="text-align:right;">
0.9937932
</td>
<td style="text-align:right;">
0.9812724
</td>
<td style="text-align:right;">
0.9841870
</td>
<td style="text-align:right;">
0.9768320
</td>
<td style="text-align:right;">
0.9876936
</td>
<td style="text-align:right;">
0.0883799
</td>
<td style="text-align:right;">
0.0884650
</td>
</tr>
<tr>
<td style="text-align:left;">
and
</td>
<td style="text-align:right;">
25344
</td>
<td style="text-align:right;">
25007.43
</td>
<td style="text-align:right;">
0.9902678
</td>
<td style="text-align:right;">
0.9687633
</td>
<td style="text-align:right;">
0.9732413
</td>
<td style="text-align:right;">
0.9670243
</td>
<td style="text-align:right;">
0.9832963
</td>
<td style="text-align:right;">
0.1251384
</td>
<td style="text-align:right;">
0.1252588
</td>
</tr>
<tr>
<td style="text-align:left;">
of
</td>
<td style="text-align:right;">
24331
</td>
<td style="text-align:right;">
24007.88
</td>
<td style="text-align:right;">
0.9830139
</td>
<td style="text-align:right;">
0.9460955
</td>
<td style="text-align:right;">
0.9490047
</td>
<td style="text-align:right;">
0.9465552
</td>
<td style="text-align:right;">
0.9769251
</td>
<td style="text-align:right;">
0.1772988
</td>
<td style="text-align:right;">
0.1774694
</td>
</tr>
<tr>
<td style="text-align:left;">
a
</td>
<td style="text-align:right;">
22694
</td>
<td style="text-align:right;">
22392.62
</td>
<td style="text-align:right;">
0.9922448
</td>
<td style="text-align:right;">
0.9772786
</td>
<td style="text-align:right;">
0.9792415
</td>
<td style="text-align:right;">
0.9727416
</td>
<td style="text-align:right;">
0.9858140
</td>
<td style="text-align:right;">
0.1066691
</td>
<td style="text-align:right;">
0.1067717
</td>
</tr>
<tr>
<td style="text-align:left;">
in
</td>
<td style="text-align:right;">
16959
</td>
<td style="text-align:right;">
16733.78
</td>
<td style="text-align:right;">
0.9878687
</td>
<td style="text-align:right;">
0.9619502
</td>
<td style="text-align:right;">
0.9642931
</td>
<td style="text-align:right;">
0.9611510
</td>
<td style="text-align:right;">
0.9805055
</td>
<td style="text-align:right;">
0.1462487
</td>
<td style="text-align:right;">
0.1463894
</td>
</tr>
</tbody>
</table>

## Dispersion statistics for a singe token

The `dispersions_token()` function calculates the dispersion measures for a single token. It returns a named list with all of the dispersion measures discussed by [S.T. Gries](http://www.stgries.info/research/dispersion/links.html) and the function is baesed on a script he originally authored.

``` r
a <- dispersions_token(sc_dfm, "data")
```

<table>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
Absolute frequency
</td>
<td style="text-align:right;">
199.0000000
</td>
</tr>
<tr>
<td style="text-align:left;">
Per_10.6
</td>
<td style="text-align:right;">
196.3572281
</td>
</tr>
<tr>
<td style="text-align:left;">
Relative entropy of all sizes of the corpus parts
</td>
<td style="text-align:right;">
0.9996961
</td>
</tr>
<tr>
<td style="text-align:left;">
Range
</td>
<td style="text-align:right;">
63.0000000
</td>
</tr>
<tr>
<td style="text-align:left;">
Maxmin
</td>
<td style="text-align:right;">
19.0000000
</td>
</tr>
<tr>
<td style="text-align:left;">
Standard deviation
</td>
<td style="text-align:right;">
1.8078064
</td>
</tr>
<tr>
<td style="text-align:left;">
Variation coefficient
</td>
<td style="text-align:right;">
3.6337818
</td>
</tr>
<tr>
<td style="text-align:left;">
Chi-square
</td>
<td style="text-align:right;">
2660.6578664
</td>
</tr>
<tr>
<td style="text-align:left;">
Juilland et al.’s D (based on equally-sized corpus parts)
</td>
<td style="text-align:right;">
0.8180834
</td>
</tr>
<tr>
<td style="text-align:left;">
Juilland et al.’s D (not requiring equally-sized corpus parts)
</td>
<td style="text-align:right;">
0.8183566
</td>
</tr>
<tr>
<td style="text-align:left;">
Carroll’s D2
</td>
<td style="text-align:right;">
0.6203020
</td>
</tr>
<tr>
<td style="text-align:left;">
Rosengren’s S (based on equally-sized corpus parts)
</td>
<td style="text-align:right;">
0.1279004
</td>
</tr>
<tr>
<td style="text-align:left;">
Rosengren’s S (not requiring equally-sized corpus parts)
</td>
<td style="text-align:right;">
0.1260951
</td>
</tr>
<tr>
<td style="text-align:left;">
Lyne’s D3 (not requiring equally-sized corpus parts)
</td>
<td style="text-align:right;">
-2.2928398
</td>
</tr>
<tr>
<td style="text-align:left;">
Distributional consistency DC
</td>
<td style="text-align:right;">
0.1279004
</td>
</tr>
<tr>
<td style="text-align:left;">
Inverse document frequency IDF
</td>
<td style="text-align:right;">
2.6665763
</td>
</tr>
<tr>
<td style="text-align:left;">
Engvall’s measure
</td>
<td style="text-align:right;">
31.3425000
</td>
</tr>
<tr>
<td style="text-align:left;">
Juilland et al.’s U (based on equally-sized corpus parts)
</td>
<td style="text-align:right;">
162.7985909
</td>
</tr>
<tr>
<td style="text-align:left;">
Juilland et al.’s U (not requiring equally-sized corpus parts)
</td>
<td style="text-align:right;">
162.8529550
</td>
</tr>
<tr>
<td style="text-align:left;">
Carroll’s Um (based on equally sized corpus parts)
</td>
<td style="text-align:right;">
123.6290008
</td>
</tr>
<tr>
<td style="text-align:left;">
Rosengren’s Adjusted Frequency (based on equally sized corpus parts)
</td>
<td style="text-align:right;">
25.4521852
</td>
</tr>
<tr>
<td style="text-align:left;">
Rosengren’s Adjusted Frequency (not requiring equally sized corpus
parts)
</td>
<td style="text-align:right;">
25.0929244
</td>
</tr>
<tr>
<td style="text-align:left;">
Kromer’s Ur
</td>
<td style="text-align:right;">
100.8473290
</td>
</tr>
<tr>
<td style="text-align:left;">
Deviation of proportions DP
</td>
<td style="text-align:right;">
0.8444693
</td>
</tr>
<tr>
<td style="text-align:left;">
Deviation of proportions DP (normalized)
</td>
<td style="text-align:right;">
0.8452817
</td>
</tr>
</tbody>
</table>

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
