# UnsupervisedLearningMall
Unsupervised Learning Project to evaluate the data of mall customers, their spending habits to cluster the data for targeted marketing and advertising.

Cluster various groups of customers using unsupervised clustering methods to identify groups.

### What we clustered and how
- Features: Age, AnnualIncomeK, SpendingScore, plus Gender (one‑hot encoded). Numeric features were imputed and standardized; categorical was imputed and one‑hot encoded.
- We explored structure with PCA/t‑SNE and selected k via elbow (inertia plateau) and silhouette peak.

### Choosing the number of clusters
- Elbow suggested diminishing returns after k≈5–7.
- Silhouette peaked at k=6, so we selected 6 clusters for global segmentation.

### How the methods compare
- KMeans (k=6): silhouette ≈ 0.356, DBI ≈ 1.005, CH ≈ 99.7 — balanced, interpretable clusters.
- Agglomerative (k=6): silhouette ≈ 0.350, DBI ≈ 1.009, CH ≈ 95.3 — extremely similar partition to KMeans.
- GMM (k=6): silhouette ≈ 0.344, DBI ≈ 1.030, CH ≈ 92.6 — similar geometry; adds soft cluster membership probabilities.
- DBSCAN: high silhouette on its non‑noise subset (≈ 0.653) but covers only a small dense pocket of ~23 customers across 4 tiny clusters. It’s great for outlier/micro‑cluster discovery, not for full‑population segmentation.

Agreement between methods (pairwise ARI)
- KMeans ↔ Agglomerative: ARI ≈ 0.892 (very high agreement).
- KMeans ↔ GMM: ARI ≈ 0.833 (strong agreement).
- Any ↔ DBSCAN: moderate agreement on the small subset it clusters; most points are excluded as noise for DBSCAN metrics.

### What the 6 clusters represent (business view)
- VIPs — High income, high spend.
- Under‑engaged affluents — High income, low spend.
- Trend‑driven youth — Low income, high spend.
- Budget‑conscious — Low income, low spend.
- Young mainstream — Mid income, mid spend (younger).
- Quality/comfort‑focused — Mid income, mid spend (older).

These personas align with PCA/t‑SNE patterns and summary stats in the profiling section. Gender skews are mild, with a male skew in the high‑income/low‑spend group.

### Why KMeans as the primary segmentation?
- Best overall silhouette among full‑coverage models; simple to operationalize and explain.
- High agreement with Agglomerative and good agreement with GMM — the structure is robust across algorithms.
- You can still use GMM for soft probabilities and DBSCAN to surface dense pockets/outliers for special handling.

### Caveats & limitations
- Small dataset (n=200) with only three numeric drivers; results can shift with additional behavioral signals.
- “SpendingScore” is a constructed metric; its definition matters for interpretation.
- PCA plots are 2D projections that can distort some separations.
- Unsupervised segments are descriptive, not causal — validate uplift through targeted experiments.

### How to use this in practice
- Treat KMeans labels as operational segments; map to friendly names and save to CSV (see Save Artifacts cell).
- Build segment‑specific lifecycle journeys and monitor uplift; refresh clusters periodically and watch silhouette/size drift.
- Enrich with behavior (recency, frequency, basket value, channel) and re‑cluster to refine personas.
