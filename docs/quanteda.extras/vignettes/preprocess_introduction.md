# Introduction to pre-processing functions in the quanteda.extras R package

``` r
library(quanteda.extras)
library(tidyverse)
```

## Pre-processing

The `preprocess_text()` function takes the following logical
(TRUE/FALSE) arguments:

- **contractions** (if set to TRUE contractions will be separated so that, for example, *can’t* becomes *ca n’t*)
- **hyphens** (if set to TRUE hyphens will be replaced by spaces)
- **punctuation** (if set to TRUE all punctuation marks will be exluded)
- **lower_case** (if set to TRUE all strings are converted to lower case)
- **accent_replace** (if set to TRUE accented chacaracters will be replaced by unaccented ones)
- **remove_numers** (if set to TRUE strings made up of numbers will be eliminated)
 
```{warning}

The `preprocess_text()` function takes a string vector. In the examples below, you will see it applied to a simple vector. It can also be applied to a column. A **tidyverse** method, for example, would be to use `mutate()` to manipulate a **readtext** `text` column: `mutate(text = preprocess_text(text))`.

Do not apply the function to an entire data frame or matrix.

```

### contractions:

``` r
a <- preprocess_text("can't won't we'll its' it's")
b <- preprocess_text("can't won't we'll its' it's", contractions = FALSE)
```

| TRUE                         |
|------------------------------|
| ca n’t wo n’t we ll its it s |

| FALSE                      |
|----------------------------|
| can’t won’t we’ll its it’s |

### hyphens:

``` r
a <- preprocess_text("un-knowable bluish-gray slo-mo stop-")
b <- preprocess_text("un-knowable bluish-gray slo-mo stop-", hypens = FALSE)
```

| TRUE                                |
|-------------------------------------|
| un knowable bluish gray slo mo stop |

| FALSE                               |
|-------------------------------------|
| un-knowable bluish-gray slo-mo stop |

### punctuation:

``` r
a <- preprocess_text("u.k. 50% 'cat' #great now?")
b <- preprocess_text("u.k. 50% 'cat' #great now?", punctuation = FALSE)
```

| TRUE                 |
|----------------------|
| u.k 50 cat great now |

| FALSE                      |
|----------------------------|
| u.k. 50% ‘cat’ #great now? |

### lower_case:

``` r
a <- preprocess_text("U.K. This A-1 1-A")
b <- preprocess_text("U.K. This A-1 1-A", lower_case = FALSE)
```

| TRUE             |
|------------------|
| u.k this a 1 1 a |

| FALSE            |
|------------------|
| U.K This A 1 1 A |

### accent_replace:

``` r
a <- preprocess_text("fiancée naïve façade")
b <- preprocess_text("fiancée naïve façade", accent_replace = FALSE)
```

| TRUE                 |
|----------------------|
| fiancee naive facade |

| FALSE                |
|----------------------|
| fiancée naïve façade |

### remove_numbers:

``` r
a <- preprocess_text("a-1 b2 50% 99 10,000", remove_numbers = TRUE)
b <- preprocess_text("a-1 50% 99 10,000")
```

| TRUE |
|------|
| a b2 |

| FALSE            |
|------------------|
| a 1 50 99 10,000 |

```{note}

These options represent some procedures that are common when “cleaning” texts. They give additional control over how a corpus is later “tokenized”. These are *not* intended to be comprehensive.

Depending on one’s data there may be other, specific ways a corpus needs to be processed prior to tokenizing.

The [**textclean**](https://github.com/trinker/textclean) package offers a host of options for pre-processing tasks. In addition, the [`tokens()`](http://quanteda.io/reference/tokens.html) function in **quanteda** has a variety of built-in options, some similar to the ones described above.

And, of course, one can use either native R `gsub()` or
[**stringr**](https://stringr.tidyverse.org/) **tidyverse** to create task-specific text processing functions.

```

