# pseudobibeR functons

## `biber_spacy`

This function takes a spacyr data object that has been
 part-of-speech tagged and dependency parsed and
 extracts counts of features that have been used in
 Douglas Biber's research since the late 1980s.


### Description

Note that function sometimes relies on lists and a dictionary,
 both of which are stored as R data.
 At other times, the function identifies features
 based on local cues, which are approximations.


### Usage

```r
biber_spacy(spacy_tks)
```


### Arguments

Argument      |Description
------------- |----------------
`spacy_tks`     |     A data.frame of tokens created by spacyr


### Details

The function returns a data.frame of normalized counts.


### Value

A data.frame of feature counts


## `biber_udpipe`

This function takes a udpipe data object or data frame that has been
 part-of-speech tagged and dependency parsed and
 extracts counts of features that have been used in
 Douglas Biber's research since the late 1980s.


### Description

Note that function sometimes relies on lists and a dictionary,
 both of which are stored as R data.
 At other times, the function identifies features
 based on local cues, which are approximations.


### Usage

```r
biber_udpipe(udpipe_tks)
```


### Arguments

Argument      |Description
------------- |----------------
`udpipe_tks`     |     A data.frame of tokens created by spacyr


### Details

The function returns a data.frame of normalized counts.


### Value

A data.frame of feature counts


