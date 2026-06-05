# Data Science Playbook

> Load this file when running analysis on scraped data. Use as a method reference.

---

## Table of Contents
1. [Dataset Overview](#overview)
2. [Descriptive Statistics](#descriptive)
3. [Distribution Analysis](#distributions)
4. [Correlation Analysis](#correlation)
5. [Outlier Detection](#outliers)
6. [ML Patterns & Clustering](#ml)
7. [Time Series & Forecasting](#timeseries)
8. [Hypothesis Testing](#hypothesis)
9. [Data Quality Assessment](#quality)
10. [Visualisation Guide](#viz)
11. [Quick Code Templates](#code)

---

## Dataset Overview {#overview}

Always start here. Report:

```python
import pandas as pd
import numpy as np

df = pd.read_csv("scraped_data.csv")

print(f"Shape: {df.shape[0]} rows × {df.shape[1]} columns")
print(f"\nColumn types:\n{df.dtypes}")
print(f"\nMissing values:\n{df.isnull().sum()}")
print(f"\nMissing %:\n{(df.isnull().sum() / len(df) * 100).round(2)}")
print(f"\nDuplicates: {df.duplicated().sum()}")
print(f"\nMemory usage: {df.memory_usage(deep=True).sum() / 1024:.1f} KB")
```

**What to report:**
- Row/column count
- Which columns are numeric vs categorical vs datetime
- Missing value counts + percentages
- Duplicate row count
- Obvious data type mismatches (e.g. price stored as string)

---

## Descriptive Statistics {#descriptive}

```python
# Numeric columns
numeric_stats = df.describe(percentiles=[.1, .25, .5, .75, .9]).round(2)

# Categorical columns
for col in df.select_dtypes(include=['object']).columns:
    print(f"\n{col}:")
    print(f"  Unique values: {df[col].nunique()}")
    print(f"  Top 5:\n{df[col].value_counts().head()}")
    print(f"  Coverage: {df[col].notna().sum() / len(df) * 100:.1f}%")
```

**Key metrics to surface:**
| Metric | Meaning | Flag if... |
|---|---|---|
| Mean vs Median gap | Skew indicator | Gap > 20% of mean |
| Std / Mean (CV) | Relative variability | CV > 1.0 = high variance |
| Min/Max ratio | Range spread | Extreme ratio suggests outliers |
| Unique count | Cardinality | ~100% unique = likely ID column |
| Zero count | Sparse data | >20% zeros may need special treatment |

---

## Distribution Analysis {#distributions}

```python
from scipy import stats
import matplotlib.pyplot as plt
import seaborn as sns

def analyse_distribution(series, name):
    clean = series.dropna()
    
    skewness = clean.skew()
    kurtosis = clean.kurtosis()
    
    # Normality test (use D'Agostino for n > 20, Shapiro-Wilk for n <= 20)
    if len(clean) <= 20:
        stat, p = stats.shapiro(clean)
        test_name = "Shapiro-Wilk"
    elif len(clean) <= 5000:
        stat, p = stats.normaltest(clean)
        test_name = "D'Agostino"
    else:
        stat, p = stats.kstest(clean, 'norm', 
                               args=(clean.mean(), clean.std()))
        test_name = "Kolmogorov-Smirnov"
    
    print(f"{name}:")
    print(f"  Skewness: {skewness:.3f} "
          f"({'right-skewed' if skewness > 0.5 else 'left-skewed' if skewness < -0.5 else 'symmetric'})")
    print(f"  Kurtosis: {kurtosis:.3f} "
          f"({'heavy-tailed' if kurtosis > 1 else 'light-tailed' if kurtosis < -1 else 'normal-tailed'})")
    print(f"  {test_name} normality test: p={p:.4f} "
          f"({'NOT normal' if p < 0.05 else 'approx. normal'})")
```

**Skewness interpretation:**
- `-0.5 to 0.5` → approximately symmetric
- `0.5 to 1.0` → moderately right-skewed (long right tail)
- `> 1.0` → highly right-skewed (prices, revenue often look like this)
- `< -0.5` → left-skewed

**Kurtosis interpretation:**
- `> 1` → leptokurtic (heavy tails, more outliers than normal)
- `< -1` → platykurtic (light tails, fewer extreme values)

---

## Correlation Analysis {#correlation}

```python
# Pearson — for normally distributed numeric data
pearson_corr = df.select_dtypes(include=np.number).corr(method='pearson')

# Spearman — for non-normal or ordinal data (more robust)
spearman_corr = df.select_dtypes(include=np.number).corr(method='spearman')

# Top correlated pairs
def top_correlations(corr_matrix, n=10):
    pairs = (corr_matrix
             .unstack()
             .reset_index()
             .rename(columns={'level_0': 'var1', 'level_1': 'var2', 0: 'correlation'}))
    pairs = pairs[pairs['var1'] < pairs['var2']]  # Remove duplicates
    pairs['abs_corr'] = pairs['correlation'].abs()
    return pairs.nlargest(n, 'abs_corr')

print(top_correlations(spearman_corr))
```

**Correlation strength guide:**
| |r| | Interpretation |
|---|---|
| 0.0 – 0.2 | Negligible |
| 0.2 – 0.4 | Weak |
| 0.4 – 0.6 | Moderate |
| 0.6 – 0.8 | Strong |
| 0.8 – 1.0 | Very strong |

**Always note:** Correlation ≠ causation. Flag confounders where obvious.

---

## Outlier Detection {#outliers}

Use multiple methods — each catches different types:

```python
def detect_outliers(df, numeric_cols):
    outlier_report = {}
    
    for col in numeric_cols:
        series = df[col].dropna()
        outliers = {}
        
        # Method 1: IQR (robust, good for skewed data)
        Q1, Q3 = series.quantile(0.25), series.quantile(0.75)
        IQR = Q3 - Q1
        iqr_outliers = df[(df[col] < Q1 - 1.5 * IQR) | 
                           (df[col] > Q3 + 1.5 * IQR)].index
        outliers['iqr'] = len(iqr_outliers)
        
        # Method 2: Z-score (assumes normality, good for symmetric data)
        z_scores = np.abs(stats.zscore(series))
        z_outliers = df[z_scores > 3].index
        outliers['z_score'] = len(z_outliers)
        
        # Method 3: Modified Z-score (robust version)
        median = series.median()
        mad = np.median(np.abs(series - median))
        mod_z = 0.6745 * (series - median) / mad if mad != 0 else pd.Series(0, index=series.index)
        mod_z_outliers = mod_z[np.abs(mod_z) > 3.5].index
        outliers['modified_z'] = len(mod_z_outliers)
        
        outlier_report[col] = outliers
    
    return outlier_report

# Isolation Forest — catches multivariate outliers
from sklearn.ensemble import IsolationForest

def multivariate_outliers(df, numeric_cols, contamination=0.05):
    X = df[numeric_cols].dropna()
    iso = IsolationForest(contamination=contamination, random_state=42)
    labels = iso.fit_predict(X)
    outlier_rows = X[labels == -1]
    print(f"Multivariate outliers detected: {len(outlier_rows)} rows ({contamination*100:.0f}% threshold)")
    return outlier_rows
```

---

## ML Patterns & Clustering {#ml}

### Clustering (unsupervised — find natural groups)

```python
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

def find_clusters(df, numeric_cols, max_k=8):
    X = df[numeric_cols].dropna()
    scaler = StandardScaler()
    X_scaled = scaler.fit_transform(X)
    
    # Elbow method — find optimal k
    inertias = []
    silhouettes = []
    K_range = range(2, min(max_k + 1, len(X)))
    
    for k in K_range:
        km = KMeans(n_clusters=k, random_state=42, n_init=10)
        labels = km.fit_predict(X_scaled)
        inertias.append(km.inertia_)
        silhouettes.append(silhouette_score(X_scaled, labels))
    
    best_k = K_range[np.argmax(silhouettes)]
    print(f"Optimal clusters: k={best_k} (silhouette={max(silhouettes):.3f})")
    
    km_final = KMeans(n_clusters=best_k, random_state=42, n_init=10)
    df['cluster'] = km_final.fit_predict(X_scaled)
    
    # Cluster profiles
    print(df.groupby('cluster')[numeric_cols].mean().round(2))
    return df

# Silhouette score interpretation
# > 0.7: strong structure
# 0.5–0.7: reasonable structure
# 0.25–0.5: weak structure
# < 0.25: no meaningful clusters
```

### Feature Importance (which columns matter most)

```python
from sklearn.ensemble import RandomForestRegressor, RandomForestClassifier
from sklearn.preprocessing import LabelEncoder

def feature_importance(df, target_col, numeric_cols):
    X = df[numeric_cols].dropna()
    y = df.loc[X.index, target_col]
    
    if y.dtype == 'object':
        le = LabelEncoder()
        y = le.fit_transform(y)
        model = RandomForestClassifier(n_estimators=100, random_state=42)
    else:
        model = RandomForestRegressor(n_estimators=100, random_state=42)
    
    model.fit(X, y)
    importance = pd.Series(model.feature_importances_, index=numeric_cols)
    return importance.sort_values(ascending=False)
```

---

## Time Series & Forecasting {#timeseries}

```python
def analyse_timeseries(df, date_col, value_col):
    df = df.copy()
    df[date_col] = pd.to_datetime(df[date_col])
    df = df.sort_values(date_col)
    df = df.set_index(date_col)
    
    series = df[value_col].dropna()
    
    # Trend: linear regression on time
    from sklearn.linear_model import LinearRegression
    X_time = np.arange(len(series)).reshape(-1, 1)
    lr = LinearRegression().fit(X_time, series.values)
    trend_direction = "upward" if lr.coef_[0] > 0 else "downward"
    trend_strength = abs(lr.score(X_time, series.values))
    
    print(f"Trend: {trend_direction} (R²={trend_strength:.3f})")
    print(f"Change per period: {lr.coef_[0]:+.4f}")
    
    # Seasonality check — rolling stats
    if len(series) >= 7:
        rolling_mean = series.rolling(7).mean()
        rolling_std = series.rolling(7).std()
        cv = rolling_std.mean() / rolling_mean.mean()
        print(f"Volatility (CV): {cv:.3f} "
              f"({'high' if cv > 0.3 else 'moderate' if cv > 0.1 else 'low'})")
    
    return series

# Simple forecast: linear extrapolation + confidence interval
def simple_forecast(series, periods=7):
    X = np.arange(len(series)).reshape(-1, 1)
    y = series.values
    
    from sklearn.linear_model import LinearRegression
    model = LinearRegression().fit(X, y)
    
    future_X = np.arange(len(series), len(series) + periods).reshape(-1, 1)
    forecast = model.predict(future_X)
    
    residuals = y - model.predict(X)
    std_err = residuals.std()
    
    print("\nForecast (next periods):")
    for i, f in enumerate(forecast, 1):
        print(f"  t+{i}: {f:.2f} ± {1.96 * std_err:.2f} (95% CI)")
    
    return forecast
```

---

## Hypothesis Testing {#hypothesis}

### Test Selection Guide

| Question | Data Type | Test |
|---|---|---|
| Are two group means different? | Numeric, 2 groups, normal | Independent t-test |
| Are two group means different? | Numeric, 2 groups, non-normal | Mann-Whitney U |
| Are 3+ group means different? | Numeric, 3+ groups | ANOVA → post-hoc Tukey |
| Are 3+ group means different? | Non-normal | Kruskal-Wallis |
| Is there a relationship? | Two numeric vars | Pearson/Spearman correlation |
| Are distributions different? | Any | Kolmogorov-Smirnov |
| Is categorical dist. as expected? | Categorical | Chi-square goodness of fit |
| Are two categoricals related? | Two categorical vars | Chi-square independence |
| Before vs after same group? | Numeric, paired | Paired t-test / Wilcoxon |

```python
def run_appropriate_test(group1, group2, alpha=0.05):
    """Auto-select and run the right two-group test"""
    from scipy import stats
    
    # Check normality
    _, p1 = stats.shapiro(group1) if len(group1) <= 50 else stats.normaltest(group1)
    _, p2 = stats.shapiro(group2) if len(group2) <= 50 else stats.normaltest(group2)
    both_normal = p1 > 0.05 and p2 > 0.05
    
    # Check equal variances (Levene's test)
    _, p_var = stats.levene(group1, group2)
    equal_var = p_var > 0.05
    
    if both_normal:
        stat, p = stats.ttest_ind(group1, group2, equal_var=equal_var)
        test_name = f"Independent t-test ({'equal' if equal_var else 'unequal'} variance)"
    else:
        stat, p = stats.mannwhitneyu(group1, group2, alternative='two-sided')
        test_name = "Mann-Whitney U"
    
    result = "SIGNIFICANT" if p < alpha else "not significant"
    print(f"{test_name}: statistic={stat:.4f}, p={p:.4f} → {result} (α={alpha})")
    
    # Effect size
    if both_normal:
        pooled_std = np.sqrt((group1.std()**2 + group2.std()**2) / 2)
        cohens_d = (group1.mean() - group2.mean()) / pooled_std if pooled_std != 0 else 0
        magnitude = "large" if abs(cohens_d) > 0.8 else "medium" if abs(cohens_d) > 0.5 else "small"
        print(f"Effect size (Cohen's d): {cohens_d:.3f} ({magnitude})")
    
    return stat, p
```

---

## Data Quality Assessment {#quality}

Always report:

```python
def data_quality_report(df):
    report = {}
    
    # Missing data
    missing = df.isnull().sum()
    report['high_missing'] = missing[missing / len(df) > 0.3].to_dict()  # >30% missing
    
    # Duplicate rows
    report['duplicate_rows'] = df.duplicated().sum()
    
    # Potential ID columns (100% unique)
    for col in df.columns:
        if df[col].nunique() == len(df):
            report.setdefault('likely_id_cols', []).append(col)
    
    # Constant columns (0 variance)
    for col in df.select_dtypes(include=np.number).columns:
        if df[col].std() == 0:
            report.setdefault('constant_cols', []).append(col)
    
    # Type mismatches (numbers stored as strings)
    for col in df.select_dtypes(include='object').columns:
        sample = df[col].dropna().head(100)
        numeric_count = pd.to_numeric(sample, errors='coerce').notna().sum()
        if numeric_count / len(sample) > 0.8:
            report.setdefault('likely_numeric_as_string', []).append(col)
    
    return report
```

---

## Visualisation Guide {#viz}

Match chart type to data type and question:

| Question | Chart Type | Python |
|---|---|---|
| Distribution of one numeric | Histogram + KDE | `sns.histplot(kde=True)` |
| Compare distributions | Box plot or violin | `sns.boxplot()` |
| Relationship between two numerics | Scatter | `sns.scatterplot()` |
| Correlation matrix | Heatmap | `sns.heatmap(corr, annot=True)` |
| Category proportions | Bar chart | `sns.countplot()` |
| Trend over time | Line chart | `plt.plot()` |
| Outlier detection | Box plot | `sns.boxplot()` |
| Cluster visualisation | Scatter with hue | `sns.scatterplot(hue='cluster')` |
| Feature importance | Horizontal bar | `importance.plot(kind='barh')` |

```python
# Standard setup for all plots
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_theme(style="whitegrid", palette="husl")
fig, axes = plt.subplots(figsize=(12, 8))
plt.tight_layout()
plt.savefig("analysis_output.png", dpi=150, bbox_inches='tight')
```

---

## Quick Code Templates {#code}

### Full pipeline in one block

```python
import pandas as pd
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import IsolationForest

# Load
df = pd.read_csv("scraped_data.csv")

# Clean
df = df.drop_duplicates()
numeric_cols = df.select_dtypes(include=np.number).columns.tolist()

# Overview
print(f"Dataset: {df.shape[0]} rows × {df.shape[1]} cols")
print(f"Missing: {df.isnull().sum().sum()} total values")

# Stats
print(df[numeric_cols].describe().round(2))

# Correlations
corr = df[numeric_cols].corr(method='spearman')
print("\nTop correlations:")
pairs = (corr.unstack().reset_index()
         .rename(columns={'level_0': 'a', 'level_1': 'b', 0: 'r'}))
pairs = pairs[pairs['a'] < pairs['b']].nlargest(5, 'r', keep='all')
print(pairs[['a', 'b', 'r']].to_string(index=False))

# Outliers
iso = IsolationForest(contamination=0.05, random_state=42)
df['is_outlier'] = iso.fit_predict(df[numeric_cols].fillna(0)) == -1
print(f"\nOutliers detected: {df['is_outlier'].sum()} rows")
```

---

*Methods align with standard statistical practice. For n < 30, prefer non-parametric tests. Always check assumptions before applying parametric methods.*
