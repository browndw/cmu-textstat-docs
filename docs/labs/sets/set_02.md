# Set 02

The goal of this first lab set is to introduce:

* How to produce and interpret token frequencies
* How to produce and interpret token distributions
* Some basic principles of token distributions in a corpus
* 
## Lab 4: Distributions

### Prepare a corpus

#### Load the needed packages

``` r
library(cmu.textstat)
library(tidyverse)
library(quanteda)
library(quanteda.textstats)
```

#### Load a corpus

The **cmu.textstat** package comes with some data sets including a data table with a column of document ids and a column of texts. Such a table is easy to create from text data on your own local drive.

To do so, you would organize plain .txt files into a directory and use the `readtext()` function from the **[readtext](https://readtext.quanteda.io/articles/readtext_vignette.html)** package.

``` r
sc_df <- sample_corpus
```

To peek at the data, we’ll look at the first 100 characters in the `text` column of the first row:

|                                                                                                      |
|:-----------------------------------------------------------------------------------------------------|
| Teachers and other school personnel are often counseled to use research findings in making curricula |

Make a corpus object.

``` r
sc <- corpus(sc_df)
```

And check the result:

| Text    | Types | Tokens | Sentences |
|:--------|------:|-------:|----------:|
| acad_01 |   842 |   2816 |        95 |
| acad_02 |   983 |   2829 |        88 |
| acad_03 |   969 |   2887 |       126 |
| acad_04 |  1017 |   2860 |       102 |
| acad_05 |   914 |   2834 |       109 |
| acad_06 |  1007 |   2813 |        86 |

#### Document variables (Name your files systematically!)

Note the names of the text files encode important meta-data: in this case, the names of text types similar to the Corpus of Contemporary American English.

This is **extremely important**. When you build your own corpora, yo want to purposefully and systematically name your files and organize your directories. This will save you time and effort later in your analysis.

We are now going to extract the meta-data from the file names and pass them as a variable.

``` r
doc_categories <- str_extract(sc_df$doc_id, "^[a-z]+")
```

Check the result:

|      |
|:-----|
| acad |
| blog |
| fic  |
| mag  |
| news |
| spok |
| tvm  |
| web  |


We will now assign the variable to the corpus. The following command might look backwards, with the function on the left hand side of the `<-` operator. That is because it’s an accessor function, which lets us add or modify data in an object. You can tell when a function is an accessor function like this because its help file will show that you can use it with `<-`, for example in `?docvars`.

``` r
docvars(sc, field = "text_type") <- doc_categories
```

And check the summary again:

| Text    | Types | Tokens | Sentences | text_type |
|:--------|------:|-------:|----------:|:----------|
| acad_01 |   842 |   2816 |        95 | acad      |
| acad_02 |   983 |   2829 |        88 | acad      |
| acad_03 |   969 |   2887 |       126 | acad      |
| acad_04 |  1017 |   2860 |       102 | acad      |
| acad_05 |   914 |   2834 |       109 | acad      |
| acad_06 |  1007 |   2813 |        86 | acad      |


### Tokenize the corpus

We’ll use **[quanteda](http://quanteda.io/)** to tokenize. And after tokenization, we’ll convert them to lower case. *Why do that here?* As a next step, we’ll being combining tokens like *a* and *lot* into single units. And we’ll be using a list of expressions that isn’t case sensitive.

``` r
sc_tokens <- tokens(sc, include_docvars = TRUE, remove_punct = TRUE, remove_numbers = TRUE,
    remove_symbols = TRUE, what = "word")

sc_tokens <- tokens_tolower(sc_tokens)
```

### Multi-word Expressions

An issue that we run into frequently with corpus analysis is what to do with multi-word expressions. For example, consider a common English quantifier: “a lot”. Typical tokenization rules will split this into two tokens: *a* and *lot*. But counting *a lot* as a single unit might be important depending on our task. We have a way of telling **quanteda**
to account for these tokens.

All that we need is a list of multi-word expressions.

The **cmu.textstat** comes with an example of an mwe list called
`multiword_expressions`:

|                   |
|:------------------|
| winter haven      |
| with a view to    |
| with reference to |
| with regard to    |
| with relation to  |
| with respect to   |


The `tokens_compound()` function looks for token sequences that match our list and combines them using an underscore.

``` r
sc_tokens <- tokens_compound(sc_tokens, pattern = phrase(multiword_expressions))
```

### Create a document-feature matrix

With our tokens object we can now create a document-feature-matrix using the `dfm()` function. As a reminder, a **[dfm](http://quanteda.io/reference/dfm.html)** is table with one row per document in the corpus, and one column per unique token in the corpus. Each cell contains a count of how many times a token shows up in that document.

``` r
sc_dfm <- dfm(sc_tokens)
```

Next we’ll create a **http://quanteda.io/reference/dfm_weight.html** with proportionally weighted counts.

``` r
prop_dfm <- dfm_weight(sc_dfm, scheme = "prop")
```

### Token distributions

#### Distributions of *the*

Let’s start by selecting frequencies of the most common token in the corpus:

``` r
freq_df <- textstat_frequency(sc_dfm) %>%
    data.frame(stringsAsFactors = F)
```

| feature | frequency | rank | docfreq | group |
|:--------|----------:|-----:|--------:|:------|
| the     |     50910 |    1 |     399 | all   |
| and     |     25222 |    2 |     398 | all   |
| to      |     24749 |    3 |     397 | all   |
| of      |     22057 |    4 |     399 | all   |
| a       |     21605 |    5 |     398 | all   |
| in      |     15964 |    6 |     399 | all   |
| i       |     12560 |    7 |     348 | all   |
| that    |     12529 |    8 |     396 | all   |
| you     |     10948 |    9 |     341 | all   |
| is      |      9895 |   10 |     389 | all   |


From the weighted **dfm**, we can select any token that we’d like to look at more closely. In this case, we’ll select the most frequent token: *the*.

After selecting the variable, we will convert the data into a more friendly data structure.

There are easier ways of doing this, but the first bit of the code-chunk allows us to filter by rank and return a character vector that we can pass. This way, we can find a word of any arbitrary rank.

Also note how the `rename()` function is set up. Let’s say our token is *the*. The `dfm_select()` function would result with a column named `the` that we’d want to rename `RF`. So our typical syntax would be: `rename(RF = the)`. In the chunk below, however, our column name is the variable `word`. To pass that variable to `rename`, we use `!!name(word)`.

``` r
word <- freq_df %>%
    filter(rank == 1) %>%
    dplyr::select(feature) %>%
    as.character()

word_df <- dfm_select(prop_dfm, word, valuetype = "fixed")  # select the token

word_df <- word_df %>%
    convert(to = "data.frame") %>%
    cbind(docvars(word_df)) %>%
    rename(RF = !!as.name(word)) %>%
    mutate(RF = RF * 1e+06)
```

With that data it is a simple matter to generate basic summary
statistics using the `group_by()` function:

``` r
summary_table <- word_df %>%
    group_by(text_type) %>%
    summarize(MEAN = mean(RF), SD = sd(RF), N = n())
```

| text_type |     MEAN |       SD |   N |
|:----------|---------:|---------:|----:|
| acad      | 68619.15 | 18327.65 |  50 |
| blog      | 51268.52 | 13391.02 |  50 |
| fic       | 54201.50 | 14254.59 |  50 |
| mag       | 57086.29 | 14007.56 |  50 |
| news      | 50482.34 | 18485.42 |  50 |
| spok      | 42812.15 |  9787.92 |  50 |
| tvm       | 32532.85 | 11981.88 |  50 |
| web       | 60600.51 | 21646.94 |  50 |

And we can inspect a histogram of the frequencies. To set the width of our bins we’ll use the Freedman-Diaconis rule. The bin-width is set to:

```{math}
h = 2 x \frac{IQR(x)}{n^{1/3}
}
```

```{note}
[Other popular methods](http://math.furman.edu/~dcs/courses/math47/R/library/car/html/n.bins.html) for calculating optimal bin width include "Scott's rule".
```

So the number of bins is `(max-min)/h`, where `n` is the number of observations, `max` is the maximum value and `min` is the minimum value.

``` r
bin_width <- function(x) {
    2 * IQR(x)/length(x)^(1/3)
}
```

Now we can plot a histogram. We’re also adding a dashed line showing the mean. Note we’re also going to use the **[scales](https://scales.r-lib.org/)** package to remove scientific notation from our tick labels.

``` r
ggplot(word_df, aes(RF)) + geom_histogram(binwidth = bin_width(word_df$RF),
    colour = "black", fill = "white", size = 0.25) + geom_vline(aes(xintercept = mean(RF)),
    color = "red", linetype = "dashed", size = 0.5) + theme_classic() +
    scale_x_continuous(labels = scales::comma) + xlab("RF (per mil. words)")
```

![Histogram of the token
.](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/labs_files/figure-gfm/the_histogram-1.png)

### Distributions of *the* and *of*

Now let’s try plotting histograms of two tokens on the same plot. First we’re going to use regular expressions (`^the$|^of$`) to select the columns. The carat or hat `^` looks for the start of line. Without it, we would also get words like *blather*. The dollar symbol `$` looks for the end of a line. The straight line `|` means "or". Think about how useful this flexibility can be. You could, for example, extract all words that end
in *-ion*.

``` r
# Note 'regex' rather than 'fixed'
word_df <- dfm_select(prop_dfm, "^the$|^of$", valuetype = "regex")

# Now we'll convert our selection and normalize to 10000 words.
word_df <- word_df %>%
    convert(to = "data.frame") %>%
    mutate(the = the * 10000) %>%
    mutate(of = of * 10000)

# Use 'pivot_longer' to go from a wide format to a long one
word_df <- word_df %>%
    pivot_longer(!doc_id, names_to = "token", values_to = "RF") %>%
    mutate(token = factor(token))
```

Now let’s make a new histogram. Here we assign the values of color and fill to the `token` column. We also make the columns a little transparent using the `alpha` setting.

``` r
ggplot(word_df, aes(x = RF, color = token, fill = token)) + geom_histogram(binwidth = bin_width(word_df$RF),
    alpha = 0.5, position = "identity") + theme_classic() + xlab("RF (per mil. words)") +
    theme(axis.text = element_text(size = 5))
```

![Histogram of the tokens and
.](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/labs_files/figure-gfm/unnamed-chunk-14-1.png)

If we don’t want overlapping histograms, we can use `facet_wrap()` to split the plots.

``` r
ggplot(word_df, aes(x = RF, color = token, fill = token)) + geom_histogram(binwidth = bin_width(word_df$RF),
    alpha = 0.5, position = "identity") + theme_classic() + theme(axis.text = element_text(size = 5)) +
    theme(legend.position = "none") + xlab("RF (per mil. words)") + facet_wrap(~token)
```

![Histogram of the tokens and
.](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/labs_files/figure-gfm/unnamed-chunk-15-1.png)


### Dispersion

We can also calculate dispersion, and there are a variety of measures at our disposal. Our toolkit has several functions for producing these calculations.

For example, we can find the dispersion of any specific token:

``` r
the <- dispersions_token(sc_dfm, "the") %>%
    unlist()
```

|                                                                           |           |
|:--------------------------------------------------------------------------|----------:|
| Absolute frequency                                                        | 50910.000 |
| Per_10.5                                                                  |  5239.482 |
| Relative entropy of all sizes of the corpus parts                         |     1.000 |
| Range                                                                     |   399.000 |
| Maxmin                                                                    |   292.000 |
| Standard deviation                                                        |    45.937 |
| Variation coefficient                                                     |     0.361 |
| Chi-square                                                                |  6310.987 |
| Juilland et al.’s D (based on equally-sized corpus parts)                 |     0.982 |
| Juilland et al.’s D (not requiring equally-sized corpus parts)            |     0.982 |
| Carroll’s D2                                                              |     0.989 |
| Rosengren’s S (based on equally-sized corpus parts)                       |     0.963 |
| Rosengren’s S (not requiring equally-sized corpus parts)                  |     0.966 |
| Lyne’s D3 (not requiring equally-sized corpus parts)                      |     0.968 |
| Distributional consistency DC                                             |     0.963 |
| Inverse document frequency IDF                                            |     0.004 |
| Engvall’s measure                                                         | 50782.725 |
| Juilland et al.’s U (based on equally-sized corpus parts)                 | 49990.109 |
| Juilland et al.’s U (not requiring equally-sized corpus parts)            | 50000.041 |
| Carroll’s Um (based on equally sized corpus parts)                        | 50331.945 |
| Rosengren’s Adjusted Frequency (based on equally sized corpus parts)      | 49022.075 |
| Rosengren’s Adjusted Frequency (not requiring equally sized corpus parts) | 49167.056 |
| Kromer’s Ur                                                               |  2135.467 |
| Deviation of proportions DP                                               |     0.139 |
| Deviation of proportions DP (normalized)                                  |     0.139 |


And let’s try another token to compare:

``` r
data <- dispersions_token(sc_dfm, "data") %>%
    unlist()
```

|                             |   the |  data |
|-----------------------------|------:|------:|
| Deviation of proportions DP | 0.139 | 0.846 |


#### Dispersions for all tokens

We can also calculate selected dispersion measures for all tokens using `dispersions_all()`:

``` r
d <- dispersions_all(sc_dfm)
```

| Token |    AF | Per_10.5 | Carrolls_D2 | Rosengrens_S | Lynes_D3 |    DC | Juillands_D |    DP | DP_norm |
|:------|------:|---------:|------------:|-------------:|---------:|------:|------------:|------:|--------:|
| the   | 50910 | 5239.482 |       0.989 |        0.966 |    0.968 | 0.963 |       0.982 | 0.139 |   0.139 |
| and   | 25222 | 2595.761 |       0.990 |        0.969 |    0.973 | 0.967 |       0.984 | 0.124 |   0.124 |
| to    | 24749 | 2547.082 |       0.993 |        0.980 |    0.983 | 0.976 |       0.987 | 0.090 |   0.090 |
| of    | 22057 | 2270.030 |       0.978 |        0.933 |    0.935 | 0.932 |       0.974 | 0.199 |   0.200 |
| a     | 21605 | 2223.512 |       0.992 |        0.976 |    0.978 | 0.973 |       0.986 | 0.109 |   0.110 |
| in    | 15964 | 1642.960 |       0.988 |        0.962 |    0.963 | 0.960 |       0.981 | 0.146 |   0.146 |


#### Generating a frequency table

Alternatively, `frequency_table()` returns only Deviation of
Proportions and Average Reduced Frequency.

Note that ARF requires a tokens object and takes a couple of minutes to calculate.

``` r
ft <- frequency_table(sc_tokens)
```

|     | Token |    AF | Per_10.5 |       ARF |    DP |
|-----|:------|------:|---------:|----------:|------:|
| 1   | the   | 50910 | 5239.482 | 31893.812 | 0.139 |
| 2   | and   | 25222 | 2595.761 | 15905.994 | 0.124 |
| 3   | to    | 24749 | 2547.082 | 15464.035 | 0.090 |
| 5   | of    | 22057 | 2270.030 | 13086.919 | 0.199 |
| 4   | a     | 21605 | 2223.512 | 13233.050 | 0.109 |
| 6   | in    | 15964 | 1642.960 |  9766.973 | 0.146 |

```{note}
In addition to a dfm of normalized frequencies (like we did above), we can create [a term frequency-inverse document frequency](https://towardsdatascience.com/tf-term-frequency-idf-inverse-document-frequency-from-scratch-in-python-6c2b61b78558) (tf-idf) matrix using the `dfm_tfidf()` [function](http://quanteda.io/reference/dfm_tfidf.html).

A tf-idf is a popular weighting scheme (particularly in text classificaation tasks) that attempts to account for both token frequency and dispersion. A tf–idf value increases proportionally according to the number of times a token appears in the document and is offset by the number of documents in the corpus that contain the token.
```

### Zipf’s Law

Let’s plot rank (along the x-axis) against frequency (along the y-axis) for the 100 most frequent tokens in the sample corpus.

``` r
ggplot(freq_df %>%
    filter(rank < 101), aes(x = rank, y = frequency)) + geom_point(shape = 1,
    alpha = 0.5) + theme_classic() + ylab("Absolute frequency") + xlab("Rank")
```

![Token rank
vs. frequency.](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/labs_files/figure-gfm/unnamed-chunk-22-1.png)

The relationship you’re seeing between the rank of a token and it’s frequency holds true for almost any corpus and is referred to as **[Zipf’s Law](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4176592/)** (see Brezina pg. 44).


## Lab 5: Collocations

This is a short lab that introduces the concept of:

-   collocations,
-   how to calculate word association measures like pointwise mutual
    information, and
-   how to plot collocational networks.

This lab will also cover the process of reading in a corpus from a
directory of text files.

### Load the needed packages

``` r
library(cmu.textstat)
library(tidyverse)
library(quanteda)
library(quanteda.textstats)
library(ggraph)
```

### Prepare the data

First, we’ll pre-process our text, create a corpus and tokenize the
data:

``` r
sc_tokens <- sample_corpus %>%
  mutate(text = preprocess_text(text)) %>%
  corpus() %>%
  tokens(what="fastestword", remove_numbers=TRUE)
```

### Collocates by mutual information (MI)

The `collocates_by_MI()` [function](https://cmu-textstat-docs.readthedocs.io/en/latest/quanteda.extras/functions/functions.html#collocates-by-mi) produces collocation measures (by [pointwise mutual information](https://en.wikipedia.org/wiki/Pointwise_mutual_information)) for a specified token in a **quanteda tokens** object. In addition to a token, a span or window (as given by a number of words to the **left** and **right** of the **node word**) is required. The default is 5 to the left and 5 to the right.

The formula for calculating MI is as follows:


```{math}
log_{2} \frac{O_{11}}{E_{11}}
```

Where *O<sub>11</sub>* and *E<sub>11</sub>* are the observed (i.e.,
node + collocate) and expected frequencies of the node word within a
given window. The expected frequency is given by:

```{math}
E_{11} = \frac{R_{1} \times C_{1}}{N}
```

-   *N* is the number of words in the corpus
-   *R<sub>1</sub>* is the frequency of the node in the whole corpus
-   *C<sub>1</sub>* is the frequency of the collocate in the whole
    corpus

We’ll start by making a table of tokens that collocate with the token
*money*.

``` r
money_collocations <- collocates_by_MI(sc_tokens, "money")
```

Check the result:

| token         | col_freq | total_freq |  MI_1 |
|:--------------|---------:|-----------:|------:|
| 10:29         |        1 |          1 | 11.08 |
| 38th          |        1 |          1 | 11.08 |
| allocations   |        1 |          1 | 11.08 |
| americanizing |        1 |          1 | 11.08 |
| anthedon      |        1 |          1 | 11.08 |
| assignats     |        1 |          1 | 11.08 |

Now, let’s make a similar table for collocates of *time*.

``` r
time_collocations <- collocates_by_MI(sc_tokens, "time")
```

| token      | col_freq | total_freq |   MI_1 |
|:-----------|---------:|-----------:|-------:|
| decleat    |        2 |          1 | 10.135 |
| poignantly |        2 |          1 | 10.135 |
| 16a        |        1 |          1 |  9.135 |
| 17a        |        1 |          1 |  9.135 |
| 21h        |        1 |          1 |  9.135 |
| aba        |        1 |          1 |  9.135 |

As is clear from the above table, MI is sensitive to rare/infrequent
words (see Brezina pg. 74). Because of that sensitivity, it commmon to make thresholds for both token frequency (absolute frequency) and MI score (usually at some value ≥ 3).

For our purposes, we’ll filter for AF ≥ 5 and MI ≥ 5.

``` r
tc <- time_collocations %>% filter(col_freq >= 5 & MI_1 >= 5)
mc <- money_collocations %>% filter(col_freq >= 5 & MI_1 >= 5)
```

Check the result:

| token       | col_freq | total_freq |  MI_1 |
|:------------|---------:|-----------:|------:|
| warner      |        6 |          8 | 8.720 |
| cessation   |        5 |          7 | 8.650 |
| irradiation |        5 |          7 | 8.650 |
| lag         |        5 |          7 | 8.650 |
| wasting     |        7 |         11 | 8.483 |
| frame       |        7 |         16 | 7.943 |

| token     | col_freq | total_freq |  MI_1 |
|:----------|---------:|-----------:|------:|
| owe       |        5 |         21 | 9.010 |
| raise     |       10 |         79 | 8.099 |
| extra     |        6 |         64 | 7.665 |
| spend     |       10 |        111 | 7.608 |
| insurance |        5 |         64 | 7.402 |
| spent     |        9 |        122 | 7.320 |


### Create a tbl_graph object for plotting

A `tbl_graph` is [a data structure](https://www.data-imaginist.com/2017/introducing-tidygraph/) for **tidyverse** (ggplot2) network plotting.

For this, we’ll use the `col_network()` [function](https://cmu-textstat-docs.readthedocs.io/en/latest/quanteda.extras/functions/functions.html#col-network).

``` r
net <- col_network(tc, mc)
```

### Plot network

The network plot shows the tokens that distinctly collocate with either
*time* or *money*, as well as those that intersect. The distance from
the central tokens (*time* and *money*) is governed by the MI score and
the transparency (or alpha) is governed by the token frequency.

The aesthetic details of the plot can be manipulated in the various
**[ggraph](https://ggraph.data-imaginist.com/)** options.

``` r
ggraph(net, weight = link_weight, layout = "stress") + 
  geom_edge_link(color = "gray80", alpha = .75) + 
  geom_node_point(aes(alpha = node_weight, size = 3, color = n_intersects)) +
  geom_node_text(aes(label = label), repel = T, size = 3) +
  scale_alpha(range = c(0.2, 0.9)) +
  theme_graph() +
  theme(legend.position="none")
```

![](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/labs_files/figure-gfm/net_plot-1.png)<!-- -->


### Reading in local files

#### Create a vector of file paths

First, go to Canvas and dowload the `screenplay_corpus` (in the Data
folder under Files). Unzip the corpus and note/copy the path to the
folder.

Next, we’ll create a vector of the file paths. Remember to replace
`your/path` with the place-holder path in the `list.files()`
function.

``` r
files_list <- list.files("your/path", full.names = T, pattern = "*.txt")
```

#### Read in files using readtext

Next, we’ll read in the files using **[readtext](https://readtext.quanteda.io/reference/readtext.html)**. And for the purposes of efficiency, we’ll sample out 50 files from the paths vector.

``` r
set.seed(1234)

sp <- sample(files_list, 50) %>%
  readtext::readtext()
```

#### Extract the dialogue

These particular files are formatted using some simple markup. So we’ll
use the `from_play()` [function](https://cmu-textstat-docs.readthedocs.io/en/latest/functions/functions.html#from-play) to extract the dialogue.

``` r
sp <- from_play(sp, extract = "dialogue")
```

#### Tokenize

``` r
sp <-   sp %>%
  mutate(text = preprocess_text(text)) %>%
  corpus() %>%
  tokens(what="fastestword", remove_numbers=TRUE)
```

#### Calculate MI

Now we’ll calculate collocations for the tokens *boy* and *girl*, and filter. Note that we’re only looking for tokens 3 words to the left of the node word.

``` r
b <- collocates_by_MI(sp, "boy", left = 3, right = 0)
b <- b %>% filter(col_freq >= 3 & MI_1 >= 3)

g  <- collocates_by_MI(sp, "girl", left = 3, right = 0)
g <- g %>% filter(col_freq >= 3 & MI_1 >= 3)
```

#### Plot the network

``` r
net <- col_network(b, g)

ggraph(net, weight = link_weight, layout = "stress") + 
  geom_edge_link(color = "gray80", alpha = .75) + 
  geom_node_point(aes(alpha = node_weight, size = 3, color = n_intersects)) +
  geom_node_text(aes(label = label), repel = T, size = 3) +
  scale_alpha(range = c(0.2, 0.9)) +
  theme_graph() +
  theme(legend.position="none")
```

![](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/labs_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

See other examples [here](https://www.sciencedirect.com/science/article/pii/S1475158518302169)and [here](https://link.springer.com/chapter/10.1007/978-3-319-92582-0_4).

[![Collocatonal Networks](https://i3.ytimg.com/vi/WX0u-lj3dkw/maxresdefault.jpg)](https://youtu.be/WX0u-lj3dkw)