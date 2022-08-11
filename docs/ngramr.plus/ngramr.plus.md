## ngramr.plus

This package has functions for processing [Google's Ngram repositories](http://storage.googleapis.com/books/ngrams/books/datasetsv2.html) without having to download them locally. These repositories vary in their size, but the larger ones (like th one for the letter *s* or common bigrams) contain multiple gigabytes.

The main function uses [chunking from the **readr** ](https://readr.tidyverse.org/reference/read_delim_chunked.html)package to reduce memory load. Still, depending on the specific word forms being searched, loading and processing the data tables can take up to a couple of minutes.

If you only want to plot and analyze trends, I would highly recommend the [**ngramr**](https://github.com/seancarmody/ngramr) package, which is extremely efficient.

The advantage of **ngramr.plus** is that it returns the underlying data, enabling more precise identification of peaks and troughs, the use of variability-based neighbor clustering for ground-up periodization (see the [vnc package](https://github.com/browndw/vnc)), etc.

## Installing ngramr.plus

Use devtools to install the package.

```r
devtools::install_github("browndw/ngramr.plus")
```
## Running ngramr.plus

The `google_ngram()` function takes three arguments: `word_forms`, `variety`, and `by`. The first can be a single word like *teenager* or lemmas like *walk*, *walks* and *walked* that are put into a vector: `c("walk", "walks", "walked")`. The same principal applies to ngrams > 1: `c("teenager is", "teenagers are"`. The first word in an ngram sequence should be from the same root. So the function would **fail** to process `c("teenager is", "child is")`. The function will combine the counts of all forms in the returned data frame.

The variety argument can be one of: `eng`, `gb`, `us`, or `fiction`, for all English, British English, American English, or fiction, respectively.

The function can also return counts summed and normalized by year or by decade using the by argument: `by="year"` or `by="decade")`.

The following would return counts for the word *x-ray* in US English by year:

```r
xray_year <- google_ngram(word_forms = "x-ray", variety = "us", by = "year")
```

Alternatively, the following would return counts of the combined forms *xray* and *xrays* in British English by decade:

```r
xray_decade <- google_ngram(word_forms = c("x-ray", "x-rays"), variety = "gb", by = "decade")
```
## Functions

```{toctree}
:maxdepth: 1

functions/functions.md
```

## Vignettes

```{toctree}
:maxdepth: 1

vignettes/ngramr_introduction.md
```
