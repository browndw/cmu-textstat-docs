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

