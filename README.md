# MSCS 634 – Lab 3: Clustering Analysis Using K-Means and K-Medoids

**Author:** Pawan Sharma  
**Course:** MSCS 634 – Advanced Big Data and Data Mining

## Purpose

For this lab, I implemented K-Means and K-Medoids clustering techniques on the Wine dataset that is part of scikit-learn. The Wine dataset contains 178 wines, characterized by 13 attributes, and classified into one of the three classes of cultivars. In this case, I performed clustering for k=3 using both clustering techniques and evaluated them based on their Silhouette Score (the degree of cluster separation) and Adjusted Rand Index (cluster-class comparison). Finally, I visualized both outputs side-by-side on a 2D PCA plot with centroids/medoids labeled.

## Files

| File | Description |
|---|---|
| `Lab_3.ipynb` | Jupyter Notebook with data exploration, both clustering models, metrics, plots, and analysis |
| `README.md` | This summary |

## Results

| Metric | K-Means | K-Medoids |
|---|---|---|
| Silhouette Score | 0.2849 | 0.2676 |
| Adjusted Rand Index | 0.8975 | 0.7411 |
| Cluster sizes | 65 / 51 / 62 | 74 / 55 / 49 |

## Key Insights

- **K-Means showed superior performance with respect to this particular dataset.** It has an almost identical silhouette score but much better ARI score. K-Means made only 6 out of 178 wine classification mistakes.
- **K-Medoids had difficulties with class_0 / class_1 boundary separation.** It put 15 class_1 wines into the class_0 group, which accounts for all the ARI loss it incurred. Both algorithms managed to separate class_2 perfectly.
- **Silhouette scores are moderate for both of the algorithms used (~0.27-0.28).** Classes overlap each other in terms of features, class_1 in particular, so that a good clustering does not necessarily mean high silhouette score.
- **Difference between Centroids & Medoids**: The K-Means centroids lie at the exact center of each cluster; the K-Medoids medoids are the actual data points, therefore they are close to the center, but do not necessarily lie in it.
- **Choosing between the two** : K-Means works well as a general purpose algorithm for numerical data that is properly scaled and exhibits spherical clusters. It is fast and efficient. The K-Medoids method is robust to outliers, provides an actual data point as the center of each cluster, and can be used for any distance metric. However, the computation cost is high.

## Challenges and Decisions

- **Scaling:** Raw variables have very different scales (proline has hundreds-thousands scale, hue has 1 range). Proline would have an overwhelming influence on distances unless all features are standardized with z-score. That is why I decided to normalize all features using `StandardScaler` before running clustering.
- **K-Medoids library problem:** Regular package with K-Medoids (`scikit-learn-extra`) isn't supported anymore and can't be imported with new version of NumPy (2.x). Rather than downgrading NumPy, I implemented my own `KMedoids` class in the notebook. This class implements k-medoids++ seeding, iterates between reassigning points and updating medoids and selects the best from 10 restarts. This way notebook stays compatible with the current version of Python without installing any extra packages.
- **Plotting 13 dimensions:** In order to create plots, I applied PCA to the dataset which reduced it to 2 components. These 2 components explain roughly 55% of variance, so they provide useful information, but not all of it. All clustering and metrics computations were made using original 13 features.
- **Reproducibility:** I set `random_state=42` to both models, so their results will always be the same when notebook is executed.

## How to Run

```bash
pip install numpy pandas matplotlib seaborn scikit-learn jupyter
jupyter notebook Lab_3.ipynb
```