# Information Processing and Retrieval

Questions and answers to the Information Processing and Retrieval course. This questions are based on the notes taken during the theoretical lectures and may not be 100% accurate.

**What is IR**: Given a library it extract information from it.

**What are libraries**: collections of docs

**Example of structured, semi structured and unstructured data**:
- structured: data structrues known, tables, graph, db
- semi-structures: web pages, articles, json
- unstructured: free text

**How does the brain find the documents?**: It uses an indexed library.

**What are precision and recall**:
- precision is the fraction of documents that are relevant and returned by the query
- recall is the fraction of documents that are relevant inside the collection of docs

**How can you specify queries**:
- natural language processing
- logical operators
important is the positional operator \n, where n is the distance between the words

**What are query supports?**: With query supports we refer to systems like query suggestion and query completion

**When we have a product in production, how to we evaluate it?**: 
- prototype:
  - ask users using it
  - use experts feedback
-  mature:
   -  get feedback from the users
-  production:
   -  A/B testing
   -  bucket testing: divide the users in two groups, one with the new feature and one without it

**Explain differene between token word and term**:
- token a word in the sentence (all the words)
- word is the number of different words in the sentence (set of words)
- term is the word normalized (lowercase, stemming, stop words)
**difference betweeen static and dynamic**:
- static: no docs are added
- dynamic: docs can be added

**Why use an inverted index**: To solve the memory usage problem and have better performance in query answering.
From docs -> terms
to terms -> docs

**How to optimzie the bytes for the document identifier**: Calculate the 

$$bits \lceil\log_2(\text{number of docs})\rceil $$

so if you have 20 bits, you need 3 bytes, because you actually need the minimum number of bytes to represent the number of bits. 

**Gap based encoding**: Gap based encoding is based on calculating the difference between the document identifiers.

If you have docs: 
$$6 \ 12 \ 20 \ 22$$

The gaps are: 
$$6\  6\  8 \ 2$$

**in dynamic collection what the hardest part?**: The hardest part is the update, because inserting and deleting is easy, but updating is hard because you need to change the pointers.

**What are the problems of phrase querying?**: Searching for specific combination of words can be hard, since you can find both words but not in the context you need. You can use **bi-grams** for 2 words queryes, but growing the number of words leads you to needing n-grams. To solve this, you can use positional queries that refers to the distance between the words, and to do this you need to use the positional index that stores the distance of the words in the documents.

**What are the problems of boolean space?**:
- Ranking, you cannot say a documents is more important than another. 
- feast of famine: you can have a lot of empty answers from your queries.

**What is Jaccard coefficient**:
Is the ratio between, given a query and a doc, the number of shared terms between q and d over the total number of terms in q and d.

**What the problem with Jaccard coefficient**: It doesn't take count of terms occurrencies.

**What is TF**: TF is the term frequency, it is the number of times a term appears in a document.

**Eucliden distane**: It is the distance between two points in a space.

$$
\sqrt{\sum_{i=1}^{n} (x_i - y_i)^2}
$$

**Cosine similarity**: 
$$
\frac{A \cdot B}{||A|| \cdot ||B||}
$$

where we are using the euclidean norm. It's just applying the eucliden distance to the points.

Something is happening at the blackboard, I don’t get it.

**The dddqqq stuff**

| TF | IDF | NORM |
| --- | --- | --- |
| n:TF | n:1 | n:1 |
| l:1+log TF | t: log N/DF | c: 1/||d|| |

$$
ddd.qqq
$$

You can choose to apply the normalization to the query or to the document and to use tf and idf and stuff.

**BM25 Formulas**:

$$
\begin{aligned}

BM25&= IDF \frac{(k+1) \frac{TF}{B}}{k + \frac{TF}{B}} &= \\
=& IDF \frac{(k+1)TF}{Bk+TF} &= \\
=& IDF \frac{(k+1)TF}{TF+k((1-b) + b \frac{|d|}{|\bar{d}|})}
\end{aligned}
$$

Using document and query:

$$
BM25(q,d) = \sum_{t \in q}IDF_t \frac{(k+1)TF_{t,d}}{TF_{tf,d}+k((1-b) + b \frac{|d|}{|\bar{d}|})}
$$

Note that $|\bar{d}|$ is the average document length.

**What are the problem of NN**:
- explainability
- computational cost

**How to solve the problem of LLM and positions of the text conversion to vectors**: You can use the positional embedding that keep track to the relative position.

**How to train a LLM**:
- Masking: you mask some words and you try to predict them

**How to train BERT**:
- predicting masked words
- next sequence prediction

**What are the 3 criterias for a good IR system**:
- usability: how much easy to use 
- efficiency: how much time it takes to answer a query
- efficacy: how much the system is able to answer the query

**What is ground truth**:
- The ground truth is the set of documents that are relevant to the query.

**What is the formula for recall**

Recall is the number of relevant documents retrieved over the total number of relevant documents.

Verticale

**What is the formula for precision**

Precision is the number of relevant documents retrieved over the total number of documents retrieved.

Orizzontale

**Why is accuracy and specificity not so relevant?:**

Formula: TP + TN / TP + TN + FP + FN

Is not a good metric because true negatives are usually very high.

**What does it means to have a high recall and low recall:**

- High recall: a lot of info but some of them are not relevant
- Low recall: you are missing info but the one you have is relevant

Evaluation to check doing exercises.

**What's topic modeling**:Starting from a set of terms, we find the topic regarded to the terms. We pass from a matrix of documnets-terms to a matrix of documents-topics.

**What is a concept pattern**: A concept pattern is a subset of terms that are relevant to a topic of a document.Imagine more docs of a collection sharing the same terms. Probably they are related to the same topics.

**What is coherent concept**: Given a set of docs, a pattern of increasing of decresing the values of the tfidf of the docs. So they follow the same andamento.

**What is tolerance**: The threshold telling us how many terms can be missing inside a concept pattern to conside the doc valid for the topic.

**How to expand a query**: Add synonyms to the query of the terms.

**What is a thesaurus**: It's a set of synonyms of the terms, really near at the meaning. You can do it manually or with some embeddings distances from the terms.

**How to perform query completion**: Starting from a query containing a regex like univ*, that we refer to this as *wildcard*, use a tree traversla technique to find the terms inside the dictionary that satisfty the regex.

**Query correction. How to do**: Given a query, you cn use bi-grams to check the terms and correct them.

**In dendograms, when we use minimum and when we use maximum linkage:**:
- minimum: for irregular shape clusters (non convex)
- maximum: for convex clusters, more balanced

**What are the pros and cons of density based cluster?**:
- very fast
- handles outliers
Problems:
- you have to find the parameters and it's not easy (k and epsilon for number of neihbhors and distnace)

**When can you use rand index and purity?**: WHen you have some ground trouth.

**When do we use Rocchio?**: Having a query, we want to move the query in the direction of the relevant documents. It's done calculating the distance between the centroids of relevant and non relevant documents and using this as a starting point.

**How to use analogizers**: Having a new doc inside a document already classified, you can check the distance and the k neighbors and assign the new doc to the class that has the most neighbors.

**Precision recall accuracy for classifiers**:
- precision: number of relevant documents retrieved over the total number of documents retrieved
- recall: number of relevant documents retrieved over the total number of relevant documents
- accuracy: number of correct predictions over the total number of predictions

**What is well behaved search**: A search engine that returns relevant results fo ra query

**What is page rank**: Page rank is an algorithm to rank the pages in the web. It's based on the number of links that point to a page.

**What are meta crawlers:** A search engine that uses other search engines for the results

**What's the URL Frontier**: The set of URLs that are not yet visited by the crawler and that need to be visited

**What are the most important thing a crawler must do**:
- be polite and crawl only the pages that are allowed
- be robust: handle traps
- be efficient: don't waste time on pages that are not relevant
- be fresh: update the index
- quality: avoid spam
- coverage: cover all the pages

**What is the bottleneck of a crawler**: The bottleneck is the DNS resolution, because it takes time to resolve the domain name to the IP address.


**What are front and back queue**: Front queue is the queue of the URLs with a priority based on an integer. Back queue takes the url from the front queue and CONTAINS ONLY URLs from a single HOST. It's for politeness.