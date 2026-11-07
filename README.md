# Air-Quality-Model
# Air Quality Clustering with K-Means

**ML Theory Assignment – Q1** · Python · Google Colab

## Purpose
Group daily air-quality readings from Indian cities into pollution "profiles" using the unsupervised **K-means** algorithm. The result shows which kinds of pollution days exist and which cities spend the most time in the worst group, which helps authorities decide where to focus action.

## Dataset
[Air Quality Data in India (2015–2020)](https://www.kaggle.com/datasets/rohanrao/air-quality-data-in-india) from Kaggle, file `city_day.csv` (daily readings for 26 cities).
Features used: `PM2.5, PM10, NO2, SO2, CO, O3`. The official `AQI_Bucket` column is **not** used for clustering, only to check the result afterwards.

## How to run
1. Open `air_quality_kmeans.ipynb` in Google Colab (Upload → Colab, or open from GitHub).
2. Run all cells. The data downloads automatically with `kagglehub`; if that fails, the notebook asks you to upload `city_day.csv`.

## Steps and purpose
| # | Step | Why |
|---|------|-----|
| 1 | Load data | Get daily pollutant readings |
| 2 | Clean: fill gaps with city median, clip top 1% outliers | Missing values and sensor spikes distort distances |
| 3 | StandardScaler | K-means uses distance, so all pollutants must share one scale |
| 4 | Elbow (inertia/loss) + silhouette | Choose the number of clusters K |
| 5 | Fit K-means (k-means++, `n_init=10`) | Final model |
| 6 | PCA 2D plot and cluster profiles | See and interpret the clusters |
| 7 | Compare with AQI buckets, rank cities | Real-life validation and use |
| 8 | Optimization experiments | Improve quality and speed |
| 9 | Overfitting / underfitting checks | Confirm the model generalises |

## How the code is optimized
K-means has no optimizer, activation function or data augmentation, so the equivalent tuning levers were tested: initialisation (random vs k-means++), number of restarts (`n_init`), outlier clipping, PCA, and MiniBatchKMeans. Results are compared by time, inertia, silhouette and Davies-Bouldin score.

## Loss graph and over/underfitting
* **Loss graph:** inertia (the K-means loss) vs K, the elbow curve.
* **Accuracy equivalent:** silhouette score and Davies-Bouldin index. K-means is unsupervised, so plain accuracy does not exist.
* **Overfitting/underfitting:** train vs test loss and silhouette across K, plus stability across random seeds (Adjusted Rand Index).
  * Small K, high loss on both sets → underfitting
  * Large K, widening train/test gap → overfitting

## Tech
Python, pandas, NumPy, scikit-learn, matplotlib, seaborn.
