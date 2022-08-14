# Set 01

The goal of this first lab set is to introduce some fundamental principles of:

* Processing pipelines
* Data presentation and document design
* The affordances and limitations of text data
* The decisions required at every step

## Lab 1: Texts, algorithms, and black-boxes

We’re going to start by unpacking the controversy regarding the **[syuzhet](https://cran.r-project.org/web/packages/syuzhet/vignettes/syuzhet-vignette.html)** R package. (The readings are short and posted on Canvas.) This is a useful exercise, I think, because it gets to some foundational issues in text analysis -- you’re going to encounter them in your work so they’re worth considering from the beginning.

```{note}

When preparing reports, it is often useful to number your sections. It allows you to point your reader to specific places in your report: *See Section 2.1*. This is [easy to accomplish in Rmarkdown](https://bookdown.org/yihui/rmarkdown-cookbook/unnumbered-sections.html).

```
### Load the cmu.textstat package

Load the package, as well as others that we’ll use in this short lab.

``` r
library(cmu.textstat)
library(tidyverse)
library(syuzhet)
```

The novels that Jockers uses as examples are [included as data](https://cmu-textstat-docs.readthedocs.io/en/latest/data/data.html#sentiment-data), which can be accessed as `sentiment_data`. There are 4 novels, and we’ll check their names stored in the `doc_id` column.

For this demonstration, we’ll be using *Madame Bovary*.

| doc_id          |
|:----------------|
| madame_bovary   |
| portrait_artist |
| ragged_dick     |
| silas_lapham    |


### Prep the data and calculate sentiment

Next, we do some simple cleaning using **str_squish** from the
**stringr** package. Then we’ll split the the novel into sentences and calculate a sentiment score for each.

``` r

# str_squish() is a useful function from readr for getting rid of
# extra spaces, carriage returns, etc.
mb <- str_squish(sentiment_data$text[1])

# chunk the novel into sentences
mb_sentences <- get_sentences(mb)

# calculate and return sentiment scores
mb_sentiment <- get_sentiment(mb_sentences)
```

Let’s check the data:

|      |
|-----:|
| 1.20 |
| 0.25 |
| 0.00 |
| 1.50 |
| 1.05 |
| 1.20 |


### Transforming the data

The next step is to transform the data. Originally, Jockers used a Fourier transformation, which he described as follows:

> Aaron introduced me to a mathematical formula from signal processing called the Fourier transformation. The Fourier transformation provides a way of decomposing a time based signal and reconstituting it in the frequency domain. A complex signal (such as the one seen above in the first figure in this post) can be decomposed into series of symmetrical waves of varying frequencies. And one of the magical things about the Fourier equation is that these decomposed component sine waves can be added back together (summed) in order to reproduce the original wave form–this is called a backward or reverse transformation. Fourier provides a way of transforming the sentiment-based plot trajectories into an equivalent data form that is independent of the length of the trajectory from beginning to end. The frequency domain begins to solve the book length problem.

This introduced some unwanted outcomes, namely that the resulting
wave-forms must begin and end at the same point. The updated function uses a Discrete Cosine Transform (DCT), which is commonly used in data compression.

``` r
mb_dct <- get_dct_transform(mb_sentiment, low_pass_size = 5, x_reverse_len = 100,
    scale_vals = FALSE, scale_range = TRUE)

mb_dct <- data.frame(dct = mb_dct) %>%
    rownames_to_column("time") %>%
    mutate(time = as.numeric(time))
```

Check the data:

| time |       dct |
|-----:|----------:|
|    1 | 1.0000000 |
|    2 | 0.9974733 |
|    3 | 0.9924392 |
|    4 | 0.9849363 |
|    5 | 0.9750217 |
|    6 | 0.9627711 |

Sample transformed scores.

Finally, the values can be plotted.

``` r
plot(mb_dct, type = "l", xlab = "Narrative Time", ylab = "Emotional Valence",
    col = "red")
```

![Sentiment in using transformed
values.](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/labs_files/figure-gfm/unnamed-chunk-6-1.png)

```{note}

Tables and figures should always be numbered and [captioned](https://stackoverflow.com/questions/31926623/figures-captions-and-labels-in-knitr). As with section number, this allows you to refer to tables and figures by number: *see Figure 2* or *Figure 3.1 illustrates...*

```

### Transformed vs. non-transformed data

In order to better compare the before vs. after, let’s create a data frame in which we normalize the narrative time values and scale the sentiment scores.

``` r
mb_df <- mb_sentiment %>%
    data.frame(sentiment = .) %>%
    rownames_to_column("time") %>%
    mutate(time = as.numeric(time)) %>%
    mutate(time = time/length(mb_sentiment) * 100) %>%
    mutate(sentiment = 2 * (sentiment - min(sentiment))/(max(sentiment) -
        min(sentiment)) - 1)
```

Now, those values can be plotted with the values extracted from DCT.

``` r
ggplot(data = mb_dct, aes(x = time, y = dct)) + geom_line(colour = "tomato") +
    geom_point(data = mb_df, aes(x = time, y = sentiment), alpha = 0.25,
        size = 0.25) + xlab("Normalized Narrative Time") + ylab("Scaled Sentiment") +
    theme_minimal()
```

![Transformed vs. non-transformed sentiment in
](https://raw.githubusercontent.com/browndw/cmu-textstat-docs/main/docs/_static/labs_files/figure-gfm/year_plot-1.png)

### What questions does this example raise for you?

This process raises any number of potential questions: about sentiment analysis, about the choice of procedures, about their application to particular kinds of data, to the very choice of the data itself. In asking you to posit your questions, the goal is not to reach any summative conclusion as to whether the **syuzhet** package is “good” or “bad.” Rather it is to have us think through all of the decisions that we make as analysts (or that get made for us inside R packages, software, etc.), and what we might want to know or test in order to advance and defend our conclusions.

## Lab 2: The basics

### A simple processing pipeline

Let’s begin by creating an object consisting of a character string. In
this case, the first sentence from *A Tale of Two Cities*.

``` r
totc_txt <- "It was the best of times, it was the worst of times, it was the age of wisdom, it was the age of foolishness, it was the epoch of belief, it was the epoch of incredulity, it was the season of Light, it was the season of Darkness, it was the spring of hope, it was the winter of despair."
```

And we’ll load the tidyverse libraries.

``` r
library(tidyverse)
```

We could then split the vector, say at each space.

``` r
totc_tkns <- totc_txt %>%
    str_split(" ")
```

Then, we can create a table of counts.

``` r
totc_df <- table(totc_tkns) %>% # make a table of counts
  as_tibble() %>%
  rename(Token = totc_tkns, AF = n) %>% # rename columns
  arrange(-AF) # sort the data by frequency
```

| Token |  AF |
|:------|----:|
| of    |  10 |
| the   |  10 |
| was   |  10 |
| it    |   9 |
| age   |   2 |
| epoch |   2 |

Token counts of sample sentence.

The process of splitting the string vector into constituent parts is called **tokenizing**. Think of this as telling the computer how to define a word (or a “token”, which is a more precise, technical term). In this case, we’ve done it in an extremely simple way–by defining a token as any string that is bounded by spaces.

| Token |  AF |
|:------|----:|
| it    |   9 |
| It    |   1 |

Case sensitive counts of the token *it*.

Note that in doing so, we are counting capitalized and non-capitalized words as distinct tokens.

There may be specific instances when we want to do this. But normally, we’d want *it* and *It* to be the same token. To do that, we can add a step in the processing pipeline that converts our vector to lower case before tokenizing.

``` r
totc_df <- tolower(totc_txt) %>%
  str_split(" ") %>%
  table() %>% # make a table of counts
  as_tibble() %>%
  rename(Token = ".", AF = n) %>% # rename columns
  arrange(-AF) # sort the data by frequency
```

| Token |  AF |
|:------|----:|
| it    |  10 |
| of    |  10 |
| the   |  10 |
| was   |  10 |
| age   |   2 |
| epoch |   2 |

Token counts of sample sentence.

### What counts as a token?

These choices are important. To carry our any statistical analysis on texts, we radically reorganize texts into counts. Precisely how we choose to do that -- the decisions we make in exactly **what** to count -- affects everything else downstream.

For example, you might consider if you want to include numbers? Or symbols? Punctuation? How would you handle hyphenated words? Contractions?

In this short example, how would want your tokenization to handle appreviations like *U.S.*?

> In spite of some problems, we saw a 35% uptick in our user-base in the  U.S. But that’s still a lot fewer than we had last year. We’d like to get that number closer to what we’ve experienced in the U.K.–something close to 3 million users.

In one common process, we could convert everything to lowercase and remove puctuation. We would, then, end up with the token *us*. Is that acceptable, given the data and analytical task?

That said, we also need to bear in mind that as analysts, we are often building models, creating representations, or testing hypotheses. Under most conditions, there will **always** be error introduced through the collection and preparation of our text data.

The point (generally) is not to perfectly mimic what our human brain understands to be a "word." It is to make decisions that best serve our analytical task. Sometimes, that might mean tokenizing in a way that [takes advange of computational power, but is decidely more difficult for a human reader](https://ai.googleblog.com/2021/12/a-fast-wordpiece-tokenization-system.html).


