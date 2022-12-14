# pseudobibeR

[The R scipts in this repository](https://github.com/browndw/pseudobibeR) aggregate the lexicogrammatical and functonal features described by Biber (1985) and widely used for text-type, register, and genre classification tasks.

The scripts are not really taggers. Rather, they use either [udpipe](https://bnosac.github.io/udpipe/en/) or [spaCy](https://spacy.io/) part-of-speech tagging and dependency parsing to summarize and aggregate patterns.

Because they rely on pobablistic taggers, the accuracy of the resulting counts are dependent on the accuracy of those models. Thus, texts with irregular spellings, non-normative punctuation, etc. will likely produce unreliable outputs, unless taggers are tuned specifically for those puposes.

## Installing and running pseudobibeR

Use devtools to install the package.

```r
devtools::install_github("browndw/pseudobibeR")
```

The main parsing function requires text processed using either **[udpipe](https://bnosac.github.io/udpipe/docs/doc1.html)** or **[spacyr](https://cran.r-project.org/web/packages/spacyr/vignettes/using_spacyr.html)**. See their package documentation for installation and usage guidelines.

### With udpipe

```r
library(udpipe)
library(pseudobibeR)

# For demonstration purposes, take the first 10 texts from data from cmu.textstat
df <- cmu.textstat::micusp_mini[1:10,]

# Initialize the model
# To download the model: udpipe_download_model(language = "english")
ud_model <- udpipe_load_model("english-ewt-ud-2.5-191206.udpipe")

# Parse the data
micusp_prsd <- udpipe_annotate(ud_model, x = df$text, doc_id = df$doc_id)

# Convert to a data frame
micusp_prsd <- data.frame(micusp_prsd, stringsAsFactors = F)

# Aggregate the tags from dependency structures and parts-of-speech
df_biber <- biber_udpipe(micusp_prsd)

```

### With spacyr

```{note}

Unlike **udpipe**, **spacyr** is not a native R package. It installs a conda virtual environment (called **spacy_condaenv**) and uses [reticulate](https://rstudio.github.io/reticulate/) to interface with Python.

Thus, parsing data with a spaCy model from R requires a potentially more complicated installation.

However, spaCy is a more feature-rich and flexible model than udpipe (though not necessarily more accurate). If you are familiar and comfortable with Python, it may be worth your time. Otherwise, udpipe provides a robust native R alternative.

```

```r
library(spacyr)
library(pseudobibeR)

spacy_initialize()

# For demonstration purposes, take the first 10 texts from data from cmu.textstat
df <- cmu.textstat::micusp_mini[1:10,]

# Create a corpus object
micusp_corpus <- quanteda::corpus(df])

# Parse using spaCy; note that we need dependency set to TRUE.
micusp_prsd <- spacy_parse(micusp_corpus, pos = T, tag = T, dependency = T, entity = F)

# # Aggregate the tags from dependency structures and parts-of-speech
df_biber <- biber_spacy(micusp_prsd)
```

## Categories

The following table is adapted from one created by [Stefan Evert](https://www.rdocumentation.org/packages/corpora/versions/0.5/topics/BNCbiber).

```{note}

Counts are normalized **per 1000 tokens**.

Type-to-token is calculated using a [moving-average type-to-token ration (MATTR)](https://www.tandfonline.com/doi/abs/10.1080/09296171003643098).

```

| Feature | Description           |
|--------|------------------------|
| - | **A. Tense and aspect markers** |
| f\_01\_past\_tense | Past tense |
| f\_02\_perfect\_aspect | Perfect aspect |
| f\_03\_present\_tense | Present tense |
| - | **B. Place and time adverbials** |
| f\_04\_place\_adverbials | Place adverbials (e.g., *above*, *beside*, *outdoors*) |
| f\_05\_time\_adverbials | Time adverbials (e.g., *early*, *instantly*, *soon*) |
| - | **C. Pronouns and pro-verbs** |
| f\_06\_first\_person\_pronouns | First-person pronouns |
| f\_07\_second\_person\_pronouns | Second-person pronouns |
| f\_08\_third\_person\_pronouns | Third-person personal pronouns (excluding it) |
| f\_09\_pronoun\_it | Pronoun *it* |
| f\_10\_demonstrative\_pronoun | Demonstrative pronouns (*that*, *this*, *these*, *those* as pronouns) |
| f\_11\_indefinite\_pronoun | Indefinite pronounes (e.g., *anybody*, *nothing*, *someone*) |
| f\_12\_proverb\_do | Pro-verb *do* |
| - | **D. Questions** |
| f\_13\_wh\_question | Direct *wh*-questions |
| - | **E. Nominal forms** |
| f\_14\_nominalization | Nominalizations (ending in -*tion*, -*ment*, -*ness*, -*ity*) |
| f\_15\_gerunds | Gerunds (participial forms functioning as nouns) |
| f\_16\_other\_nouns | Total other nouns |
| - | **F. Passives** |
| f\_17\_agentless\_passives | Agentless passives |
| f\_18\_by\_passives | *by*-passives |
| - | **G. Stative forms** |
| f\_19\_be\_main\_verb | *be* as main verb |
| f\_20\_existential\_there | Existential *there* |
| - | **H. Subordination features** |
| f\_21\_that\_verb\_comp | that verb complements (e.g., *I said [that he went]*.) |
| f\_22\_that\_adj\_comp | that adjective complements (e.g., *I'm glad [that you like it]*.) |
| f\_23\_wh\_clause | *wh*-clauses (e.g., *I believed [what he told me]*.) |
| f\_24\_infinitives | Infinitives |
| f\_25\_present\_participle | Present participial adverbial clauses (e.g., *[Stuffing his mouth with cookies], Joe ran out the door*.) |
| f\_26\_past\_participle | Past participial adverbial clauses (e.g., *[Built in a single week], the house would stand for fifty years*.) |
| f\_27\_past\_participle\_whiz | Past participial postnominal (reduced relative) clauses (e.g., *the solution [produced by this process]*) |
| f\_28\_present\_participle\_whiz | Present participial postnominal (reduced relative) clauses (e.g., *the event [causing this decline[*) |
| f\_29\_that\_subj | *that* relative clauses on subject position (e.g., *the dog [that bit me]*) |
| f\_30\_that\_obj | *that* relative clauses on object position (e.g., *the dog [that I saw]*) |
| f\_31\_wh\_subj | *wh*- relatives on subject position (e.g., *the man [who likes popcorn]*) |
| f\_32\_wh\_obj | *wh*- relatives on object position (e.g., *the man [who Sally likes]*) |
| f\_33\_pied\_piping | Pied-piping relative clauses (e.g., *the manner [in which he was told]*) |
| f\_34\_sentence\_relatives | Sentence relatives (e.g., *Bob likes fried mangoes, [which is the most disgusting thing I've ever heard of]*.) |
| f\_35\_because | Causative adverbial subordinator (*because*) |
| f\_36\_though | Concessive adverbial subordinators (*although*, *though*) |
| f\_37\_if | Conditional adverbial subordinators (*if*, *unless*) |
| f\_38\_other\_adv\_sub | Other adverbial subordinators (e.g., *since*, *while*, *whereas*) |
| - | **I. Prepositional phrases, adjectives and adverbs** |
| f\_39\_prepositions | Total prepositional phrases |
| f\_40\_adj\_attr | Attributive adjectives (e.g., *the [big] horse*) |
| f\_41\_adj\_pred | Predicative adjectives (e.g., *The horse is [big]*.) |
| f\_42\_adverbs | Total adverbs |
| - | **J. Lexical specificity** |
| f\_43\_type\_token | Type-token ratio (including punctuation) |
| f\_44\_mean\_word\_length | Average word length (across tokens, excluding punctuation) |
| - | **K. Lexical classes** |
| f\_45\_conjuncts | Conjuncts (e.g., *consequently*, *furthermore*, *however*) |
| f\_46\_downtoners | Downtoners (e.g., *barely*, *nearly*, *slightly*) |
| f\_47\_hedges | Hedges (e.g., *at about*, *something like*, *almost*) |
| f\_48\_amplifiers | Amplifiers (e.g., *absolutely*, *extremely*, *perfectly*) |
| f\_49\_emphatics | Emphatics (e.g., *a lot*, *for sure*, *really*) |
| f\_50\_discourse\_particles | Discourse particles (e.g., sentence-initial *well*, *now*, *anyway*) |
| f\_51\_demonstratives | Demonstratives |
| - | **L. Modals** |
| f\_52\_modal\_possibility | Possibility modals (*can*, *may*, *might*, *could*) |
| f\_53\_modal\_necessity | Necessity modals (*ought*, *should*, *must*) |
| f\_54\_modal\_predictive | Predictive modals (*will*, *would*, *shall*) |
| - | **M. Specialized verb classes** |
| f\_55\_verb\_public | Public verbs (e.g., *assert*, *declare*, *mention*) |
| f\_56\_verb\_private | Private verbs (e.g., *assume*, *believe*, *doubt*, *know*) |
| f\_57\_verb\_suasive | Suasive verbs (e.g., *command*, *insist*, *propose*) |
| f\_58\_verb\_seem | *seem* and *appear* |
| - | **N. Reduced forms and dispreferred structures** |
| f\_59\_contractions | Contractions |
| f\_60\_that\_deletion | Subordinator that deletion (e.g., *I think [he went]*.) |
| f\_61\_stranded\_preposition | Stranded prepositions (e.g., *the candidate that I was thinking [of]*) |
| f\_62\_split\_infinitve | Split infinitives (e.g., *He wants [to convincingly prove] that* ???) |
| f\_63\_split\_auxiliary | Split auxiliaries (e.g., *They [were apparently shown] to* ???) |
| - | **O. Co-ordination** |
| f\_64\_phrasal\_coordination | Phrasal co-ordination (N and N; Adj and Adj; V and V; Adv and Adv) |
| f\_65\_clausal\_coordination | Independent clause co-ordination (clause-initial *and*) |
| - | **P. Negation** |
| f\_66\_neg\_synthetic | Synthetic negation (e.g., *No answer is good enough for Jones*.) |
| f\_67\_neg\_analytic | Analytic negation (e.g., *That isn't good enough*.) |

## Functions

```{toctree}
:maxdepth: 2

functions/functions.md
```

## Data

```{toctree}
:maxdepth: 2

data/data.md
```

## References

Biber, Douglas (1988). Variations Across Speech and Writing. Cambridge University Press, Cambridge.

Biber, Douglas (1995). Dimensions of Register Variation: A cross-linguistic comparison. Cambridge University Press, Cambridge.

Gasthaus, Jan (2007). Prototype-Based Relevance Learning for Genre Classification. B.Sc.\ thesis, Institute of Cognitive Science, University of Osnabr<U+00FC>ck. Data sets and software available from [http://cogsci.uni-osnabrueck.de/~CL/download/BSc_Gasthaus2007/](http://cogsci.uni-osnabrueck.de/~CL/download/BSc_Gasthaus2007/).
