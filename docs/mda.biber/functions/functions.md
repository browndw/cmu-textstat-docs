# mda.biber functions

## `boxplot_mda`

The boxplot_mda() function combines scaled vectors of the relevant factor loadings and boxplots of diminesion scores.


### Description

The boxplot_mda() function combines scaled vectors of the relevant factor loadings and boxplots of diminesion scores.


### Usage

```r
boxplot_mda(mda_data, n_factor = 1)
```


### Arguments

Argument      |Description
------------- |----------------
`mda_data`     |     An mda data.frame produced by the mda_loadings() function.
`n_factor`     |     The factor to be plotted.


### Value

A combined plot of scaled vectors and boxplots.

## `heatmap_mda`

The heatmap_mda() function combines a stick plot with a heat map of the relevant factor loadings.


### Description

The heatmap_mda() function combines a stick plot with a heat map of the relevant factor loadings.


### Usage

```r
heatmap_mda(mda_data, n_factor = 1)
```


### Arguments

Argument      |Description
------------- |----------------
`mda_data`     |     An mda data.frame produced by the mda_loadings() function.
`n_factor`     |     The factor to be plotted.


### Value

A combined stick plot and heat map.

## `mda_loadings`

Multi-Dimensional Analysis is a statistical procedure developed Biber and is commonly used in descriptions of language as it varies by genre, register, and task. The procedure is a specific application factor analysis, which is used as the basis for calculating a 'dimension score' for each text.


### Description

The function mda_loadings() returns a data.frame of dimension scores with the means for each category and the factor loadings accessible as attributes. Calculating MDA requires a data.frame containing a column with a categorical variable (formatted as a factor) and more than 2 continuous, numeric variables.


### Usage

```r
mda_loadings(obs_by_group, n_factors, cor_min = 0.2, threshold = 0.35)
```


### Arguments

Argument      |Description
------------- |----------------
`obs_by_group`     |     A data.frame containing 1 categorical (factor) variable and continuous (numeric) variables.
`n_factors`     |     The number of factors to be calculated in the factor analysis.
`cor_min`     |     The correlation threshold for including variables in the factor analysis.
`threshold`     |     A value indicating the threshold at which variables should be included in dimension score calculations (the default is .35).


### Value

An mda data structure containing scores, means by group, and factor loadings

## `screeplot_mda`

A wrapper for the [nScree](https://search.r-project.org/CRAN/refmans/nFactors/html/nScree.html) function included in the nFactors package.


### Description

A wrapper for the [nScree](https://search.r-project.org/CRAN/refmans/nFactors/html/nScree.html) function included in the nFactors package.


### Usage

```r
screeplot_mda(obs_by_group, cor_min = 0.2)
```


### Arguments

Argument      |Description
------------- |----------------
`obs_by_group`     |     A data.frame containing 1 categorical (factor) variable and continuous (numeric) variables.
`cor_min`     |     The correlation threshold for including variables in the factor analysis.


### Value

A scree plot.

## `stickplot_mda`

A simple function for producing the stick plots that are common in visualizing the location of category means along a given dimension.


### Description

A simple function for producing the stick plots that are common in visualizing the location of category means along a given dimension.


### Usage

```r
stickplot_mda(mda_data, n_factor = 1)
```


### Arguments

Argument      |Description
------------- |----------------
`mda_data`     |     An mda data.frame produced by the mda_loadings() function.
`n_factor`     |     The factor to be plotted.


### Value

A stick plot showing category means long a positve/negative cline.






