### Advances of IR Systems

We have something more to add to our IR system:
- fully unsupervided
  - topic modeling
  - concept analysis
  - clustering
- knowledge bases:
  - Tolerant IR
- Annotation reference answers
  - Supervised IR (Prediction)

## Topic modeling and concept analysis

### Topic modeling
| | t1 | t2 | t3 | t4 | t5 |
|---|---|---|---|---|---|
d1| 0.1| 0.2| 0.3| 0.4| 0.0|
d2| 0.0| 0.0| 0.0| 0.0| 1.0|
d3| 0.0| 0.0| 0.0| 0.0| 1.0|
d4| 0.0| 0.0| 0.0| 0.0| 1.0|

`Question`: How can we handle something like **synonyms**?

**Example**: `car` and `automobile` are synonyms.

And what about `polysemy`? 

**Example**: `bank` can be a financial institution or a river shore.

`Topic modeling` can solve this problem. But exactly, what is a topic?
Let's make an example:

|topic1|topic2|topic3|
|---|---|---|
DNA|number|life|
gene|math|human|
cell|equation|biology|
...|...|...|

*A topic is a distribution over a set of therms*

So we can imagine an histogram of word for each topic giving a probability of a word to be in a topic.

We can even translate this to `Doc 1` having a certain topic distribution.

|doc1|doc2|
|---|---|
topic1 0.4|topic1 1.0|
topic2 0.3|topic2 0.40|
topic3 0.02|topic3 0.3|

But are we actually solving the problem for the `synonyms` and `polysemy`?

We can translate from a vector space of `documents` and `terms` to a vector space of `documents` and `topics`.

| | t1 | t2 | t3 | t4 | t5 |
|---|---|---|---|---|---|
d1| 0.1| 0.2| 0.3| 0.4| 0.0|
d2| 0.0| 0.0| 0.0| 0.0| 1.0|
d3| 0.0| 0.0| 0.0| 0.0| 1.0|

to something like:

| | topic1 | topic2 | topic3 | topic4 | topic5 |
|---|---|---|---|---|---|
d1| 0.4| 0.3| 0.02| 0.0| 0.0|
d2| 1.0| 0.40| 0.3| 0.0| 0.0|
d3| 1.0| 0.40| 0.3| 0.0| 0.0|
d4| 1.0| 0.40| 0.3| 0.0| 0.0|

So when we need to `evaluate` the `sim` function $sim(q,d)$ we can use the `topic` space instead of the `term` space.

### Main topic modeling solutions

The solutions adopted to apply topic modeling are:
- Latent Semantic Indexing (LSI)
- Latent Dirichlet Allocation (LDA)
- Hierarchical Dirichlet Process (HDP)

#### Latent Semantic Indexing (LSI)
How can we find topics when we are looking at a `term-document matrix`?
The principle it's using `decomposition` of the matrix, finding a subset of terms that are relevant for a subset of documents.
It's a `SVD` (Singular Value Decomposition) of the `term-document matrix`, so basically we are finding a `low-rank approximation` of the matrix.

#### Latent Dirichlet Allocation (LDA)
Uses alpha  (doc-topic-density) and beta (topic-term-density) parameters to find the topics. 
It's a `generative model` that tries to find the `topics` and the `words` that are in the `topics`.

We don't need to understand the fully behavior, but just have the intuition of the previous 2 stuff.

### Concept analysis

Let's state what are concepts:

$$
\text{Concept} = \text{Content pattern}
$$

Content Patterns is a *subset of terms* that are **currently** relevant for a subset of documents.

Let's start with a boolean space:

| | t1 | t2 | t3 | t4 | t5 |
|---|---|---|---|---|---|
d1| 1| 1| 1| 0| 0|
d2| 1| 1| 1| 0| 0|
d3| 1| 1| 1| 0| 0|
d4| 0| 0| 0| 1| 1|

We can see that documents *1,2,3* are sharing the same terms, so we can say that they are sharing the same `concept`. We can call this the `formal concept`.

We can even talk about this in `tf-idf` space, so we can have a `concept` in a `tf-idf` space.

| | t1 | t2 | t3 | t4 | t5 |
|---|---|---|---|---|---|
d1| 0.8| 0.7 | 0.85| 0.0| 0.0|
d2| 0.8| 0.7 | 0.85| 0.0| 0.0|
d3| 0.8| 0.7 | 0.85| 0.0| 0.0|
d4| 0.0| 0.0 | 0.0| 0.9| 0.9|

They of course don't need to be the same probability, but they need to be high of course.

We can call this a `coherent concept`. A `coherent concept` is a `content pattern` that follows some increasing and
decreasing pattern of the `tf-idf` values between the documents (At least, that's what I understood).

Like, imagine having 2 documents:

| | t1 | t2 | t3 | t4 | t5 |
|---|---|---|---|---|---|
d1| 0.8| 1.2 | 0.9| 0.5| 0.8|
d2| 0.8| 1.7 | 1.2| 0.6| 0.9|

I think that's the idea of `coherent concept`.

We can have:
- constant concepts
- OP concepts

#### What can we do wit this?

- Document navigation: Starting from a document and knowing the concepts, we can suggest to the user other documents that are related to the same concept.

Moreover, we can transition from a `concept` to a matrix of `documents` and `concepts`

`Question`: How can we use queries if they work with `terms`?

`Answer`: We would need to translate the query to a `concept` space and then use the `concept` space to find the documents.

#### Tolerance

We can even set a `threshold` to say that a concept should follow a maximum amount of terms that the concept can miss.
| | t1 | t2 | t3 | t4 | t5 |
|---|---|---|---|---|---|
d1| `1`| `1`| `1`| 0| 0|
d2| `1`| `1`| `1`| 0| 0|
d3| `1`| `1`| `0`| 0| 0|
d4| 0| 0| 0| 1| 1|

And the having a maximum of 1 term that can be missed, we can say that `d1,d2,d3` are sharing the same concept.

#### Quality of concept pattern

The forula is just calculating the number of terms sharing the same concept over the total number of terms.

$$
\text{Quality} = \frac{\text{Number of terms in the concept}}{\text{Total number of terms}}
$$

Example using the above table:

$$
\text{Quality} = \frac{10}{15} = 0.53
$$

#### Advantages and disadvantages of FCA  

Advantages:
- It's pretty `simple`

Disadvantages:
- No `term relevance` is considered
  - Or I can use BM25 scores but I will have to `binarize` the scores 
  
The solution of the previous problem is to use `CCA` (Coherent Concept Analysis). It uses `biclustering` to find the `coherent concepts`.

We can have here `true positive` or `false positive` and `true negative` or `false negative` concepts.

What we do see as `true positive`?

- `True Positive`: Is a concept I am able to retrieve and it's relevant (statistically relevant)
- `False Positive`: Is a concept I am able to retrieve but it's not relevant
- `True Negative`: Is a concept I am not able to retrieve and it's not relevant
- `False Negative`: Is a concept I am not able to retrieve but it's relevant


## Knowledge

### Query Expansion

What can we consider as knowledge in our IR systems?

#### `Dictionary`

Imagine starting with a query `q` with some terms inside, like automobile.

And then we can think of expanding it `q'` adding synonyms of the terms in `q`, making the `recall` increase.

Like:

|q|q'|
|---|---|
automobile|car, vehicle, auto|
building|house, construction, edifice|

This is wat we refer as `query expansion`.

#### `Semantic Relationships`

He is talking about `WordNet`. He is speaking about generalization and specialization.

Like: Think about `is-a`.

`Car` is-a `vehicle`
`Dog` -> `Mammal` -> `Animal`

But this is not sufficient if we `don't` want to miss relevant documents.

#### `Thesaurus`

A thesaurus is a `set` of `near-synonyms` 

An example, for instance, is the term `absolutely`.
The therms that are related are:

|absolutely|absurd|whatsoever|certainly|definitely|...|
---|---|---|---|---|---|
|pathogens|toxin|bacteria|virus|...|

`How to build a thesaurus?`

I can do it `manually`: Maybe experts can help to write and find the correct synonyms that are related

I acn do it `automatically`: I can use `embeddings`, so similar word are near in the vector space. I can use `co-oocurrence` of words, so words that are often together are synonyms.
- For example: [apple, pears, bananas] are often together in the same context.
  - From here maybe you can find [pie fruit] that are related to the previous words.

### Query Completion

It's something like `auto-suggestion`. It's important for our IR system.
Let's think I want to search for `University of Genova`. 

We can have something like `univ*` and `gen*` as suggestions, and we want something that satisfies this regular expression.

`Note:` We refer at this words as `wild-cards`.

A solution to find the answer to the regex is to use a `tree` structure. So starting from
the root, we can find the words that satisfy the regex. But a problem of this approach is that it's `memory consuming`. We can have `A TONS` of words in our dictionary. 
`N.B`: We are using a post-fix tree.

For queries like `*univ` we can use a `pre-fix tree`.

He is talking about `n-grams` again and that can be applied to:
- words
- char

Let's make an example with the wild-card: `*etro*` and having n-grams = 3-grams

- etr -> beetroot -> metric -> petro -> retro -> ...
- ...
- ...
- tro -> beetroot -> metro -> strom -> retro -> ...

And now for each word we can use the `two-pointer` technique to find the words that satisfy the regex, searching for a `match` between the `n-grams`.

### Query Correction

Let's think about a query `q` and a set of documents `D`.

The query `q` is = "`gatu`". This is a misspelled word for `gato` in Portuguese.

When we are doing query correction we want to think:
- We want to correct the query?
  - Yes
  - No
- Correct which?
  - Query -> Yes, it's better to do this rather than correcting the documents
  - Documents -> Usually we don't do this. Maybe it can look like a typo, but it's better to leave the content as it is.
- Errors: Imagine the query `fell form the sky`
  - Term: They are correct! There are no errors in the terms
  - Sentence/Context: The context is `wrong`, it should be *fell `from` the sky*

How to correct `errors` in the `terms`?

A possibility is using `n-grams`. Imagine fixing `n` = 2.

For `gatu` and `gato` the bi-grams are:
- ga, ga
- at, at
- tu, to <- This is the error, so we can use `Jaccard` to calculate the similarity between the bi-grams.


Another thing is the `Leweinstein distance` that is the number of operations to transform a string into another string.

`cat` into `act`:
- c -> a
- a -> c
So the distance is 2.

Another is `Lew-Domeouw` that works like handling insertion and deletion in the same time? And then change something? Btw the final result for the same query as before is `1`. Okkk.

Another is the `Weighted Distance`. Don't know what he is talking about. Said something about the `keyboard distance` of characters or `spelling distance and phonetics`. He said another thing about `Soundex` that is a phonetic algorithm used for calculating similarity between phonetic words (erman, ermon) and translate it into something like `X75S`

Ok, now we know how to find the similarity. But there is the problem of finding the words to check the similarity.

A solution can be to use the some `n-grams` to fetch some words that share the same `n-grams` and then calculate the similarity with this compact set of words, in order to not having to calculate the similarity with all the words in the dictionary.
