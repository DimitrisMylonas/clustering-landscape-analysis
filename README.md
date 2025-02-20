# Clustering Comparison Project: KMeans vs Agglomerative vs DBSCAN

## Overview
This project explores and compares the effectiveness of three clustering algorithms — **KMeans**, **Agglomerative Clustering**, and **DBSCAN** — on a custom synthetic dataset with sinusoidal structure and approximately 40,000 data points. The goal is to evaluate each algorithm based on clustering performance, computational efficiency, and ability to preserve the natural shape of the data.

All three algorithms were applied to the **same scaled dataset**, and performance was measured using:
- **Silhouette Score**
- **Davies-Bouldin Index**
- **Execution Time**

Additionally, the clusters were visualized using both **scatter plots** and **graph-based representations**, including PCA-based layouts and nearest-neighbor connectivity graphs.

---

## Dataset Description
The dataset is derived from the well-known `birch2` synthetic dataset. It contains 2D data points arranged along a wave-like curve.

Due to computational constraints in Google Colab, the dataset was downsampled to **1/3 of its original size** (~40,000 points), while specific visualizations (e.g., dendrogram, graphs) used further samples of **4,000** or **17,000** points where appropriate.

---

## Clustering Algorithms & Tuning

### 1. KMeans Clustering
- Grid search was performed on `k` values from **2 to 10**.
- Evaluation metrics (Elbow and Silhouette methods) indicated an optimal `k=4`.

**Results:**
- **Silhouette Score**: `0.6908`
- **Davies-Bouldin Index**: `0.4000`
- **Execution Time**: `0.03 sec`

**Observations:**
- KMeans produced clean, symmetric clusters that follow the global structure of the wave.
- The graph-based visualization revealed tightly grouped communities, though lacking curved granularity.

---

### 2. Agglomerative Clustering
- Hierarchical clustering was applied with a dendrogram built on a **sample of 17,000 points**.
- Based on the dendrogram, `k=4` was chosen to maintain consistency in evaluation.

**Results:**
- **Silhouette Score**: `0.6557`
- **Davies-Bouldin Index**: `0.4361`
- **Execution Time**: `57.98 sec`

**Observations:**
- Agglomerative clustering yielded groups that were less symmetric than KMeans but captured more local features.
- Graph-based visualization showed mid-level cohesion and finer granularity in cluster transitions.

---

### 3. DBSCAN (Density-Based)

- Initial runs with `eps=0.08`, `min_samples=5` gave poor results (3 clusters, low silhouette).
- A grid search was conducted over:
  - **`eps`: 0.05 to 0.10**
  - **`min_samples`: 5 to 30**

**Best Configuration:**
- `eps = 0.05`
- `min_samples = 20`

**Final Results:**
- **Silhouette Score**: `0.5419`
- **Davies-Bouldin Index**: `0.9900`
- **Execution Time**: `~2 sec`
- **Detected Clusters**: `11`
- **Noise Points**: `6`

**Observations:**
- DBSCAN performed better after tuning, detecting finer wave structures without predefining `k`.
- However, it remained slightly inferior to KMeans and Agglomerative in metric-based evaluation.

#### Graph Interpretation:
> The graph-based visualization of DBSCAN (sample of 4000) revealed a distinct **peripheral circular structure**, not present in the other methods. This is due to the wave-like nature of the data combined with PCA projection, where spatially distant parts wrap into a circular layout. The DBSCAN algorithm, favoring local density and connectivity, naturally traces this curvature, producing a graph layout that reflects the true topology of the original data more faithfully than centroid-based methods.

---

## Comparative Summary

| Algorithm       | Silhouette | DB Index | Clusters | Noise | Exec Time |
|----------------|------------|----------|----------|--------|------------|
| **KMeans (k=4)**        | **0.6908** | **0.4000** | 4        | 0      | 0.03 sec   |
| **Agglomerative (k=4)** | 0.6557     | 0.4361     | 4        | 0      | 57.98 sec  |
| **DBSCAN (tuned)**      | 0.5419     | 0.9900     | 11       | 6      | ~2 sec     |

---

## Conclusions
- **KMeans** was the most effective in terms of quantitative scores and clean visual separation.
- **Agglomerative** captured subtle transitions between parts of the curve and produced more structural granularity.
- **DBSCAN** revealed an interesting circular topology via its graph, reflecting the true geometric continuity of the data — despite scoring lower in traditional metrics.

This project demonstrates how different clustering algorithms excel in different scenarios, and why it's important to use **both metric-based and structural evaluation**.

---


