# Cluster analysis

- Two types of unsupervised cluster analysis: **partitioning** and **hierarchical**.

  - **Partitioning** algorithms separate objects(features) into a finite set of disjoint subsets, each of which has a center based on the average feature values within the cluster.

  - **Hierarchical** algorithms order objects(features) into a hierarchically nested sequence, or tree structure, for which the result is a dendogram that contains a root, branches, and leaves.

- Resemblance coefficients or distance metrics, which form a matrix of all pairwise comparisons of similarity or dissimilarity between objects.

- Correlation is a similarity coefficient.

- Computationally efficient variant of correlation is *Pitman correlation*

- Other names for *Euclidean* and *Manhattan distance* are the $L_1$ and $L_2$ distance, respectively (see [here](../Miscellaneous/Miscellaneous%20Notes.md))
  - Dot production: $x_l'x_m$
  - Norm of a vector: $||\mathbf{x}|| = \sqrt{x_1^2 + \cdots + x_p^2}$

- **Cluster validity**:
    - Ideal cluster structure will reveal clusters whose centers are far apart and whose assigned objects are all close in proximity.
    - The more disjoint the clusters are and the less overlap, the greater the chance that the specified number of clusters is the optimal choice.
    - **Silhouette index** measures the degree of membership of objects within their assigned clusters.
        - if all of the objects in a cluster have the same feature values, then the average intra-cluster distance $a(i) = 0$, resulting in a numerator of $b(i) - 0 = b(i)$, denominator of $\max{0,b(i)} = b(i)$ and ratio $s(i)$ of unity.
        - if $a(i) > b(i)$, the numerator $b(i) − a(i)$ and $s(i)$ become negative, indicating that $x_i$ is misclassified.
        - if there is no real cluster structure present, both $a(i)$ and $b(i)$ will be similar causing s(i) to approach zero.
        - singly, $s(i)$ by itself only reflects the cluster support for $x_i$ , so the average silhouette index is used to measure the overall clustering validity
        - Strong evidence of cluster structure occurs if $0.7<  s\le 1$, reasonable evidence when $0.5<s\le 0.7$, weak evidence for $0.25 < s \le 0.5$, and no support for structure if $s\le 0.25$.

## Gaussian mixture models
- Model-based clustering makes the assumption that a finite mixture of probability distributions is responsible for generating the data under consideration.
- Model-based cluster analysis is an extension of K-means cluster analysis in which maximum likelihood estimation is used.
- It is the evaluation and comparison of how a selected mixture of probability density functions can best describe the cluster structure.



## [Introduction to K-Means Clustering](https://www.pinecone.io/learn/k-means-clustering/)