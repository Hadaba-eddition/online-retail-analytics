#  Online Retail II — Customer Analytics & Revenue Intelligence

An end-to-end analytics pipeline on the UCI Online Retail II dataset covering data cleaning, EDA, RFM segmentation, KMeans clustering, market basket analysis, and predictive modelling.

---

##  Overview

> Full analytics journey from raw transactional data to customer segments and ML predictions.

| What | Details |
|------|---------|
| **Dataset** | UCI Online Retail II (`online_retail_II.xlsx`) |
| **Scope** | Invoices, products, customers, countries |
| **Target tasks** | Revenue prediction + High-value customer classification |
| **Segmentation** | RFM scoring + KMeans clustering (k=4) |
| **Basket Analysis** | Association rules (support, confidence, lift) |

---

##  Pipeline

| # | Stage | Description |
|---|-------|-------------|
| 1 | **Data Quality Audit** | Missing values, duplicates, cancellations, non-product codes, zero prices |
| 2 | **Cleaning** | 7-step logged pipeline — deduplicate, remove non-products, separate returns, cap outliers (IQR Winsorisation) |
| 3 | **Feature Engineering** | `TotalPrice`, `Year`, `Month`, `DayOfWeek`, `Hour`, `Quarter`, `IsWeekend` |
| 4 | **Customer Aggregation** | `TotalRevenue`, `TotalOrders`, `TotalItems`, `Recency`, `UniqueProducts`, `LifespanDays` |
| 5 | **EDA** | Monthly trends, top products, geographic revenue, temporal patterns, distributions |
| 6 | **Return Analysis** | Monthly return value, top returned products, net vs gross revenue |
| 7 | **RFM Segmentation** | Quintile-based R/F/M scoring → 8 named segments |
| 8 | **KMeans Clustering** | Elbow method → k=4 clusters: VIP, Loyal, Occasional, Churned |
| 9 | **Market Basket Analysis** | Pairwise association rules on top-30 UK products (support ≥ 1%) |
| 10 | **Linear Regression** | Predict `log(TotalRevenue)` from customer features |
| 11 | **Logistic Regression** | Classify customers as High-Value (top 25% revenue) vs Regular |

---

##  Data Cleaning Steps

| Step | Action |
|------|--------|
| Fix data types | Invoice, StockCode, InvoiceDate, Customer ID |
| Remove exact duplicates | Deduplication |
| Remove non-product StockCodes | POST, DOT, ADJUST, AMAZONFEE, etc. |
| Separate cancellations | C-prefix invoices kept in `df_returns` for return analysis |
| Remove negative Quantity | Non-cancellation anomalies dropped |
| Remove zero/negative Price | Invalid pricing rows dropped |
| Standardise Description | Strip, uppercase, resolve to mode per StockCode |
| Outlier capping | IQR Winsorisation on Quantity and Price (no rows dropped) |

---

##  RFM Segments

| Segment | Description |
|---------|-------------|
| **Champions** | R≥4, F≥4, M≥4 — best customers |
| **Loyal** | R≥3, F≥3 — consistent buyers |
| **Potential Loyalists** | R≥3, M≥3 — high spend, moderate frequency |
| **New Customers** | R≥4, F≤2 — recently acquired |
| **At Risk** | R≤2, F≥4 — previously frequent, going quiet |
| **Needs Attention** | R≤2, F≥2 — declining engagement |
| **About to Sleep** | Mid-range on all dimensions |
| **Lost** | R=1, F=1 — disengaged |

---

##  KMeans Clusters (k=4)

| Cluster | Profile |
|---------|---------|
| **VIP / High-Value** | Low recency, high monetary |
| **Loyal / Regular** | Above-median frequency |
| **Occasional** | Mid-range across all dimensions |
| **Churned / Inactive** | High recency (haven't bought in a long time) |

---

##  ML Models

### Linear Regression — Predict Customer Revenue

| Feature | Role |
|---------|------|
| `Recency` | Days since last order |
| `TotalOrders` | Order count |
| `UniqueProducts` | Product variety |
| `LifespanDays` | Days between first and last purchase |

> Target: `log1p(TotalRevenue)` — log-transformed to handle skew

### Logistic Regression — Classify High-Value Customers

> Binary target: top 25% revenue customers = `HighValue = 1`

Metrics reported: Accuracy, Precision, Recall, F1, ROC-AUC, Confusion Matrix

---

##  Visualisations Generated

| Plot | Description |
|------|-------------|
| Data quality audit | Horizontal bar — affected records per issue |
| Monthly trends | Revenue, Orders, Customers over time |
| Top 15 products | By revenue and by units sold |
| Geographic revenue | Top 12 countries + international (excl. UK) |
| Temporal patterns | Revenue by hour, day of week, quarter |
| Value distributions | Quantity, Price, TotalPrice histograms |
| Customer behaviour | Orders, revenue, recency per customer |
| Return analysis | Monthly return value + top returned products |
| RFM segmentation | Customers per segment, avg revenue, RFM scatter |
| Elbow method | Inertia vs k for KMeans |
| Cluster profiles | Recency vs Monetary scatter + profile heatmap |
| Market basket | Support vs Confidence scatter + top-15 lift rules |
| Linear regression | Predicted vs Actual, residuals, coefficients |
| Logistic regression | Confusion matrix, ROC curve, log-odds coefficients |

---

##  Getting Started

### 1. Clone the repo
```bash
git clone https://github.com/Hadaba-eddition/online-retail-analytics.git
cd online-retail-analytics
```

### 2. Install dependencies
```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl
```

### 3. Add the dataset
Place `online_retail_II.xlsx` in the project root.  
Download from the [UCI repository](https://archive.ics.uci.edu/ml/datasets/Online+Retail+II).

### 4. Run
```bash
jupyter notebook HADABA_Online Retail.ipynb
```

---

##  Requirements

| Package | Minimum |
|---------|---------|
| Python | 3.8 |
| pandas | 1.3 |
| numpy | 1.21 |
| matplotlib | 3.4 |
| seaborn | 0.11 |
| scikit-learn | 1.0 |
| openpyxl | 3.0 |

---

##  Repository Structure

```
online-retail-analytics/
├── HADABA_Online Retail.ipynb    # Main notebook
└── README.md
```

---

##  Citation

Dataset: [UCI Machine Learning Repository — Online Retail II](https://archive.ics.uci.edu/ml/datasets/Online+Retail+II)

> Chen, D., Sain, S.L., & Guo, K. (2012). Data mining for the online retail industry. *Journal of Database Marketing & Customer Strategy Management*, 19(3), 197–208.
