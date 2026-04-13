# clustering-benchmark
The goal of this experiment was to evaluate the overall quality of
selected clustering algorithms on a wide range of benchmark
datasets. The testing focused on:
• 43 2-dimensional datasets, with sets of reference labels
(label0)
• 6 clustering algorithms (K-means, Gaussian mixture model,
Genie (with different g parameter values), Agglomerative
Hierarchical Clustering (with single, average, complete and
Ward linkage), DBSCAN, spectral clustering).

### DBSCAN

DBSCAN algorithm had to
be considered separately, since opposed to other clustering
algorithms it estimates the number of clusters on its own. For
other algorithms, the number of clusters is passed as input, and
the number of requested clusters is equal to number of clusters
found in the reference labels.
Additional for DBSCAN parameters ϵ and MinPts have to be
estimated.

Parameter estimation:
1. MinPts estimation
   - heuristic 1 - MinPts >= d + 1 => MinPts >= 3
   - heuristic 2 - MinPts ∈ {2d, 4d} => MinPts ∈ {4, 8}
Therefore for each dataset I tested MinPts = {3, 4, 5, 6, 7, 8}.
3. ϵ estimation - The optimal ϵ was estimated for each of the
MinPts values using the KneeLocator method.
After choosing the optimal ϵ for each MinPts value and evaluating
each model using ARI, the model maximizing ARI was chosen and
its partitions were returned.

Additionally:
• all points marked as noise by the algorithm (with label = −1)
were removed,
• all label values were increased by 1 to be consistent with the
reference labels.

### Results

All algorithms were tested on all 43 datasets and their performance has been
saved into a dataframe. The dataframe consists of:
- dataset and battery name,
- algorithm name (and the g parameter value for Genie),
- reference metrics values (ARI, NMI, NCA),
- internal metrics values (Silhouette, Calinski Harabasz etc.),
- median of reference metrics,
- median of internal metrics.
Additionally for DBSCAN experiments I included:
- the number of clusters in the reference labels and the number of clusters
produced by DBSCAN,
- MinPts and ϵ parameters values,
- coverage (the percent of points which have not been classified as noise).
All further analyses have been performed on these data frames.

### Summary of outcomes:

1. The Genie algorithm achieves the best overall performance,
combining high clustering accuracy with low variance across
datasets, indicating strong robustness.
2. DBSCAN is hard to tune and fails often at cluster count
estimation, however when it estimates the correct number of
clusters its performance is extremely strong.
3. Internal validation metrics may be misleading when assessing
clustering quality, as they do not always reflect agreement
with ”ground-truth” labels


