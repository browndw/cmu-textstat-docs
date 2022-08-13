# quanteda.extras

This package contains statstical and plotting tools that expand the functionality of [quanteda](http://quanteda.io/). These include functions for calculating:

* keyness
* effect size
* dispersion
* collocation


## Installing quanteda.extras

Use devtools to install the package.

```r
devtools::install_github("browndw/quanteda.extras")
```

## Using quanteda.extras

The package is designed to facilitate basic corpus functions of the kind produced by concordancers like [AntConc](https://www.laurenceanthony.net/software/antconc/) or [WordSmith](https://lexically.net/wordsmith/) in the R programming environment.

As the name of the package suggests, many of the functions take outputs produced by the [quanteda](https://quanteda.io/) package. Users of the package should, therefore, be familiar with quanteda processing pipelines -- particularly [tokenizing](https://quanteda.io/reference/tokens.html) and the creation of [document-feature matrices](https://quanteda.io/reference/dfm.html).

## A simple frequency table

As a simple example, we can start with the `sample_corpus` included with the package.

We can check the composition of the corpus by taking the text-type information from the `doc_id` variable:

``` r
sample_corpus %>%
  mutate(text_type = str_extract(doc_id, "[a-z]+")) %>%
  group_by(text_type) %>%
  tally()
```

The corpus contains 50 text sample of each type:


| text_type |  n |
|-----------|---:|
| acad      | 50 |
| blog      | 50 |
| fic       | 50 |
| mag       | 50 |
| news      | 50 |
| spok      | 50 |
| tvm       | 50 |
| web       | 50 |

In order to generate a table from the `frequency_table` function, the data must first be tokenized.

``` r
sc_tokens <- sample_corpus %>%
  mutate(text = preprocess_text(text)) %>% # Pre-process the text column
  corpus() %>% # Generate a corpus object
  tokens(what = "fastestword") # Tokenize
  
# Generate a frequency table
sc_counts <- frequency_table(sc_tokens)

```
The result is a table showing absolute and relative frquencies, as well as average reduced frequncy, and deviation of proportions. 

| Token | AF    | Per\_10.6           | ARF                | DP                  |
|-------|------:|-------------------:|-------------------:|--------------------:|
| the   | 51046 | 49572.98822779917  | 31756.267973437283 | 0.14353581079341268 |
| to    | 25942 | 25193.403216815543 | 16219.377547552038 | 0.09146807813084532 |
| and   | 25344 | 24612.65943747487  | 15910.523549257367 | 0.12570387667514152 |
| of    | 24331 | 23628.8911289931   | 14695.894699887542 | 0.17752068678885638 |
| a     | 22694 | 22039.129311634104 | 13920.262892414788 | 0.1098711133049567  |
| in    | 16959 | 16469.6216619372   | 10384.016099616008 | 0.14662171556500003 |

## Functions

```{toctree}
:maxdepth: 2

functions/functions.md
```

## data

```{toctree}
:maxdepth: 2

data/data.md
```


## Vignettes

```{toctree}
:maxdepth: 2

vignettes/collocations_introduction.md
vignettes/dispersions_introduction.md
vignettes/keyness_introduction.md
vignettes/preprocess_introduction.md
```
