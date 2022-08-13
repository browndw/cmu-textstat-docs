# quanteda.extras data

## `sample_corpus`

### Description

The corpus contains data from 8 text-types:

- Academic
- Blog
- Fiction
- Magazine
- News
- Spoken
- Television & Movie
- Web

Its compilation follows the sampling procedures of the [Corpus of Contemporary American English](https://www.english-corpora.org/coca/). However, it contains only 50 texts from each type, and each text is only about 2,500 words.

Thus, it is similar to the [Brown family of corpora](https://www1.essex.ac.uk/linguistics/external/clmt/w3c/corpus_ling/content/corpora/list/private/brown/brown.html) in its size (roughly 1 million words).

The data frame contains 2 variables (`doc_id` and `text`) and 400 observations.


```{warning}

These data are included *only* for demonstration purposes. They *not* compiled to be used for research.

```


### Data snippet

| doc_id  | text                                                                             |
|---------|----------------------------------------------------------------------------------|
| acad_01 | Teachers and other school personnel are often counseled to use research fin[...] |
| acad_02 | Abstract Does the conflict in El Salvador, conceptualized by the U.S. gover[...] |
| acad_03 | January 17, 1993, will mark the 100th anniversary of the deposing of the Ha[...] |
| acad_04 | Thirty years have passed since the T1961 meeting of the National Council fo[...] |
| acad_05 | ABSTRACT -- A common property resource with open access, such as a fishery,[...] |
| acad_06 | Despite some encouraging signs and hopeful expectations that democracy has [...] |


## `ds_dict`

### Description

A [**quanteda** dictionary](http://quanteda.io/reference/dictionary.html) that allows for token classification via lookups: `tokens_lookup(dictionary = ds_dict, levels = 1, valuetype = "fixed")`.

The dictionary is a highly simplified version of [DocuScope](https://docuscospacy.readthedocs.io/en/latest/docuscope.html). You can find [the category descriptions here](https://docuscospacy.readthedocs.io/en/latest/docuscope.html#categories). DocuScope follows similar principles to a [Sentiment Lexicon](https://www.saifmohammad.com/WebPages/NRC-Emotion-Lexicon.htm) or [LIWC (Linguistic Inquiry and Word Count)](https://lit.eecs.umich.edu/geoliwc/liwc_dictionary.html), but is orders of manitude larger and organizes its categories according to a rhetorical orientation to language.

The dictionary object is a list of lists. It contains 37 named lists and 145,905 entries. The entries are phrases in lowercase, with no glob or regex patterns.

To use the dictionary, fixed matches should be used and tokens should be prepared with minimal processing: `tokens(remove_punct = F, remove_numbers = F, remove_symbols = F, what = "word")`.


### Data snippet

```
Dictionary object with 1 key entry.- [AcademicTerms]:  - a chapter in, a couple, a declaration of, a detail, a distinction between, a domain, a force, a forced, a form of, a grade, a hint of, a home for, a hub, a kind of, a kind of a, a load, a loaded, a metaphor for, a mix of, a mixture of [ ... and 8,884 more ]

```
