### Question 1
```
1. sentence clustering(d,I,args)
@input document d, optional inverted index I, optional clustering args
@behavior identifies the best number of sentence clusters for the target tasks according
to proper internal indices, returning the corresponding clustering solution
@output clustering solution C
```

Question: With clustering solution, we mean a number $n$ of clusters?
If so, do we need to specify the way we choose the $n$ clusters?

### Question 2
```
summarization(d,C,I,args)
@input document d, sentence clusters C, optional inverted index I and guiding args
@behavior ranks non-redundant sentences from the clustering solution, taking into
attention that ranking criteria can be derived from the clusters’ properties
@output summary (without pre-fixed size limits)
```

Question: In which sense ranking criteria can be derived from the clusters' properties?

### Question 3
```
1. feature extraction(s,d,args)
@input sentence s and the enclosed document d
@behavior extracts features of potential interest considering the characteristics of the
sentence and of the wrapping document
@output sentence-specific feature vector
2. training(Dtrain,Rtrain,args)
@input training document collection Dtrain, reference extracts Rtrain, and optional
arguments on the classification process
@behavior learns a classifier to predict the presence of a sentence in the summary
@output classification mode
```

Question: An example of row of a training set?

### Question 4
```
Are the learned models able to generalize from one category to another? Justify.
```
Question: What do we mean by "generalize from one category to another"?

### Question 5
```
In alternative to the given reference extracts, consider the presence of manual abstractive
summaries, can supervised IR be used to explore such feedback? Justify.
```
Question: Didn't fully understand the question. Can you provide an example?



# Stuff for the project


Notes:
1. Silhouette: internal measure of cohesion and separation
2. Adjusted ran score: if the cluster is separated well


1. See a document as set of sentences
2. See how sentences are in the vector space
3. Choose the sentence in the cluster without redundancy selecting only the most important sentences and not from the same cluster
4. How `many` clusters?
   1. Solution 1: If we want 1 sentence per cluster, we need to choose the number of clusters as the maximum number of sentences we want in the summary
   2. Solution 2: Elbow method using silhouette score
5. Which algorithm to use?
   1. Agglomerative clustering
   2. Hierarchical clustering
   Try to compare two in order to see the differences

6. Create a dataset containing 
   1. sentence
   2. metrics michele has in the quadernone
   3. category
   4. label: 1 if the sentence is in the summary, 0 otherwise
   5. Use SHAP for the feature importance
7. How to do classification?
   1. Training set with label 1 and 0 for being in the summary or not
   2. Use some similarity measure
   3. We can train in different ways:
      1. Train in 1 category and test in that category
      2. Train in 1 category and test in another category
      3. Train in all categories and test in all categories
   