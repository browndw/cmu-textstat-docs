# Set 3

## Lab 7: Part-of-speech tagging and dependency parsing

In the previous lab, we worked with keyness and effect sizes, specifically using log-likelihood and log ratio measures.

We are now going to add to our toolkit by using the same measures, but applied to data that has been tagged and parsed. To our processing pipeline, we will be adding **[udpipe](https://bnosac.github.io/udpipe/en/)**.

### What does udpipe do?

Before we start processing in R, let’s get some sense of what “universal
dependency parsing” is and what its output looks like.

### Parse a sample sentence online

Go to this webpage: <http://lindat.mff.cuni.cz/services/udpipe/>.

And paste the following sentence into the text field:

> The company offers credit cards, loans and interest-generating accounts.

Set the model to **english-ewt-ud**. Then, click the “Process Input” button. You should now see an output. If you choose the “Table” tab, you can view the output in a tablular format:

| Id |    Form    |   Lemma  | UPosTag | XPosTag |                           Feats                           | Head |  DepRel  | Deps |               Misc              |
|:--:|:----------:|:--------:|:-------:|:-------:|:---------------------------------------------------------:|:----:|:--------:|:----:|:-------------------------------:|
| 1  | The        | the      | DET     | DT      | Definite=Def\|PronType=Art                                | 2    | det      | _    | TokenRange=0:3                  |
| 2  | company    | company  | NOUN    | NN      | Number=Sing                                               | 3    | nsubj    | _    | TokenRange=4:11                 |
| 3  | offers     | offer    | VERB    | VBZ     | Mood=Ind\|Number=Sing\|Person=3\|Tense=Pres\|VerbForm=Fin | 0    | root     | _    | TokenRange=12:18                |
| 4  | credit     | credit   | NOUN    | NN      | Number=Sing                                               | 5    | compound | _    | TokenRange=19:25                |
| 5  | cards      | card     | NOUN    | NNS     | Number=Plur                                               | 3    | obj      | _    | SpaceAfter=No\|TokenRange=26:31 |
| 6  | ,          | ,        | PUNCT   | ,       | _                                                         | 7    | punct    | _    | TokenRange=31:32                |
| 7  | loans      | loan     | NOUN    | NNS     | Number=Plur                                               | 5    | conj     | _    | TokenRange=33:38                |
| 8  | and        | and      | CCONJ   | CC      | _                                                         | 12   | cc       | _    | TokenRange=39:42                |
| 9  | interest   | interest | NOUN    | NN      | Number=Sing                                               | 11   | compound | _    | SpaceAfter=No\|TokenRange=43:51 |
| 10 | -          | -        | PUNCT   | HYPH    | _                                                         | 11   | punct    | _    | SpaceAfter=No\|TokenRange=51:52 |
| 11 | generating | generate | NOUN    | NN      | Number=Sing                                               | 12   | compound | _    | TokenRange=52:62                |
| 12 | accounts   | account  | NOUN    | NNS     | Number=Plur                                               | 5    | conj     | _    | SpaceAfter=No\|TokenRange=63:71 |
| 13 | .          | .        | PUNCT   | .       | _                                                         | 3    | punct    | _    | SpaceAfter=No\|TokenRange=71:72 |

### Basic parse structure

There is a column for the **token** and one for the token’s base form or **[lemma](https://en.wikipedia.org/wiki/Lemma_(morphology))**.

Those are followed by a tag for the general lexical class or “[universal
part-of-speech](https://universaldependencies.org/u/pos/)” (**upos**) tag, and a tree-bank specific (**xpos**) part-of-speech tag.

The **xpos** tags are [Penn Treebank tags](https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html).

The part-of-speech tags are followed by a column of integers that refer to the id of the token that is at the head of the dependency structure, which is followed by the **[dependency relation](https://universaldependencies.org/u/dep/index.html)** identifier.

### Visualize the dependency

From the “Output Text” tab, copy the output start with the `sent_id` including the pound sign

Paste the information into the text field here:
<https://urd2.let.rug.nl/~kleiweg/conllu/>.

Then click the “Submit Query” button below the text field. This should generate a visualization of the dependency structure.

### Load the needed packages

``` r
library(cmu.textstat)
library(tidyverse)
library(quanteda)
library(quanteda.textstats)
library(udpipe)
```

### Parsing

### Preparing a corpus

When we parse texts using a model like ones available in **udpipe** or **spacy**, we need to do very little to prepare the corpus. We could trim extra spaces and returns using `str_squish()` or remove urls, but generally we want the text to be mostly “as is” so the model can do its job.

#### Download a model

You only need to run this line of code **once**. To run it, remove the pound sign, run the line, then add the pound sign after you’ve downloaded the model. Or you can run the next chunk and the model will automatically be downloaded in your working directory.

``` r
# udpipe_download_model(language = "english")
```

#### Annotate a sentence

``` r
txt <- "The company offers credit cards, loans and interest-generating accounts."
annotation <- udpipe(txt, "english")
```

| doc_id | paragraph_id | sentence_id | sentence                                                                 | start | end | term_id | token_id | token      | lemma    | upos  | xpos | feats                                                     | head_token_id | dep_rel  | deps | misc           |
|--------|-------------:|------------:|--------------------------------------------------------------------------|------:|----:|--------:|----------|------------|----------|-------|------|-----------------------------------------------------------|---------------|----------|------|----------------|
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |     1 |   3 |       1 | 1        | The        | the      | DET   | DT   | Definite=Def\|PronType=Art                                | 2             | det      | NA   | NA             |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |     5 |  11 |       2 | 2        | company    | company  | NOUN  | NN   | Number=Sing                                               | 3             | nsubj    | NA   | NA             |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |    13 |  18 |       3 | 3        | offers     | offer    | VERB  | VBZ  | Mood=Ind\|Number=Sing\|Person=3\|Tense=Pres\|VerbForm=Fin | 0             | root     | NA   | NA             |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |    20 |  25 |       4 | 4        | credit     | credit   | NOUN  | NN   | Number=Sing                                               | 5             | compound | NA   | NA             |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |    27 |  31 |       5 | 5        | cards      | card     | NOUN  | NNS  | Number=Plur                                               | 3             | obj      | NA   | SpaceAfter=No  |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |    32 |  32 |       6 | 6        | ,          | ,        | PUNCT | ,    | NA                                                        | 7             | punct    | NA   | NA             |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |    34 |  38 |       7 | 7        | loans      | loans    | NOUN  | NNS  | Number=Plur                                               | 5             | conj     | NA   | NA             |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |    40 |  42 |       8 | 8        | and        | and      | CCONJ | CC   | NA                                                        | 12            | cc       | NA   | NA             |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |    44 |  51 |       9 | 9        | interest   | interest | NOUN  | NN   | Number=Sing                                               | 11            | compound | NA   | SpaceAfter=No  |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |    52 |  52 |      10 | 10       | -          | -        | PUNCT | HYPH | NA                                                        | 11            | punct    | NA   | SpaceAfter=No  |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |    53 |  62 |      11 | 11       | generating | genera   | NOUN  | NN   | Number=Sing                                               | 12            | compound | NA   | NA             |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |    64 |  71 |      12 | 12       | accounts   | account  | NOUN  | NNS  | Number=Plur                                               | 5             | conj     | NA   | SpaceAfter=No  |
| doc1   |            1 |           1 | The company offers credit cards, loans and interest-generating accounts. |    72 |  72 |      13 | 13       | .          | .        | PUNCT | .    | NA                                                        | 3             | punct    | NA   | SpacesAfter=No |


#### Plot the annotation

We can also plot the dependency structure using **[igraph](https://igraph.org/r/)**:

``` r
library(igraph)
library(ggraph)
```

First we’ll create a plotting function.

``` r
plot_annotation <- function(x, size = 3){
  stopifnot(is.data.frame(x) & all(c("sentence_id", "token_id", "head_token_id", "dep_rel",
                                     "token_id", "token", "lemma", "upos", "xpos", "feats") %in% colnames(x)))
  x <- x[!is.na(x$head_token_id), ]
  x <- x[x$sentence_id %in% min(x$sentence_id), ]
  edges <- x[x$head_token_id != 0, c("token_id", "head_token_id", "dep_rel")]
  edges <- edges[edges$dep_rel != "punct",]
  edges$head_token_id <- ifelse(edges$head_token_id == 0, edges$token_id, edges$head_token_id)
  nodes = x[, c("token_id", "token", "lemma", "upos", "xpos", "feats")]
  edges$label <- edges$dep_rel
  g <- graph_from_data_frame(edges,
                             vertices = nodes,
                             directed = TRUE)
  ggraph(g, layout = "linear") +
    geom_edge_arc(ggplot2::aes(label = dep_rel, vjust = -0.20), fold = T,linemitre = 2,
                  arrow = grid::arrow(length = unit(3, 'mm'), ends = "last", type = "closed"),
                  end_cap = ggraph::label_rect("wordswordswords"),
                  label_colour = "red", check_overlap = TRUE, label_size = size) +
    geom_node_label(ggplot2::aes(label = token), col = "black", size = size, fontface = "bold") +
    geom_node_text(ggplot2::aes(label = xpos), nudge_y = -0.35, size = size) +
    theme_graph(base_family = "Arial Narrow")
}
```

And plot the annotation:

``` r
plot_annotation(annotation, size = 2.5)
```

![Dependency structure of a sample parsed
sentence.](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/labs_files/figure-gfm/anno_plot-1.png)

### Annotate a corpus

Parsing text is a computationally intensive process and can take time.
So for the purposes of this lab, we’ll create a smaller sub-sample of
the the data. By adding a column called `text_type` which includes
information extracted from the file names, we can sample 5 texts from
each.

``` r
set.seed(123)
sub_corpus <- quanteda.extras::sample_corpus %>%
  mutate(text_type = str_extract(doc_id, "^[a-z]+")) %>%
  group_by(text_type) %>%
  sample_n(5) %>%
  ungroup() %>%
  dplyr::select(doc_id, text)
```

#### Parallel processing

Parallel processing is a method whereby separate parts of an overall complex task are broken up and run simultaneously on multiple CPUs, thereby reducing the amount of time for processing. Part-of-speech tagging and dependency parsing are computationally intensive, so using parallel processing can save valuable time.

The `udpipe()` [function](https://bnosac.github.io/udpipe/docs/doc8.html) has an argument for assigning cores: `parallel.cores = 1L`. It’s easy to set up, so feel free to use that option.

A second option, requires more preparation, but is even faster. So we’ll walk through how it works. First, we will split the corpus based on available cores.

``` r
corpus_split <- split(sub_corpus, seq(1, nrow(sub_corpus), by = 10))
```

For parallel processing in R, we’ll us the package **[future.apply](https://future.apply.futureverse.org/)**.

``` r
library(future.apply)
```

Next, we set up our parallel session by specifying the number of cores, and creating a simple annotation function.

``` r
ncores <- 4L
plan(multisession, workers = ncores)

annotate_splits <- function(corpus_text) {
  ud_model <- udpipe_load_model("english-ewt-ud-2.5-191206.udpipe")
  x <- data.table::as.data.table(udpipe_annotate(ud_model, x = corpus_text$text,
                                                 doc_id = corpus_text$doc_id))
  return(x)
}
```

Finally, we annotate using `future_lapply`. On my machine, this takes
roughly 32 seconds.

``` r
annotation <- future_lapply(corpus_split, annotate_splits, future.seed = T)
```

As you might guess, the output is a list of data frames, so we’ll
combine them using `rbindlist()`.

``` r
annotation <- data.table::rbindlist(annotation)
```

### Process with quanteda

#### Format the data for quanteda

If we want to do any further processing in **[quanteda](http://quanteda.io/index.html)**, we need to make a couple of adjustments to our data frame.

``` r
anno_edit <- annotation %>%
  dplyr::select(doc_id, sentence_id, token_id, token, lemma, upos, xpos, head_token_id, dep_rel) %>%
  rename(pos = upos, tag = xpos)

anno_edit <- structure(anno_edit, class = c("spacyr_parsed", "data.frame"))
```

#### Convert to tokens

``` r
sub_tkns <- as.tokens(anno_edit, include_pos = "tag", concatenator = "_")
```

#### Create a dfm

We will also extract and assign the variable `text_type` to the tokens
object.

``` r
doc_categories <- names(sub_tkns) %>%
  data.frame(text_type = .) %>%
  mutate(text_type = str_extract(text_type, "^[a-z]+"))

docvars(sub_tkns) <- doc_categories

sub_dfm <- dfm(sub_tkns)
```

And check the frequencies:

| feature | frequency | rank | docfreq | group | end | term_id | token_id | token    | lemma    | upos  | xpos | feats                                                     | head_token_id | dep_rel  | deps | misc          |
|---------|----------:|-----:|--------:|-------|----:|--------:|----------|----------|----------|-------|------|-----------------------------------------------------------|---------------|----------|------|---------------|
| .\_.    |      6452 |    1 |      40 | all   |   3 |       1 | 1        | The      | the      | DET   | DT   | Definite=Def\|PronType=Art                                | 2             | det      | NA   | NA            |
| ,\_,    |      5900 |    2 |      40 | all   |  11 |       2 | 2        | company  | company  | NOUN  | NN   | Number=Sing                                               | 3             | nsubj    | NA   | NA            |
| the_dt  |      5217 |    3 |      40 | all   |  18 |       3 | 3        | offers   | offer    | VERB  | VBZ  | Mood=Ind\|Number=Sing\|Person=3\|Tense=Pres\|VerbForm=Fin | 0             | root     | NA   | NA            |
| and_cc  |      2596 |    4 |      40 | all   |  25 |       4 | 4        | credit   | credit   | NOUN  | NN   | Number=Sing                                               | 5             | compound | NA   | NA            |
| of_in   |      2513 |    5 |      40 | all   |  31 |       5 | 5        | cards    | card     | NOUN  | NNS  | Number=Plur                                               | 3             | obj      | NA   | SpaceAfter=No |
| a_dt    |      2256 |    6 |      40 | all   |  32 |       6 | 6        | ,        | ,        | PUNCT | ,    | NA                                                        | 7             | punct    | NA   | NA            |
| to_to   |      1702 |    7 |      40 | all   |  38 |       7 | 7        | loans    | loans    | NOUN  | NNS  | Number=Plur                                               | 5             | conj     | NA   | NA            |
| in_in   |      1645 |    8 |      40 | all   |  42 |       8 | 8        | and      | and      | CCONJ | CC   | NA                                                        | 12            | cc       | NA   | NA            |
| i_prp   |      1497 |    9 |      36 | all   |  51 |       9 | 9        | interest | interest | NOUN  | NN   | Number=Sing                                               | 11            | compound | NA   | SpaceAfter=No |
| you_prp |      1202 |   10 |      36 | all   |  52 |      10 | 10       | -        | -        | PUNCT | HYPH | NA                                                        | 11            | punct    | NA   | SpaceAfter=No |

#### Filter/select tokens

```{warning}
There are multiple ways to filter/select the tokens we want to count. We could, for example, just filter out all rows in the annotation data frame tagged as **PUNCT**, if we wanted to exclude punctuation from our counts.

I would, however, advise against altering the original parsed file. We may want to try different options, and we want to avoid having to re-parse our corpus, as that is the most computationally intensive step in the processing pipeline. In fact, if this were part of an actual project, I would advise that you save the parsed data frame as a `.csv` file using `write_csv()` or as an `.rda` file for later use.
```

We will use the `tokens_select()` function to either keep or remove tokens based on regular expressions.

``` r
sub_dfm <- sub_tkns %>%
  tokens_select("^.*[a-zA-Z0-9]+.*_[a-z]", selection = "keep", valuetype = "regex", case_insensitive = T) %>%
  dfm()
```

And check the frequencies:

| feature | frequency | rank | docfreq | group | end | term_id | token_id | token    | lemma    | upos  | xpos | feats                                                     | head_token_id | dep_rel  | deps | misc          |
|---------|----------:|-----:|--------:|-------|----:|--------:|----------|----------|----------|-------|------|-----------------------------------------------------------|---------------|----------|------|---------------|
| the_dt  |      5217 |    1 |      40 | all   |   3 |       1 | 1        | The      | the      | DET   | DT   | Definite=Def\|PronType=Art                                | 2             | det      | NA   | NA            |
| and_cc  |      2596 |    2 |      40 | all   |  11 |       2 | 2        | company  | company  | NOUN  | NN   | Number=Sing                                               | 3             | nsubj    | NA   | NA            |
| of_in   |      2513 |    3 |      40 | all   |  18 |       3 | 3        | offers   | offer    | VERB  | VBZ  | Mood=Ind\|Number=Sing\|Person=3\|Tense=Pres\|VerbForm=Fin | 0             | root     | NA   | NA            |
| a_dt    |      2256 |    4 |      40 | all   |  25 |       4 | 4        | credit   | credit   | NOUN  | NN   | Number=Sing                                               | 5             | compound | NA   | NA            |
| to_to   |      1702 |    5 |      40 | all   |  31 |       5 | 5        | cards    | card     | NOUN  | NNS  | Number=Plur                                               | 3             | obj      | NA   | SpaceAfter=No |
| in_in   |      1645 |    6 |      40 | all   |  32 |       6 | 6        | ,        | ,        | PUNCT | ,    | NA                                                        | 7             | punct    | NA   | NA            |
| i_prp   |      1497 |    7 |      36 | all   |  38 |       7 | 7        | loans    | loans    | NOUN  | NNS  | Number=Plur                                               | 5             | conj     | NA   | NA            |
| you_prp |      1202 |    8 |      36 | all   |  42 |       8 | 8        | and      | and      | CCONJ | CC   | NA                                                        | 12            | cc       | NA   | NA            |
| it_prp  |      1168 |    9 |      39 | all   |  51 |       9 | 9        | interest | interest | NOUN  | NN   | Number=Sing                                               | 11            | compound | NA   | SpaceAfter=No |
| is_vbz  |      1042 |   10 |      40 | all   |  52 |      10 | 10       | -        | -        | PUNCT | HYPH | NA                                                        | 11            | punct    | NA   | SpaceAfter=No |

If we want to compare one text-type (as our target corpus) to another
(as our reference corpus), we can easily subset the data.

``` r
acad_dfm <- dfm_subset(sub_dfm, text_type == "acad") %>% dfm_trim(min_termfreq = 1)
fic_dfm <- dfm_subset(sub_dfm, text_type == "fic") %>% dfm_trim(min_termfreq = 1)
```

And finally, we can generate a keyness table,

``` r
acad_v_fic <- keyness_table(acad_dfm, fic_dfm) %>%
  separate(col = Token, into = c("Token", "Tag"), sep = "_")
```

|    Token    | Tag |        LL |        LR | PV | AF_Tar | AF_Ref | Per\_10.4\_Tar | Per\_10.4\_Ref |    DP_Tar |    DP_Ref |
|:-----------:|:---:|----------:|----------:|---:|-------:|-------:|-----------:|-----------:|----------:|----------:|
| of          | in  | 254.97151 | 1.8779434 |  0 |    563 |    159 |  445.02411 | 121.078282 | 0.0937508 | 0.0673735 |
| japan       | nnp |  69.77395 | 6.6685451 |  0 |     49 |      0 |   38.73212 |   0.000000 | 0.7559586 |        NA |
| methylation | nn  |  68.34999 | 6.6387977 |  0 |     48 |      0 |   37.94166 |   0.000000 | 0.8039681 |        NA |
| smoking     | nn  |  66.92604 | 6.6084241 |  0 |     47 |      0 |   37.15121 |   0.000000 | 0.7826915 |        NA |
| china       | nnp |  58.38229 | 6.4113872 |  0 |     41 |      0 |   32.40851 |   0.000000 | 0.5975812 |        NA |
| japanese    | jj  |  48.41458 | 6.1412981 |  0 |     34 |      0 |   26.87535 |   0.000000 | 0.7967750 |        NA |
| the         | dt  |  40.62914 | 0.4889023 |  0 |    822 |    608 |  649.75101 | 462.991167 | 0.1385967 | 0.1397936 |
| by          | in  |  38.86344 | 1.7319071 |  0 |     96 |     30 |   75.88333 |  22.844959 | 0.1877124 | 0.1651132 |
| chinese     | jj  |  37.02291 | 5.7542749 |  0 |     26 |      0 |   20.55174 |   0.000000 | 0.5975812 |        NA |
| sauropods   | nns |  34.17500 | 5.6387977 |  0 |     24 |      0 |   18.97083 |   0.000000 | 0.7980397 |        NA |
| intensity   | nn  |  32.75104 | 5.5773972 |  0 |     23 |      0 |   18.18038 |   0.000000 | 0.7604898 |        NA |
| united      | nnp |  32.75104 | 5.5773972 |  0 |     23 |      0 |   18.18038 |   0.000000 | 0.5971860 |        NA |
| these       | dt  |  31.63882 | 3.5563356 |  0 |     34 |      3 |   26.87535 |   2.284496 | 0.3953349 | 0.6019647 |
| states      | nnp |  31.32708 | 5.5132668 |  0 |     22 |      0 |   17.38993 |   0.000000 | 0.5971860 |        NA |
| as          | in  |  30.26069 | 1.4791411 |  0 |     94 |     35 |   74.30243 |  26.652452 | 0.0836651 | 0.4012032 |

From that data, we can filter specific lexical classes, like modal
verbs:

|  Token | Tag |          LL |         LR |        PV | AF_Tar | AF_Ref | Per\_10.4\_Tar | Per\_10.4\_Ref |    DP_Tar |    DP_Ref |
|:------:|:---:|------------:|-----------:|----------:|-------:|-------:|-----------:|-----------:|----------:|----------:|
| may    | md  |   3.9877692 |  1.4323469 | 0.0458317 |     13 |      5 | 10.2758675 |  3.8074931 | 0.1648517 | 0.4012032 |
| will   | md  |   3.6584302 |  1.1412981 | 0.0557862 |     17 |      8 | 13.4376729 |  6.0919890 | 0.5387577 | 0.5992994 |
| ill    | md  |   1.4239582 |  1.0538352 | 0.2327530 |      1 |      0 |  0.7904513 |  0.0000000 | 0.7967750 |        NA |
| ought  | md  |   1.4239582 |  1.0538352 | 0.2327530 |      1 |      0 |  0.7904513 |  0.0000000 | 0.8004110 |        NA |
| must   | md  |   0.1321795 |  0.3168696 | 0.7161829 |      6 |      5 |  4.7427081 |  3.8074931 | 0.4305193 | 0.6019647 |
| wo     | md  |   0.0006962 |  0.0538352 | 0.9789499 |      1 |      1 |  0.7904513 |  0.7614986 | 0.7967750 | 0.7996497 |
| ca     | md  |  -0.1657799 | -0.5311273 | 0.6838899 |      2 |      3 |  1.5809027 |  2.2844959 | 0.7967750 | 0.5992994 |
| should | md  |  -0.2169360 | -0.3612023 | 0.6413845 |      6 |      8 |  4.7427081 |  6.0919890 | 0.2614813 | 0.3464819 |
| can    | md  |  -3.3401792 | -0.8041458 | 0.0676072 |     16 |     29 | 12.6472216 | 22.0834602 | 0.3475812 | 0.2237178 |
| might  | md  |  -3.6344817 | -1.9461648 | 0.0565942 |      2 |      8 |  1.5809027 |  6.0919890 | 0.5984507 | 0.6765535 |
| could  | md  |  -6.1468621 | -0.9129979 | 0.0131645 |     22 |     43 | 17.3899296 | 32.7444411 | 0.2592573 | 0.2407274 |
| would  | md  |  -9.2158449 | -1.3087349 | 0.0023993 |     14 |     36 | 11.0663189 | 27.4139507 | 0.3131317 | 0.2149795 |
| ’ll    | md  | -12.9666315 | -3.7535197 | 0.0003171 |      1 |     14 |  0.7904513 | 10.6609808 | 0.7967750 | 0.2393390 |
| ’d     | md  | -32.3838413 | -5.5311273 | 0.0000000 |      0 |     24 |  0.0000000 | 18.2759671 |        NA | 0.2340339 |

#### Extract phrases

We can also extract phrases of specific types. To so so, we first use
the function `as_phrasemachine()` to add a new column to our
annotation called `phrase_tag`.

``` r
annotation$phrase_tag <- as_phrasemachine(annotation$upos, type = "upos")
```

Next, we can use the function **keywords_phrases()** to extract
phrase-types based on regular expressions. Refer to the documentation
for [suggested **regex** patterns](https://www.rdocumentation.org/packages/udpipe/versions/0.8.6/topics/keywords_phrases).

You can also read [examples of use cases](https://bnosac.github.io/udpipe/docs/doc7.html).

First, we’ll subset our data into annotations by text-type.

``` r
acad_anno <- annotation %>% filter(str_detect(doc_id, "acad"))
fic_anno <- annotation %>% filter(str_detect(doc_id, "fic"))
```

``` r
acad_nps <- keywords_phrases(x = acad_anno$phrase_tag, term = tolower(acad_anno$token), 
                          pattern = "(A|N)*N(P+D*(A|N)*N)*", 
                          is_regex = TRUE, detailed = T)


fic_nps <- keywords_phrases(x = fic_anno$phrase_tag, term = tolower(fic_anno$token), 
                             pattern = "(A|N)*N(P+D*(A|N)*N)*", 
                             is_regex = TRUE, detailed = T)
```

| keyword                | ngram | pattern | start | end |
|------------------------|------:|---------|------:|----:|
| largest creatures      |     2 | AN      |     2 |   3 |
| creatures              |     1 | N       |     3 |   3 |
| earth                  |     1 | N       |     9 |   9 |
| animals                |     1 | N       |    11 |  11 |
| apatosaurus            |     1 | N       |    14 |  14 |
| aka                    |     1 | N       |    16 |  16 |
| aka brontosaurus       |     2 | NN      |    16 |  17 |
| brontosaurus           |     1 | N       |    17 |  17 |
| paleontologists        |     1 | N       |    19 |  19 |
| picture                |     1 | N       |    23 |  23 |
| they                   |     1 | N       |    26 |  26 |
| english anatomist      |     2 | AN      |    35 |  36 |
| anatomist              |     1 | N       |    36 |  36 |
| paleontologist         |     1 | N       |    39 |  39 |
| paleontologist richard |     2 | NN      |    39 |  40 |

Note that although the function uses the term **keywords**, it is
**NOT** executing a hypothesis test of any kind.

#### Extract only unique phrases

Note that **udpipe** extracts overlapping constituents of phrase structures. Normally, we would want only *unique* phrases. To find those we’ll take advantage of the **start** and **end** indexes, using the `between()` function from the **[data.table](https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html)** package.

That will generate a logical vector, which we can use to filter out only those phrases that don’t overlap with another.

``` r
idx <- seq(1:nrow(acad_nps))

is_unique <- lapply(idx, function(i) sum(data.table::between(acad_nps$start[i], acad_nps$start, acad_nps$end) & data.table::between(acad_nps$end[i], acad_nps$start, acad_nps$end)) == 1) %>% unlist()

acad_nps <- acad_nps[is_unique, ]
```

``` r
idx <- seq(1:nrow(fic_nps))

is_unique <- lapply(idx, function(i) sum(data.table::between(fic_nps$start[i], fic_nps$start, fic_nps$end) & data.table::between(fic_nps$end[i], fic_nps$start, fic_nps$end)) == 1) %>% unlist()

fic_nps <- fic_nps[is_unique, ]
```

We can also add a rough accounting of the lengths of the noun phrases by
summing the spaces and adding 1.

``` r
acad_nps <- acad_nps %>%
  mutate(phrase_length = str_count(keyword, " ") + 1)

fic_nps <- fic_nps %>%
  mutate(phrase_length = str_count(keyword, " ") + 1)
```

| keyword                   | ngram | pattern | start | end | phrase_length |
|---------------------------|------:|---------|------:|----:|--------------:|
| it                        |     1 | N       |     1 |   1 |             1 |
| pleasant summer night     |     3 | ANN     |     4 |   6 |             3 |
| wind off the ocean        |     4 | NPDN    |    10 |  13 |             4 |
| trees along copley square |     4 | NPNN    |    16 |  19 |             4 |
| i                         |     1 | N       |    21 |  21 |             1 |
| boston public library     |     3 | NNN     |    25 |  27 |             3 |
| square                    |     1 | N       |    31 |  31 |             1 |
| copley plaza bar          |     3 | NNN     |    37 |  39 |             3 |
| first time                |     2 | AN      |    44 |  45 |             2 |
| sammy                     |     1 | N       |    47 |  47 |             1 |
| piano                     |     1 | N       |    50 |  50 |             1 |
| his music                 |     2 | NN      |    53 |  54 |             2 |
| talk of the crowd         |     4 | NPDN    |    58 |  61 |             4 |
| sammy                     |     1 | N       |    64 |  64 |             1 |
| ease                      |     1 | N       |    68 |  68 |             1 |
