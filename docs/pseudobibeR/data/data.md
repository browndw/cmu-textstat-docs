# pseudobibeR data

## `spacyr_parse`

### Description

The [MICUSP mini](https://cmu-textstat-docs.readthedocs.io/en/latest/data/data.html#micusp-mini) corpus as parsed by **[spacyr](https://spacyr.quanteda.io/)**.


### Data snippet

| doc_id          | sentence_id | token_id | token | lemma | pos   | tag | head_token_id | dep_rel  |
|-----------------|-------------|----------|-------|-------|-------|-----|---------------|----------|
| BIO.G0.02.1.txt | 1           | 1        | Ernst | Ernst | PROPN | NNP | 2             | compound |
| BIO.G0.02.1.txt | 1           | 2        | Mayr  | Mayr  | PROPN | NNP | 4             | nsubj    |
| BIO.G0.02.1.txt | 1           | 3        | once  | once  | ADV   | RB  | 4             | advmod   |
| BIO.G0.02.1.txt | 1           | 4        | wrote | write | VERB  | VBD | 4             | ROOT     |
| BIO.G0.02.1.txt | 1           | 5        | ,     | ,     | PUNCT | ,   | 4             | punct    |
| BIO.G0.02.1.txt | 1           | 6        | "     | "     | PUNCT | ``  | 4             | punct    |


## `udpipe_parse`

### Description

The [MICUSP mini](https://cmu-textstat-docs.readthedocs.io/en/latest/data/data.html#micusp-mini) corpus as parsed by **[udpipe](https://bnosac.github.io/udpipe/docs/doc0.html)**.


### Data snippet

| doc_id          | paragraph_id | sentence_id | sentence                                                                                                                                                        | token_id | token | lemma | upos  | xpos | feats                              | head_token_id | dep_rel       | deps | misc          |
|-----------------|--------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------|-------|-------|------|------------------------------------|---------------|---------------|------|---------------|
| BIO.G0.02.1.txt | 1            | 1           | Ernst Mayr once wrote, "sympatric speciation is like the Lernaean Hydra which grew two new heads whenever one of its old heads was cut off" (1963:451). | 1        | Ernst | Ernst | PROPN | NNP  | Number=Sing                        | 4             | nsubj         | NA   | NA            |
| BIO.G0.02.1.txt | 1            | 1           | Ernst Mayr once wrote, "sympatric speciation is like the Lernaean Hydra which grew two new heads whenever one of its old heads was cut off" (1963:451). | 2        | Mayr  | Mayr  | PROPN | NNP  | Number=Sing                        | 1             | flat          | NA   | NA            |
| BIO.G0.02.1.txt | 1            | 1           | Ernst Mayr once wrote, "sympatric speciation is like the Lernaean Hydra which grew two new heads whenever one of its old heads was cut off" (1963:451). | 3        | once  | once  | ADV   | RB   | NumType=Mult                       | 4             | advmod        | NA   | NA            |
| BIO.G0.02.1.txt | 1            | 1           | Ernst Mayr once wrote, "sympatric speciation is like the Lernaean Hydra which grew two new heads whenever one of its old heads was cut off" (1963:451). | 4        | wrote | write | VERB  | VBD  | Mood=Ind\|Tense=Past\|VerbForm=Fin | 0             | root          | NA   | SpaceAfter=No |
| BIO.G0.02.1.txt | 1            | 1           | Ernst Mayr once wrote, "sympatric speciation is like the Lernaean Hydra which grew two new heads whenever one of its old heads was cut off" (1963:451). | 5        | ,     | ,     | PUNCT | ,    | NA                                 | 4             | punct         | NA   | NA            |
| BIO.G0.02.1.txt | 1            | 1           | Ernst Mayr once wrote, "sympatric speciation is like the Lernaean Hydra which grew two new heads whenever one of its old heads was cut off" (1963:451). | 6        | " | "    | PUNCT | ``    | NA                              | 4            | punct   |NA   |SpaceAfter=No  |