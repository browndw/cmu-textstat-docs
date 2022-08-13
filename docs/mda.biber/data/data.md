# mda.biber data

## `micusp_biber`

### Description

[The Michigan Corpus of Upper-Level Student Papers (MUCUSP)](https://elicorpora.info/main) tagged with pseudobibeR. Can be used for comparisons with [Hardy and Römer's (2013) study](https://www.euppublishing.com/doi/abs/10.3366/cor.2013.0040).

The data frame contains 68 variables (`doc_id` and [the 67 tags produced by psuedobibeR](https://cmu-textstat-docs.readthedocs.io/en/latest/pseudobibeR/pseudobibeR.html#categories)) and 828 observations.

```{note}

Counts are normalized **per 1000 tokens**.

Type-to-token is calculated using a [moving-average type-to-token ration (MATTR)](https://www.tandfonline.com/doi/abs/10.1080/09296171003643098).

```


### Data snippet

| doc\_id      | f\_01\_past\_tense    | f\_02\_perfect\_aspect | f\_03\_present\_tense | f\_04\_place\_adverbials | f\_05\_time\_adverbials |
|-------------|--------------------|---------------------|--------------------|-----------------------|----------------------|
| BIO.G0.01.1 | 17.678708685626443 | 8.070714834742505   | 52.45964642582629  | 3.074558032282859     | 4.0353574173712525   |
| BIO.G0.02.1 | 11.477761836441896 | 10.043041606886655  | 61.33428981348637  | 2.152080344332855     | 6.097560975609756    |
| BIO.G0.02.2 | 3.875968992248062  | 0                   | 62.01550387596899  | 0                     | 1.2919896640826871   |
| BIO.G0.02.3 | 1.7006802721088434 | 3.401360544217687   | 64.62585034013605  | 0                     | 0                    |
| BIO.G0.02.4 | 1.531393568147014  | 4.594180704441042   | 70.44410413476264  | 1.531393568147014     | 0                    |
| BIO.G0.02.5 | 14.844804318488528 | 7.75978407557355    | 47.57085020242915  | 3.373819163292848     | 2.0242914979757085   |

[...]

| f\_62\_split\_infinitve | f\_63\_split\_auxiliary | f\_64\_phrasal\_coordination | f\_65\_clausal\_coordination | f\_66\_neg\_synthetic | f\_67\_neg\_analytic  |
|----------------------|----------------------|---------------------------|---------------------------|--------------------|--------------------|
| 0.1921598770176787   | 4.611837048424289    | 8.262874711760183         | 5.95695618754804          | 0.7686395080707148 | 4.227517294388932  |
| 0.7173601147776184   | 5.0215208034433285   | 6.097560975609756         | 3.945480631276901         | 1.4347202295552368 | 2.8694404591104736 |
| 0                    | 3.875968992248062    | 6.459948320413436         | 1.2919896640826871        | 0                  | 3.875968992248062  |
| 0                    | 1.7006802721088434   | 18.70748299319728         | 0                         | 0                  | 0                  |
| 3.062787136294028    | 6.1255742725880555   | 7.656967840735069         | 0                         | 0                  | 7.656967840735069  |
| 0.6747638326585694   | 4.723346828609987    | 3.0364372469635628        | 1.686909581646424         | 0.6747638326585694 | 5.398110661268555  |