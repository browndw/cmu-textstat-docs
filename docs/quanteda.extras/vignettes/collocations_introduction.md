# Introduction to collocation functions in the quanteda.extras R package

## Load the quanteda.extras package

Load the package, as well as others that we’ll use in this vignette.

``` r
library(quanteda.extras)
library(quanteda)
library(tidyverse)
library(ggraph)
```

## Prepare the data

First, we’ll use to `preprocess_text()` function to “clean” the text
data. See the **preprocess vignette** for more information.

``` r
sc <- sample_corpus %>%
  mutate(text = preprocess_text(text))
```

Next, create a corpus and tokenize the data:

``` r
sc_tokens <- sc %>%
  corpus() %>% # create a corpus object
  tokens(what="fastestword", remove_numbers=TRUE) # tokenize the data
```

## Collocates by mutual information

The `collocates_by_MI()` function produces collocation measures (by pointwise mutual information) for a specified token in a **quanteda tokens** object. In addition to a token, a span or window (as given by a number of words to the **left** and **right** of the **node word**) is required. The default is 5 to the left and 5 to the right.

We’ll start by making a table of tokens that collocate with the token
*money*.

``` r
money_collocations <- collocates_by_MI(sc_tokens, "money")
```

Check the result:

<table>
<thead>
<tr>
<th style="text-align:left;">
token
</th>
<th style="text-align:right;">
col_freq
</th>
<th style="text-align:right;">
total_freq
</th>
<th style="text-align:right;">
MI_1
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
10:29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
11.08049
</td>
</tr>
<tr>
<td style="text-align:left;">
38th
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
11.08049
</td>
</tr>
<tr>
<td style="text-align:left;">
allocations
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
11.08049
</td>
</tr>
<tr>
<td style="text-align:left;">
americanizing
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
11.08049
</td>
</tr>
<tr>
<td style="text-align:left;">
anthedon
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
11.08049
</td>
</tr>
<tr>
<td style="text-align:left;">
assignats
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
11.08049
</td>
</tr>
</tbody>
</table>

Now, let’s make a similar table for collocates of *time*.

``` r
time_collocations <- collocates_by_MI(sc_tokens, "time")
```

<table>
<thead>
<tr>
<th style="text-align:left;">
token
</th>
<th style="text-align:right;">
col_freq
</th>
<th style="text-align:right;">
total_freq
</th>
<th style="text-align:right;">
MI_1
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
decleat
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
10.135473
</td>
</tr>
<tr>
<td style="text-align:left;">
poignantly
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
10.135473
</td>
</tr>
<tr>
<td style="text-align:left;">
16a
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.135473
</td>
</tr>
<tr>
<td style="text-align:left;">
17a
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.135473
</td>
</tr>
<tr>
<td style="text-align:left;">
21h
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.135473
</td>
</tr>
<tr>
<td style="text-align:left;">
aba
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.135473
</td>
</tr>
</tbody>
</table>

As is clear from the above table, MI is sensitive to rare/infrequent
words. Because of that sensitivity, it commmon to make thresholds for
both token frequency (absolute frequency) and MI score (ususally at some
value ≥ 3).

For our purposes, we’ll filter for AF ≥ 5 and MI ≥ 5.

``` r
tc <- time_collocations %>% filter(col_freq >= 5 & MI_1 >= 5)
mc <- money_collocations %>% filter(col_freq >= 5 & MI_1 >= 5)
```

Check the result:

<table>
<thead>
<tr>
<th style="text-align:left;">
token
</th>
<th style="text-align:right;">
col_freq
</th>
<th style="text-align:right;">
total_freq
</th>
<th style="text-align:right;">
MI_1
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
warner
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8.720436
</td>
</tr>
<tr>
<td style="text-align:left;">
cessation
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
8.650046
</td>
</tr>
<tr>
<td style="text-align:left;">
irradiation
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
8.650046
</td>
</tr>
<tr>
<td style="text-align:left;">
lag
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
8.650046
</td>
</tr>
<tr>
<td style="text-align:left;">
wasting
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
11
</td>
<td style="text-align:right;">
8.483396
</td>
</tr>
<tr>
<td style="text-align:left;">
frame
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
16
</td>
<td style="text-align:right;">
7.942828
</td>
</tr>
</tbody>
</table>
<table>
<thead>
<tr>
<th style="text-align:left;">
token
</th>
<th style="text-align:right;">
col_freq
</th>
<th style="text-align:right;">
total_freq
</th>
<th style="text-align:right;">
MI_1
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
owe
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
21
</td>
<td style="text-align:right;">
9.010102
</td>
</tr>
<tr>
<td style="text-align:left;">
raise
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
79
</td>
<td style="text-align:right;">
8.098639
</td>
</tr>
<tr>
<td style="text-align:left;">
extra
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
64
</td>
<td style="text-align:right;">
7.665454
</td>
</tr>
<tr>
<td style="text-align:left;">
spend
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
111
</td>
<td style="text-align:right;">
7.608004
</td>
</tr>
<tr>
<td style="text-align:left;">
insurance
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
64
</td>
<td style="text-align:right;">
7.402420
</td>
</tr>
<tr>
<td style="text-align:left;">
spent
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
122
</td>
<td style="text-align:right;">
7.319679
</td>
</tr>
</tbody>
</table>

## Create a tbl_graph object for plotting

A [**tbl_graph**](https://www.data-imaginist.com/2017/introducing-tidygraph/) is a data structure for **tidyverse** (ggplot2) network plotting.

For this, we’ll use the `col_network()` function.

``` r
net <- col_network(tc, mc)
```

## Plot network

The network plot shows the tokens that distinctly collocate with either *time* or *money*, as well as those that interect. The distance from the central tokens (*time* and *money*) is governed by the MI score and the transparency (or alpha) is governed by the token frequency.

The aesthetic details of the plot can be manipulated in the various **ggraph** options.

``` r
ggraph(net, weight = link_weight, layout = "stress") + 
  geom_edge_link(color = "gray80", alpha = .75) + 
  geom_node_point(aes(alpha = node_weight, size = 3, color = n_intersects)) +
  geom_node_text(aes(label = label), repel = T, size = 3) +
  scale_alpha(range = c(0.2, 0.9)) +
  theme_graph() +
  theme(legend.position="none")
```

![](/Users/davidwestbrown/Desktop/cmu-textstat-docs/quanteda.extras/vignettes/collocations_introduction_files/figure-gfm/net_plot-1.png)<!-- -->

## Bibliography

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-brezina2018collocation" class="csl-entry">

Brezina, Vaclav. 2018. “Collocation Graphs and Networks: Selected
Applications.” In *Lexical Collocation Analysis*, 59–83. Springer.
<https://link.springer.com/chapter/10.1007/978-3-319-92582-0_4>.

</div>

<div id="ref-brezina2015collocations" class="csl-entry">

Brezina, Vaclav, Tony McEnery, and Stephen Wattam. 2015. “Collocations
in Context: A New Perspective on Collocation Networks.” *International
Journal of Corpus Linguistics* 20 (2): 139–73.
<https://www.jbe-platform.com/content/journals/10.1075/ijcl.20.2.01bre>.

</div>

<div id="ref-church1990word" class="csl-entry">

Church, Kenneth, and Patrick Hanks. 1990. “Word Association Norms,
Mutual Information, and Lexicography.” *Computational Linguistics* 16
(1): 22–29. <https://aclanthology.org/J90-1003.pdf>.

</div>

</div>
