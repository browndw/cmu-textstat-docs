## quanteda.extras

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

## Functions

```{toctree}
:maxdepth: 1

functions/functions.md
```

## Vignettes

```{toctree}
:maxdepth: 1

vignettes/collocations_introduction.md
vignettes/dispersions_introduction.md
vignettes/keyness_introduction.md
vignettes/preprocess_introduction.md
```
