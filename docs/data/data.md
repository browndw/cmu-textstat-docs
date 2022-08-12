# cmu.textstat data

## `federalist_meta`

### Description

Metadata for the *Federalist Papers*. This data is primarily used in replicating the classification problem undertaken by [Mosteller Wallace](https://www.jstor.org/stable/2283270) in attempting to identify t[he disputed authorship of several chapters](https://priceonomics.com/how-statistics-solved-a-175-year-old-mystery-about/).

The data frame contains 4 variables (`doc_id`, `author_id`, `title` and `date`) and 85 observations.

### Data snippet

| doc_id        | author_id | title                                                                            | date |
|---------------|-----------|----------------------------------------------------------------------------------|------|
| FEDERALIST_01 | Hamilton  | General Introduction                                                             | NA   |
| FEDERALIST_02 | Jay       | Concerning Dangers from Foreign Force and Influence                              | NA   |
| FEDERALIST_03 | Jay       | The Same Subject Continued (Concerning Dangers From Foreign Force and Influence) | NA   |
| FEDERALIST_04 | Jay       | The Same Subject Continued (Concerning Dangers From Foreign Force and Influence) | NA   |
| FEDERALIST_05 | Jay       | The Same Subject Continued (Concerning Dangers From Foreign Force and Influence) | NA   |
| FEDERALIST_06 | Hamilton  | Concerning Dangers from Dissensions Between the States                           | NA   |


## `federalist_papers`

### Description

The text of the *Federalist Papers*. The data frame contains 2 variables (`doc_id`, `text`) and 85 observations.

### Data snippet

| doc_id        | text                                                    |
|---------------|---------------------------------------------------------|
| FEDERALIST_01 | To the People of the State of New York: AFTER an u[...] |
| FEDERALIST_02 | To the People of the State of New York: WHEN the p[...] |
| FEDERALIST_03 | To the People of the State of New York: IT IS not [...] |
| FEDERALIST_04 | To the People of the State of New York: MY LAST pa[...] |
| FEDERALIST_05 | To the People of the State of New York: QUEEN ANNE[...] |
| FEDERALIST_06 | To the People of the State of New York: THE three [...] |

## `micusp_meta`

### Description

Metadata from the [Michigan Corpus of Upper-Level Student Papers (MICUSP)](https://elicorpora.info/main). The data frame contains 11 variables (`doc_id`, `paper_title`, `paper_discipline`, `student_level`, `discipline_cat`, `level_cat`, `student_gender`, `speaker_status` `speaker_l1` `paper_type`, `paper_features`) and 828 observations.

```{note}

The `doc_id` encodes metaata about the discipline (the first 3 capitalized letters) the student level (G0 to G3), student id, and the paper id associated with the student.

Thus, some analytical tasks don't require only the document ids and no additional data from this table.

```

### Data snippet

| doc_id      | paper_title                                                                                       | paper_discipline | student_level            | discipline_cat | level_cat | student_gender | speaker_status | speaker_l1 | paper_type | paper_features                                                                              |
|-------------|---------------------------------------------------------------------------------------------------|------------------|--------------------------|----------------|-----------|----------------|----------------|------------|------------|---------------------------------------------------------------------------------------------|
| BIO.G0.01.1 | The Ecology and Epidemiology of Plague                                                            | Biology          | Final Year Undergraduate | BIO            | G0        | F              | NS             | NA         | Report     | Literature review section, Reference to sources                                             |
| BIO.G0.02.1 | Host-Parasite Interactions: On the Presumed Sympatric Speciation of Vidua                         | Biology          | Final Year Undergraduate | BIO            | G0        | M              | NS             | NA         | Report     | Tables, graphs or figures, Reference to sources                                             |
| BIO.G0.02.2 | Sensory Drive and Speciation                                                                      | Biology          | Final Year Undergraduate | BIO            | G0        | M              | NS             | NA         | Report     | Reference to sources                                                                        |
| BIO.G0.02.3 | Plant Pollination Systems: Evolutionary Trends in Generalization and Specialization               | Biology          | Final Year Undergraduate | BIO            | G0        | M              | NS             | NA         | Report     | Reference to sources                                                                        |
| BIO.G0.02.4 | Chromosomal Rearrangements, Recombination Suppression, and Speciation: A Review of Rieseberg 2001 | Biology          | Final Year Undergraduate | BIO            | G0        | M              | NS             | NA         | Report     | Reference to sources                                                                        |
| BIO.G0.02.5 | On the Origins of Man: Understanding the Last Two Million Years                                   | Biology          | Final Year Undergraduate | BIO            | G0        | M              | NS             | NA         | Report     | Definitions, Discussion of results section, Tables, graphs or figures, Reference to sources |

## `micusp_mini`

### Description

The text of a subsmple of the MICUSP corpus. The data frame contains 2 variables (`doc_id`, `text`) and 170 observations (10 texts from each of 17 disciplines).


### Data snippet

| doc_id      | text                                                    |
|-------------|---------------------------------------------------------|
| BIO.G0.02.1 | Ernst Mayr once wrote, "sympatric speciation is li[...] |
| BIO.G0.03.1 | The ability of a species to compete for limited re[...] |
| BIO.G0.06.1 | Generally, females make a larger investment toward[...] |
| BIO.G0.12.1 | In the field of plant biology, one of the fundamen[...] |
| BIO.G0.21.1 | Parasites in nonhuman animals offer insight in und[...] |
| BIO.G0.25.1 | Malaria is caused by a parasite with the genus Pla[...] |

## `person_1st_pl`

### Description

Counts of 1st person plural pronouns (*we*, *us*, *our*, *ours*). The data come from [the Google Books data sets](https://storage.googleapis.com/books/ngrams/books/datasetsv3.html).

The data frame has 5 variables (`year`, `decade`, `token_count`, `total_count`, and `counts_permil`) and 210 observations beginning with 1800.

### Data snippet

| year | decade | token_count | total_count | counts_permil |
|------|--------|-------------|-------------|---------------|
| 1800 | 1800   | 104442      | 18412019    | 5672.49       |
| 1801 | 1800   | 126814      | 19941239    | 6359.38       |
| 1802 | 1800   | 115818      | 23355869    | 4958.84       |
| 1803 | 1800   | 170391      | 27922721    | 6102.23       |
| 1804 | 1800   | 134866      | 36024446    | 3743.74       |
| 1805 | 1800   | 154443      | 28285597    | 5460.13       |

## `person_1st_sing`

### Description

Counts of 1st person plural pronouns (*I*, *me*, *my*, *mine*). The data come from [the Google Books data sets](https://storage.googleapis.com/books/ngrams/books/datasetsv3.html).

The data frame has 5 variables (`year`, `decade`, `token_count`, `total_count`, and `counts_permil`) and 210 observations beginning with 1800.

### Data snippet

| year | decade | token_count | total_count | counts_permil |
|------|--------|-------------|-------------|---------------|
| 1800 | 1800   | 236846      | 18412019    | 12863.66      |
| 1801 | 1800   | 200603      | 19941239    | 10059.71      |
| 1802 | 1800   | 162318      | 23355869    | 6949.77       |
| 1803 | 1800   | 183530      | 27922721    | 6572.78       |
| 1804 | 1800   | 170764      | 36024446    | 4740.23       |
| 1805 | 1800   | 244560      | 28285597    | 8646.1        |

## `person_2nd`

### Description

Counts of 2nd person pronouns (*you*, *your*, *yours*). The data come from [the Google Books data sets](https://storage.googleapis.com/books/ngrams/books/datasetsv3.html).

The data frame has 5 variables (`year`, `decade`, `token_count`, `total_count`, and `counts_permil`) and 210 observations beginning with 1800.

### Data snippet

| year | decade | token_count | total_count | counts_permil |
|------|--------|-------------|-------------|---------------|
| 1800 | 1800   | 98483       | 18412019    | 5348.84       |
| 1801 | 1800   | 78919       | 19941239    | 3957.58       |
| 1802 | 1800   | 69956       | 23355869    | 2995.22       |
| 1803 | 1800   | 91388       | 27922721    | 3272.89       |
| 1804 | 1800   | 80542       | 36024446    | 2235.76       |
| 1805 | 1800   | 79699       | 28285597    | 2817.65       |


## `sentiment_data`

### Description

The text of four novels: *[Madame Bovary](https://www.gutenberg.org/ebooks/2413)*, *[A Portrait of the Artist as a Young Man](https://www.gutenberg.org/ebooks/4217)*, *[Ragged Dick](https://www.gutenberg.org/ebooks/5348)*, and *[The Rise of Silas Lapham](https://www.gutenberg.org/ebooks/154)*. The data are included to explore the **[syuzhet](https://cran.r-project.org/web/packages/syuzhet/vignettes/syuzhet-vignette.html)** R package and [its applications](https://www.matthewjockers.net/2015/02/02/syuzhet/) to [literary works](http://www.digitalhumanities.org/dhq/vol/16/2/000612/000612.html).

The data frame contains 2 variables (`doc_id`, `text`) and 4 observations.


### Data snippet

| doc_id          | text                                                    |
|-----------------|---------------------------------------------------------|
| madame_bovary   | Part I Chapter One We were in class when the head-[...] |
| portrait_artist | Chapter I Once upon a time and a very good time it[...] |
| ragged_dick     | CHAPTER I RAGGED DICK IS INTRODUCED TO THE READER [...] |
| silas_lapham    | I. WHEN Bartley Hubbard went to interview Silas La[...] |

## `shakespeare_corpus`

### Description

A corpus of plays by Shakespeare. The data frame contains 2 variables (`doc_id`, `text`) and 37 observations.

```{note}

The `doc_id` encodes metaata about the genre of play (whether comedy, historical, or tragedy)

```

```{warning}

Unlike most text data, this data uses simple markup to identify stage directions, dialogue, and speakers.

Data can be extracted using the `from_play` [function](https://cmu-textstat-docs.readthedocs.io/en/latest/functions/functions.html#from-play).

```


### Data snippet

| doc\_id                                | text                                                                                                             |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------|
| comedies\_a\_midsummernights\_dream    | &lt;DIRECTION&gt; Scene.--Athens, and a Wood near it. &lt;/DIRECTION&gt; &lt;ACT\_1&gt; &lt;SCENE[...]           |
| comedies\_alls\_well\_that\_ends\_well | &lt;DIRECTION&gt; Scene.--Rousillon, Paris, Florence, Marseilles. &lt;/DIRECTION&gt; &lt;A[...]                  |
| comedies\_as\_you\_like\_it            | &lt;DIRECTION&gt; Scene.--First, Oliver's Orchard near his House; afterwards,... [...]                           |
| comedies\_cymbeline                    | &lt;DIRECTION&gt; Scene.--Sometimes in Britain, sometimes in Italy. &lt;/DIRECTION&gt; [...]                     |
| comedies\_loves\_labours\_lost         | &lt;DIRECTION&gt; Scene.--Navarre. &lt;/DIRECTION&gt; &lt;ACT\_1&gt; &lt;SCENE\_1&gt; &lt;DIRECTION&gt; The[...] |
| comedies\_measure\_for\_measure        | &lt;DIRECTION&gt; Scene.--Vienna. &lt;/DIRECTION&gt; &lt;ACT\_1&gt; &lt;SCENE\_1&gt; &lt;DIRECTION&gt; An A[...] |

The data uses simple markup, as in the beginning of *Hamlet*:

```xml

<DIRECTION>
Scene.--Elsinore.
</DIRECTION>
<ACT_1>
<SCENE_1>
<DIRECTION>
Elsinore. A Platform before the Castle.
</DIRECTION>
<DIRECTION>
Francisco at his post. Enter to him Bernardo.
</DIRECTION>
<BERNARDO>
	<DIALOGUE>
	Who's there?
	</DIALOGUE>
</BERNARDO>
<FRANCISCO>
	<DIALOGUE>
	Nay, answer me; stand, and unfold yourself.
	</DIALOGUE>
</FRANCISCO>
<BERNARDO>
	<DIALOGUE>
	Long live the king!
	</DIALOGUE>
</BERNARDO>
<FRANCISCO>
	<DIALOGUE>
	Bernardo?
	</DIALOGUE>
</FRANCISCO>


```