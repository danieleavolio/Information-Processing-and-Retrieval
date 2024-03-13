> Agenda
> 1. [Into the supervised information retrieval](#foundation)
> 2. [Types of feedback](#feedback)
> 3. [Rocchio](#rocchio)
> 4. Prediction
>   Features
>   Approaches
>   Evaluation
> 5. Supervised ranking

Just a little summary on the `foundation of IR`

<a name="foundation"></a>
`Advancing IR`:
- Knowledge Scores
  - Query expansion
  - Query completion
  - Query correction
- Fully Unsupervised
  - Topic modeling
  - Concept analysis
  - `Clustering`
- `Feedback`

Let's focus a bit on the `feedback` part.

`Document Annotation`: Given a document, we can annotate it with a set of terms that describe its content.
In this part we can talk about:
- Quality, Authority, Relevance
- Sentiment
- Spam/Explicit content/Fake/Hateful content
- Categories

<a name="feedback"></a>
## Types of feedback

Let's call this `modes of feedback`.

`Regular`: This can be in 2 ways:
- Task independent
- Task dependent

`Indirect`: 
- Clickstream data: For example, clicked links are relevant.
- Link structure

`Interactive`: Something like `like` and `dislike` systems.

`Pseudo-feedback`: 
- automates the assignment of relevance feedback
-  assume that the retrieved top-k documents are relevant
-  advantage: less effort
-  problem: drift towards known documents

### The types

We can have different type of feedback.

- `Numeric`
  - Scored
  - Numbers
- `Non Numeric`
  - Unary: Relevant or not
  - Binary: Relevance vs Non-relevance
  - Nominal: Categories
  - Ordinal: Order of preference of the documents

### How to work with these

When we are handling `Unary` and `Binary`, we use something called 
`Rocchio`.
For everything else, we use `Prediction`. In particular, when we are talking about : `Nominal` and `Ordinal`, we are talking about `Classification`,  since they are categorical. When we are talking about `Scored` and `Numbers`, we are talking about `Regression`.

`N.B`: It's possible to have `Ordinal Regression`


<a name="rocchio"></a>
## Rocchio

Imagine having a multidimensional space of documents with `+` and `-` regarding the documents.

Moreover, there are documents without annotation.

![Rocchio](https://i.imgur.com/cxxjskH.png)

Rocchio works like this:

`Task`: Identify a document in a space

1. Find the centroid of the relevant documents
2. Find the centroid of the non-relevant documents
3. Query `q` = `Mr` + (Mr - Mnr) = 2`Mr` - `Mnr`

Usually the `q` is computed using a `Beta` and `Lambda` that is used to give more or less importance to the relevant and non-relevant documents.

Moreover, there is another kind of formula using `alpha` giving more importance to the original query.

$$
q' = \alpha q  + \beta Mr - \lambda Mnr
$$

This is called `Smart Rocchio`.

`The goal`: Go away from the non relevant documents and go towards the relevant documents.

Plot from the slides.

![Rocchio](https://i.imgur.com/aVfUFiu.png)

#### Exercise on Rocchio

![Rocchio](https://i.imgur.com/vxGUd0o.png)

We are using the `smart rocchio` formula.

$$
q' = \alpha q  + \beta Mr - \lambda Mnr
$$

Let's state that:

$$
\alpha = \beta = \lambda = 1
$$

The query is `q ={sun, fire}`.

The Term Frequency (TF) space is:

|     | sun | rain | fire | weather | feedback |
| --- | --- | ---- | ---- | ------- | -------- |
| d1  | 3   | 0    | 0    | 1       | o        |
| d2  | 1   | 4    | 0    | 2       | -        |
| d3  | 1   | 1    | 2    | 4       | 0        |
| d4  | 0   | 5    | 0    | 3       | -        |
| d5  | 3   | 0    | 3    | 2       | +        |


Now let's do calculations.

$$
q' = \alpha \begin{bmatrix} 1 \\ 0 \\ 1 \\ 0 \end{bmatrix} + 1 \times \begin{bmatrix} 3 \\ 0 \\ 3 \\ 2 \end{bmatrix} - 1 \times \begin{bmatrix} 0.5 \\ 4.5 \\ 0 \\ 2.5 \end{bmatrix} \\
= \begin{bmatrix} 2.5 \\ -4.5 \\ 4 \\ -0.5 \end{bmatrix}
$$  

Rocchio have some problems:
- Only good with unary and binary feedback
- It's not good with non-linear separable data
- It's not good with irrelevant documents that are close to the relevant ones
- Multi-class problems are not handled well

<a name="prediction"></a>
## Prediction

Imagine having a table containing documents and categories and variables.

|     | y1  | y2  | ... | yk  | z1, ..., zn   |
| --- | --- | --- | --- | --- | ------------- |
| d1  | 1   | 0   | ... | 0   | 1, 0, 0, 1, 0 |
| d2  | 0   | 1   | ... | 0   | 0, 1, 0, 0, 1 |
| ... | ... | ... | ... | ... | ...           |
| dn  | 0   | 0   | ... | 1   | 1, 0, 0, 0, 1 |

`The goal`: Obtaining a `model`
$$
M: X \rightarrow \sum \ \hat{z} = M(d) \ Classification \\
M: X \rightarrow \mathbb{R}^k \ \hat{z} = M(d) \ Regression 
$$

We can have:
- Valence [-1, 1]
- Arousal [-1, 1]

`Valence` refers to the emotional value associated with a stimulus. It describes the positivity or negativity of an emotion. In your scale, -1 represents a highly negative emotion (e.g., sadness, anger) and 1 represents a highly positive emotion (e.g., happiness, joy).

`Arousal` refers to the intensity of an emotion. It describes the level of alertness and mental engagement. In your scale, -1 represents a state of low arousal (e.g., calm, relaxed) and 1 represents a state of high arousal (e.g., excited, anxious).

The variables `y` can be:
- term relevance
- embeddings
- features

### Features extraction

We can have many features. In our context, we can build something like:
- Document length
- Topics
- Concepts
- Sentiment

## Approaches

We can have different approaches to the problem. Let's start with the base one: `Connectionist`, so `Neural Networks`.

### Connectionist
`Neural Networks`: We know that the set of feature is given as input to the model. The model is a set of layers of neurons. The output is the prediction based on the probability of the document to be in a category.

$$
\hat{z} = M(d) = {A,B}
$$

### Linear
- `Perceptron`: Logistic regression
- `Linear regression`
- `Support Vector Machines`

### Bayesian

- `Naive Bayes`
- `Bayesian Networks`

### Instance Based

- `K-Nearest Neighbors`

### Tree Based

- `Decision Trees`
- `Random Forest`  

### Rule Based

- `RIPPER`
- `C4.5`

### Ensemble

- `Boosting`
- `Bagging`

`N.B:` Association is not used in IR usually. With association we mean the `Tree` and `Ensemble` methods.

One last missing is `Analogizers`.

### Analogizers

We are gonna tick the most important ones to classify documents.

Imagine having a space of documents with `+` and `-`. If we have a new document with `o`, what can we do?

We can use the `cosine` or `euclidean`, in general distance measure to find the closest document. Having the the `neighbours`,
we can use the `majority` to classify the new document.

`N.B`: This can be even used for `Regression`. Like, use the mean or median to get a score.

![](https://i.imgur.com/l2E39c1.png)

We have an example here having a point with 5 neighbours. `4` are `-` and `1` is `+`. The majority is `-`.

<a name="evaluation"></a>
## Evaluation

How do we evaluate predictors? Let's start with `classifier`

### Classifier evaluation

We can use the `confusion matrix` to evaluate the classifier.

|     | A   | B   | C   |
| --- | --- | --- | --- |
| A   | TP  | FP  | FP  |
| B   | FN  | TP  | FP  |
| C   | FN  | FN  | TP  |

Where:
- `TP`: True Positive
- `FP`: False Positive
- `FN`: False Negative
- `TN`: True Negative

`Question`: When we do `Cross Validation`? 

`Answer`: When we have a small dataset so we want to use all the data to train and test the model. It's a shuffle to change the training and testing set and take the average of the results to see the performance of the model.

- `Recall`: Number of relevant documents retrieved over the total number of relevant documents.
- `Precision`: Number of relevant documents retrieved over the total number of documents retrieved.
- `Accuracy`: Number of relevant documents retrieved over the total number of documents.
  
| z   | z'  |
| --- | --- |
| 2   | 1   |
| 3   | 2   |
| 4   | 3   |
| 5   | 4   |

`MAE`: Mean Absolute Error

| z   | z'  | z-z' |
| --- | --- | ---- |
| 2   | 1   | 1    |
| 3   | 2   | 1    |
| 4   | 3   | 1    |
| 5   | 4   | 1    |

With `z` being the true value and `z'` the predicted value.

`MAE` is the average of the absolute differences between the true values and the predicted values.

$$
MAE = \frac{1}{n} \sum_{i=1}^{n} |z_i - z'_i|
$$

$$
MAE = \frac{1}{4} \sum_{i=1}^{4} |z_i - z'_i| = \frac{1}{4} \times 4 = 1
$$

`RMSE`: Root Mean Squared Error. It's the square root of the average of the squared differences between the true values and the predicted values.

$$
RMSE = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (z_i - z'_i)^2}
$$

| z   | z'  | z-z' | (z-z')^2 |
| --- | --- | ---- | -------- |
| 2   | 1   | 1    | 1        |
| 3   | 2   | 1    | 1        |
| 4   | 3   | 1    | 1        |
| 5   | 4   | 1    | 1        |

$$
RMSE = \sqrt{\frac{1}{4} \sum_{i=1}^{4} (z_i - z'_i)^2} = \sqrt{\frac{1}{4} \times 4} = 1
$$

`RMSE vs MAE`: RMSE is more sensitive to outliers. It's the most used in practice. MAE is more robust to outliers.

#### Exercise on Evaluation
- -: -1
- +: +3
- o: 0

|     | sun | rain | tie | weather | climate | relevance |
| --- | --- | ---- | --- | ------- | ------- | --------- |
| d1  | 3   | 0    | 0   | 1       | 1       | -         |
| d2  | 1   | 4    | 0   | 2       | 3       | -         |
| d3  | 1   | 1    | 2   | 4       | 1       | +         |
| d4  | 0   | 5    | 0   | 3       | 2       | 0         |
| d5  | 3   | 0    | 3   | 2       | 2       | +         |


Now imagine having 2 documents:

- d6: sun, time
- d7: sun, climate

We use `KNN` <=3

We can rewrite the table like this:

|     | sun | rain | tie | weather | climate | relevance |
| --- | --- | ---- | --- | ------- | ------- | --------- |
| d1  | 3   | 0    | 0   | 1       | 1       | -         |
| d2  | 1   | 4    | 0   | 2       | 3       | -         |
| d3  | 1   | 1    | 2   | 4       | 1       | +         |
| d4  | 0   | 5    | 0   | 3       | 2       | 0         |
| d5  | 3   | 0    | 3   | 2       | 2       | +         |
| d6  | 1   |      |     | 1       |         |           |
| d7  | 1   |      |     |         | 1       |           |


We can use the measure of `cosine` or `manhattan` to find the closest documents. But we can even use other measures.

In this case, the used is the difference.

| SIM    | d1  | d2  | d3  | d4  | d5  |
| --- | --- | --- | --- | --- | --- |
| d6  | 3   | 1   | 3   | 0   | 6   |
| d7  | 4   | 4   | 2   | 2   | 5   |

We are using the `sim` similarity as the function for the `KNN`.

`3NN(d6)`= {d1,d3, d5}
    
`3NN(d7)`= {d1,d2, d5}

If we apply the `mode` for the values:

`z'(d6)` = `mode`{d1,d3, d5} = {-,+,+} = `+`
`z'(d7)` = `mode`{d1,d2, d5} = {-,-,+} = `-`

If we use the `mean`:
`z'(d6)` = `mean`{d1,d3, d5} = {-1,3,3} = 5/3 = 1.67
`z'(d7)` = `mean`{d1,d2, d5} = {-1,-1,3} = 1/3 = 0.33

Now we can assess the measures.
To do so, we need some `ground truth`.

In the statement, both `d6` and `d7` are `o`.

> Actually, the accuracy is `0` because we are classifying `d6` and `d7` as `+` and `-` but they are `o`.

Let's do `RMSE`:

$$
RMSE = \sqrt{\frac{1}{2} \sum_{i=1}^{2} (z_i - z'_i)^2} \\
RMSE = \sqrt{\frac{1}{2}(0-1.67)^2 + (0-0.33)^2} \\
RMSE = 1.2
$$

<a name="supervised-ranking"></a>
## Supervised ranking

Now we are talking about something that is `task dependent`.

We have a `query`.
Imagine having a set of documents and features. We have even a label that is `Relevant` and `Non Relevant`.

The features can be calculated using doucment and query
- TF-IDF(d,q)
- BM25(d,q)
- BERT embeddings(d,q)

The goal is to rank the documents based on the relevance.
The score can be done using `relevant` and `non relevant` documents. Like, in `KNN` we can divide the number of relevant documents over the total number of documents to get a `numerical score`.

### For the summarization task

1. Compute some features to assess how relevant is a sentence
2. Learn a predictor that works as a summarizer that given a document and the sentence checks based on the features if the sentence is relevant or not.

