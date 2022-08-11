# Introduction to pre-processing functions in the quanteda.extras R package

``` r
library(quanteda.extras)
library(tidyverse)
```

## Pre-processing

The `preprocess_text()` function takes the following logical
(TRUE/FALSE) arguments:

- **contractions** (if set to TRUE contractions will be separated so
  that, for example, *can’t* becomes *ca n’t*)
- **hyphens** (if set to TRUE hyphens will be replaced by spaces)
- **punctuation** (if set to TRUE all punctuation marks will be exluded)
- **lower_case** (if set to TRUE all strings are converted to lower
  case)
- **accent_replace** (if set to TRUE accented chacaracters will be
  replaced by unaccented ones)
- **remove_numers** (if set to TRUE strings made up of numbers will be
  eliminated)

### contractions:

``` r
a <- preprocess_text("can't won't we'll its' it's")
b <- preprocess_text("can't won't we'll its' it's", contractions = FALSE)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
TRUE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
ca n’t wo n’t we ll its it s
</td>
</tr>
</tbody>
</table>
<table>
<thead>
<tr>
<th style="text-align:left;">
FALSE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
can’t won’t we’ll its it’s
</td>
</tr>
</tbody>
</table>

### hyphens:

``` r
a <- preprocess_text("un-knowable bluish-gray slo-mo stop-")
b <- preprocess_text("un-knowable bluish-gray slo-mo stop-", hypens = FALSE)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
TRUE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
un knowable bluish gray slo mo stop
</td>
</tr>
</tbody>
</table>
<table>
<thead>
<tr>
<th style="text-align:left;">
FALSE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
un-knowable bluish-gray slo-mo stop
</td>
</tr>
</tbody>
</table>

### punctuation:

``` r
a <- preprocess_text("u.k. 50% 'cat' #great now?")
b <- preprocess_text("u.k. 50% 'cat' #great now?", punctuation = FALSE)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
TRUE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
u.k 50 cat great now
</td>
</tr>
</tbody>
</table>
<table>
<thead>
<tr>
<th style="text-align:left;">
FALSE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
u.k. 50% ‘cat’ #great now?
</td>
</tr>
</tbody>
</table>

### lower_case:

``` r
a <- preprocess_text("U.K. This A-1 1-A")
b <- preprocess_text("U.K. This A-1 1-A", lower_case = FALSE)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
TRUE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
u.k this a 1 1 a
</td>
</tr>
</tbody>
</table>
<table>
<thead>
<tr>
<th style="text-align:left;">
FALSE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
U.K This A 1 1 A
</td>
</tr>
</tbody>
</table>

### accent_replace:

``` r
a <- preprocess_text("fiancée naïve façade")
b <- preprocess_text("fiancée naïve façade", accent_replace = FALSE)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
TRUE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
fiancee naive facade
</td>
</tr>
</tbody>
</table>
<table>
<thead>
<tr>
<th style="text-align:left;">
FALSE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
fiancée naïve façade
</td>
</tr>
</tbody>
</table>

### remove_numbers:

``` r
a <- preprocess_text("a-1 b2 50% 99 10,000", remove_numbers = TRUE)
b <- preprocess_text("a-1 50% 99 10,000")
```

<table>
<thead>
<tr>
<th style="text-align:left;">
TRUE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
a b2
</td>
</tr>
</tbody>
</table>
<table>
<thead>
<tr>
<th style="text-align:left;">
FALSE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
a 1 50 99 10,000
</td>
</tr>
</tbody>
</table>

Note that these options represent some procedures that are common when
“cleaning” texts. They give additional control over how a corpus is
later “tokenized”. These are *not* intended to be comprehensive.
Depending on one’s data there may be other, specific ways a corpus needs
to be processed prior to tokenizing.

The [**textclean**](https://github.com/trinker/textclean) package offers
a host of options for pre-processing tasks. In addition, the [**tokens(
)**](http://quanteda.io/reference/tokens.html) function in **quanteda**
has a variety of built-in options, some similar to the ones described
above.

And, of course, one can use either native R **gsub( )** or
[**stringr**](https://stringr.tidyverse.org/) **tidyverse** to create
task-specific text processing functions.

## Sample corpus

The package comes with a small corpus – the **sample_corpus**. The
corpus contains data from 8 text-types:

- Academic
- Blog
- Fiction
- Magazine
- News
- Spoken
- Television & Movie
- Web

In this way, it resembles the [Corpus of Contemporary American
English](https://www.english-corpora.org/coca/). However, it contains
only 50 texts from each type, and each text is only about 2,500 words.
Thus, it is similar to the [Brown family of
corpora](https://www1.essex.ac.uk/linguistics/external/clmt/w3c/corpus_ling/content/corpora/list/private/brown/brown.html)
in its size (roughly 1 million words).

Note that this data is included *only* for demonstration purposes. It
was *not* compiled to be used for research.

``` r
sc <- sample_corpus %>%
  mutate(text_type = str_extract(doc_id, "[a-z]+")) %>%
  group_by(text_type) %>%
  tally()
```

<table>
<thead>
<tr>
<th style="text-align:left;">
text_type
</th>
<th style="text-align:right;">
n
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
acad
</td>
<td style="text-align:right;">
50
</td>
</tr>
<tr>
<td style="text-align:left;">
blog
</td>
<td style="text-align:right;">
50
</td>
</tr>
<tr>
<td style="text-align:left;">
fic
</td>
<td style="text-align:right;">
50
</td>
</tr>
<tr>
<td style="text-align:left;">
mag
</td>
<td style="text-align:right;">
50
</td>
</tr>
<tr>
<td style="text-align:left;">
news
</td>
<td style="text-align:right;">
50
</td>
</tr>
<tr>
<td style="text-align:left;">
spok
</td>
<td style="text-align:right;">
50
</td>
</tr>
<tr>
<td style="text-align:left;">
tvm
</td>
<td style="text-align:right;">
50
</td>
</tr>
<tr>
<td style="text-align:left;">
web
</td>
<td style="text-align:right;">
50
</td>
</tr>
</tbody>
</table>
