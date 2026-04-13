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

### Algorithm performance and stability

<img width="910" height="624" alt="mean_ari_vs_std_var_per_model" src="https://github.com/user-attachments/assets/c0dc735c-6b0c-4354-988a-1aaf1461bb62" />

This visualisation represents the accuracy and stability of the clustering algorithms. The genie model, with different g parameter values, achieves high mean ARI, with low standard deviation as the same time.
The genie algorithm for g = 0.3 achieves not only the best average ARI value, but also has very stable performance across across all datasets.

### Mean dataset difficulty

<img width="1001" height="633" alt="mean_ari_per_dataset" src="https://github.com/user-attachments/assets/cbf41c84-43a3-4190-8ea8-a2b91e045eaf" />

Mean ARI achieved by all algorithms dor each of the datasets, with respect to their battery.The uci battery was the most difficult on average.

### DBSCAN - performance and number of clusters estimation

<img width="563" height="455" alt="dbscan_k_comparison" src="https://github.com/user-attachments/assets/0f2943e5-e6dd-4501-86e9-0757efd0ac21" />

Finding the right number of clusters is a significant DBSCAN issue. The algorithms under- or overestimated the number of clusters for 56% of the datasets.


<img width="989" height="396" alt="dbscan_k_case_boxplots" src="https://github.com/user-attachments/assets/22778a68-5124-48cf-9d6d-986c2eb9fd07" />

Internal validation measures, especially Silhouette, which prefers spherical clusters, while DBSCAN can produce irregularly shaped ones, may indicate good clustering quality even when the clustering structure is incorrect.

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


