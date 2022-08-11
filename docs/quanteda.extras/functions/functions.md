# quanteda.extras functions

## `ARF`

Average reduced frequencies for all tokens in a corpus


### Description

ARF calculates average reduced frequency, which combines dispersion and frequency into a single measure. It does this by de-emphasizing occurrences that appear clustered in close proximity.


### Usage

```r
ARF(target_tkns)
```


### Arguments

Argument      |Description
------------- |----------------
`target_tkns`     |     The target quanteda tokens object.


### Value

A data.frame containing average reduced frequency for each token.


## `col_network`

Create a table for plotting a collocational network


### Description

This function operationalizes the idea of collcational networks described by Brezina, McEnery & Wattam (2015): https://www.jbe-platform.com/content/journals/10.1075/ijcl.20.2.01bre The function takes data.frames produced by the collocates_by_MI() function above and generates a tidygraph data object for plotting in ggraph.


### Usage

```r
col_network(col_1, ...)
```


### Arguments

Argument      |Description
------------- |----------------
`col_1`     |     A collocations object produced by collocates_by_MI().
`...`     |     Other collocations objects for plotting.


### Value

A tidygraph table for network plotting.


## `collocates_by_MI`

Calculate collocational associations by Mutual Information


### Description

A function for calculating point-wise mutual information from quanteda tokens.
 The function requires:
  

*  a tokens object 

*  a node word to search for 

*  a window counting to the left 

*  a window counting to the right So collocates_by_MI(my_tokens, "test", 5, 5) would look for collocates words to the left and 5 words to the right of the word "test".


### Usage

```r
collocates_by_MI(target_tkns, node_word, left = 5, right = 5)
```


### Arguments

Argument      |Description
------------- |----------------
`target_tkns`     |     The target quanteda tokens object.
`node_word`     |     The token of interest.
`left`     |     A numeric value indicating how many words a span should extend to the left of the node word.
`right`     |     A numeric value indicating how many words a span should extend to the right of the node word


### Value

A data.frame containing absolute frequencies and Mutual Information calculations.

## `dispersions_all`

Dispersion measures for all tokens in a corpus


### Description

The dispersions_all() function calculates a subset of of the most common dispersion measures
 for all of the tokens in a document-feature matrix and returns a data.frame.
 For example: dispersions_all(my_dfm)


### Usage

```r
dispersions_all(target_dfm)
```


### Arguments

Argument      |Description
------------- |----------------
`target_dfm`     |     The target document-feature matrix.


### Value

A data.frame containing dispersion measures for the tokens in the document-feature matrix.

## `dispersions_token`

Dispersion measures for a token


### Description

The dispersions_token() function calculates the dispersion measures for a single token. For example: dispersions_token (my_dfm, "cat") It returns a named list with all of the [dispersion measures discussed by S.T. Gries](http://www.stgries.info/research/dispersion/links.html).
### Usage

```r
dispersions_token(target_dfm, token)
```


### Arguments

Argument      |Description
------------- |----------------
`target_dfm`     |     The target document-feature matrix.
`token`     |     The token for which dispersion measures are to be calculated.


### Value

A named list of dispersion measures.


## `excel_style`

A function for expanding letter sequences.


### Description

A function for expanding letter sequences.


### Usage

```r
excel_style(i)
```


### Arguments

Argument      |Description
------------- |----------------
`i`     |     Index of alphabetic character


### Value

A vector of character combinations in the style of Excel column headers

## `frequency_table`

Descriptive measures for all tokens in a corpus


### Description

The frequency_table() function aggregates useful descriptive measures: absolute frequency, relative frequency, average reduced frequency, and deviation of proportions.


### Usage

```r
frequency_table(target_tkns)
```


### Arguments

Argument      |Description
------------- |----------------
`target_tkns`     |     The target quanteda tokens object.


### Value

A data.frame containing absolute frequency, relative frequency, average reduced frequency, and deviation of proportions.

## `key_keys`

Key of keys calculation


### Description

The following function is based on an idea proposed by Mike Scott and used in [his concordancer WordSmith](https://lexically.net/downloads/version4/html/index.html?database_info.htm). Rather than summing counts from all texts in the target corpus and comparing them to those in a reference corpus, Scott proposes to iterate through each text in the target corpus, calculating keyness values against the reference corpus. Then you find how many texts reach some significance threshold. Essentially, this is a way of accounting for distribution: Are a few texts driving keyness values? Or many? The function returns a data.frame that includes:
  

*  the percent of texts in the target corpus for which keyness reaches the specified threshold;
*  the mean keyness value in the target;
*  the standard deviation of keyness;
*  the mean effect size by log ratio Note that it is easy enough to alter the function to return other values.

### Usage

```r
key_keys(
  target_dfm,
  reference_dfm,
  threshold = c(0.05, 0.01, 0.001, 1e-04),
  yates = FALSE
)
```

### Arguments

Argument      |Description
------------- |----------------
`target_dfm`     |     The target document-feature matrix
`reference_dfm`     |     The reference document-feature matrix
`threshold`     |     The p-value threshold for calculating percentage of documents reaching significance
`yates`     |     A logical value indicating whether the "Yates" correction should be performed


### Value

A data.frame containing the percentage of documents reaching significance, mean keyness, and mean effect size


## `keyness_pairs`

Pairwise keyness values from any number of dfms.


### Description

This function takes any number of quanteda dfm objects and returns a table of log-likelihood values, effect sizes using Hardie's log ratio and p-values.


### Usage

```r
keyness_pairs(dfm_a, dfm_b, ..., yates = FALSE)
```


### Arguments

Argument      |Description
------------- |----------------
`dfm_a`     |     A document-feature matrix
`dfm_b`     |     A document-feature matrix
`...`     |     Additional document-feature matrices
`yates`     |     A logical value indicating whether the "Yates" correction should be performed


### Value

A data.frame containing pairwise keyness comparisons of all dfms

## `keyness_table`

Keyness measures for all tokens in a corpus


### Description

The keyness_table() function returns the log-likelihood of the target vs. reference corpus, effect sizes by log ratio, p-values, absolute frequencies, relative frequencies, and deviation of proportions.


### Usage

```r
keyness_table(target_dfm, reference_dfm, yates = FALSE)
```


### Arguments

Argument      |Description
------------- |----------------
`target_dfm`     |     The target document-feature matrix
`reference_dfm`     |     The reference document-feature matrix
`yates`     |     A logical value indicating whether the "Yates" correction should be performed


### Value

A data.frame containing the log-likelihood, log ratio, absolute frequencies, relative frequencies, and dispersions


## `log_like`

Log-likelihood calculation


### Description

Log-likelihood tests the frequencies of tokens in one corpus vs. another. It is often used instead of a chi-square test, as it has been shown to be more resistant to corpora of varying sizes. For more detail see: [http://ucrel.lancs.ac.uk/llwizard.html](http://ucrel.lancs.ac.uk/llwizard.html).


### Usage

```r
log_like(n_target, n_reference, total_target, total_reference, correct = FALSE)
```


### Arguments

Argument      |Description
------------- |----------------
`n_target`     |     The raw (non-normalized) token count in the target corpus
`n_reference`     |     The raw (non-normalized) token count in the reference corpus
`total_target`     |     The total number of tokens in the target corpus
`total_reference`     |     The total number of tokens in the reference corpus


### Value

A numeric value representing log-likelihood


## `log_ratio`

Log-ratio calculation


### Description

Take a target column and a reference column, and return an effect size This effect size calculation is called Log Ratio And was proposed by Andrew Hardie: [http://cass.lancs.ac.uk/log-ratio-an-informal-introduction/](http://cass.lancs.ac.uk/log-ratio-an-informal-introduction/).


### Usage

```r
log_ratio(n_target, n_reference, total_target, total_reference)
```


### Arguments

Argument      |Description
------------- |----------------
`n_target`     |     The raw (non-normalized) token count in the target corpus
`n_reference`     |     The raw (non-normalized) token count in the reference corpus
`total_target`     |     The total number of tokens in the target corpus
`total_reference`     |     The total number of tokens in the reference corpus


### Value

A numeric value representing the log ratio


## `normalizing_factor`

A function for detecting the size of a corpus and setting the narmalizing factor to the nearest power of 10.


### Description

A function for detecting the size of a corpus and setting the narmalizing factor to the nearest power of 10.


### Usage

```r
normalizing_factor(x)
```

### Arguments

Argument      |Description
------------- |----------------
`corpus_total`     |     The total number of words in the corpus


### Value

A named vector


## `preprocess_text`

Pre-process texts


### Description

A simple function that requires a readtext object. It then processes the text column using basic regex substitutions. The default is to add a space before possessives and contractions. This will force their tokenization in quanteda. So that "Shakespeare's" will be counted as two tokens rather than a single one. It is easy to add or delete substations as fits your analytical needs.


### Usage

```r
preprocess_text(
  txt,
  contractions = TRUE,
  hypens = TRUE,
  punctuation = TRUE,
  lower_case = TRUE,
  accent_replace = TRUE,
  remove_numbers = FALSE
)
```


### Arguments

Argument      |Description
------------- |----------------
`txt`     |     A character vector
`contractions`     |     A logical value to separate contractions into two tokens
`hypens`     |     A logical value to separate hypenated words into two tokens
`punctuation`     |     A logical value to remove punctuation
`lower_case`     |     A logical value to make all tokens lower case
`accent_replace`     |     A logical value to replace accented characters with un-accented ones
`remove_numbers`     |     A logical value to remove numbers


### Value

A character vector


## `readtext_lite`

Read texts from a vector of paths.


### Description

Replaces the readtext::readtext function that reads a lists of text files into a data frame.


### Usage

```r
readtext_lite(paths)
```

### Arguments

Argument      |Description
------------- |----------------
`paths`     |     A vector of paths to text files that are to be read in.


### Value

A readtext data.frame







