# ngramr.plus functions

## `google_ngram`

This function extracts frequency data from Google Books' Ngram data:

[http://storage.googleapis.com/books/ngrams/books/datasetsv2.html](http://storage.googleapis.com/books/ngrams/books/datasetsv2.html)

The function is set up to facilitate the counting of lemmas and ingnore differences in capitalization. The user has control over what to combine into counts with the word_forms argument.


### Description

<div class="panel panel-warning">
**Warning**
{: .panel-heading}
<div class="panel-body">

Google's data tables are HUGE. Sometime running into multiple gigabytes for simple text files. Thus, depending on the table being accessed, the return time can be slow. For example, asscessing the 1-gram Q file should take only a few seconds, but the 1-gram T file might take 10 minutes to process. The 2-gram, 3-gram, etc. files are even larger and slower to process.

</div>
</div>


### Usage

```r
google_ngram(
  word_forms,
  variety = c("eng", "gb", "us", "fiction"),
  by = c("year", "decade")
)
```


### Arguments

Argument      |Description
------------- |----------------
`word_forms`     |     A vector of words or phrases to be searched
`variety`     |     The variety of English to be searched
`by`     |     Whether the counts should be summed by year or by decade


### Value

A dataframe of counts from Google Books


