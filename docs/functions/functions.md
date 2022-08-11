# cmu.textstat functions

## `from_play`

This is a helper function for reading in the dialogue from Shakespeare plays
 and screenplays, which are formatted in very simple markup.
 With this function, either the dialogue or the direction can be
 execrated in bulk. It can also be used to extract the dialogue of specific characters.


### Description

This is a helper function for reading in the dialogue from Shakespeare plays
 and screenplays, which are formatted in very simple markup.
 With this function, either the dialogue or the direction can be
 execrated in bulk. It can also be used to extract the dialogue of specific characters.


### Usage

```r
from_play(plays, extract = c("dialogue", "direction"))
```


### Arguments

Argument      |Description
------------- |----------------
`plays`     |     A readtext dataframe containing a doc_id column and a text column.
`extract`     |     A character vector specifying what is to be extracted like 'dialogue' or 'romeo'.


### Value

A readtext dataframe with the extracted text.


