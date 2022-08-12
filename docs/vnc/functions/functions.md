# vnc functions

## `is.sequence`

A function to test the "evenness" of a sequence.


### Description

A function to test the "evenness" of a sequence.


### Usage

```r
is.sequence(x, ...)
```


### Arguments

Argument      |Description
------------- |----------------
`x`     |     A vector of integers or number


### Value

A logical value

## `vnc_clust`

This function is based on the work of Greis and Hilpert (2012) for
 Variability-Based Neighbor Clustering. See here:
 https://www.oxfordhandbooks.com/view/10.1093/oxfordhb/9780199922765.001.0001/oxfordhb-9780199922765-e-14


### Description

The idea is to use hierarchical clustering to aid "bottom up" periodization of language change. The functions below are built on their original code here: [http://global.oup.com/us/companion.websites/fdscontent/uscompanion/us/static/companion.websites/nevalainen/Gries-Hilpert_web_final/vnc.individual.html](http://global.oup.com/us/companion.websites/fdscontent/uscompanion/us/static/companion.websites/nevalainen/Gries-Hilpert_web_final/vnc.individual.html). However, rather than producing a plot, this function returns an **hclust** object. The advantage, is that an "hclust" object can be used to produce not only base R dendrograms, but can be passed to other functions for more detailed and controlled plotting.


### Usage

```r
vnc_clust(time, values, distance.measure = c("sd", "cv"))
```

### Arguments

Argument      |Description
------------- |----------------
`time`     |     A vector of sequential time intervals like years or decades
`values`     |     A vector containing normaized frequency counts
`distance.measure`     |     Indicating whether the standard deviation or coefficient of variation should be used in dinstance calculations


### Value

An hclust object


## `vnc_scree`

This is a simple function to return a scree plot based on the VNC algorithm.


### Description

This is a simple function to return a scree plot based on the VNC algorithm.


### Usage

```r
vnc_scree(time, values, distance.measure = c("sd", "cv"))
```


### Arguments

Argument      |Description
------------- |----------------
`time`     |     A vector of sequential time intervals like years or decades
`values`     |     A vector containing normaized frequency counts
`distance.measure`     |     Indicating whether the standard deviation or coefficient of variation should be used in dinstance calculations


### Value

A scree plot



