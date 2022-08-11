## vnc

This package contains functions based on the work of Greis and Hilpert (2012) for [Variability-Based Neighbor Clustering](https://www.oxfordhandbooks.com/view/10.1093/oxfordhb/9780199922765.001.0001/oxfordhb-9780199922765-e-14).

The idea is to use hierarchical clustering to aid "bottom up" periodization of language change. The functions are built on [their original code](http://global.oup.com/us/companion.websites/fdscontent/uscompanion/us/static/companion.websites/nevalainen/Gries-Hilpert_web_final/vnc.individual.html). However, rather than producing a plot, this function returns an [**hclust**](https://www.rdocumentation.org/packages/stats/versions/3.6.2/topics/hclust) object. The advantage, is that an **hclust** object can be used to produce not only base R dendrograms, but can be passed to other functions for more detailed and controlled plotting.

## Installing vnc

Use devtools to install the package.

```r
devtools::install_github("browndw/vnc")
```
## Running vnc

The package contains two basic functions. The first, **vnc_scree( )** requires a vector representing a time sequence, which would typically be a numeric vector of years (1900, 1901, 1902, ...) or decades (1900, 1910, 1920, ...). It also requires a vector representing normalized frequencies of the word or phrase being analyzed.

For example, imagine that historical changes in the frequency of the word [*teenager*](https://books.google.com/ngrams/graph?content=teenager&year_start=1800&year_end=2019&corpus=26&smoothing=3) were being analyzed, and we had a data.frame **df** with a column of years and column of normalized counts per million words. We would generate the scree plot:

```r
vnc_scree(df$year, df$counts_permil, distance.measure = "sd")
```
Then we could generate a VNC dendrogram by first creating an **hclust** object then plotting it:

```r
hc <- vnc_clust(df$year, df$counts_permil, distance.measure = "sd")
plot(hc, hang = -1)
```
For more details, consult the [vignette](http://htmlpreview.github.io/?https://raw.githubusercontent.com/browndw/vnc/main/vignettes/introduction.html).
