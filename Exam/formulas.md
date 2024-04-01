

Jaccard

$$
J(A,B) = \frac{|A \cap B|}{|A \cup B|}
$$

Jaccard normalized:

$$
J(A,B) = \frac{|A \cap B|}{\sqrt{|A| \cup |B|}}
$$

TF: Sum of the occurrencies of the term in the document

DF: Number of documents that contains the term

IDF:

$$
log_{10} \frac{N}{DF}
$$

TF-IDF:

$$
score(q,d) = \sum_{t \in q} IDF_t \cdot (1+log_{10} TF_{t,d})
$$

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