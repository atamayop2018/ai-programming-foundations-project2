# Florida Real Estate Sales Statistical Analysis Report

## 1. Overview
This report summarizes an exploratory and inferential analysis of the Florida real-estate transactions dataset (10,893 closed sales). The objective is to demonstrate statistical reasoning and reproducibility by computing descriptive statistics, producing visualizations, conducting hypothesis tests, and communicating results in accessible language [1], [2].

## 2. Dataset Description
Dataset source: Kaggle, Florida Homes Sold – Real Estate Transactions (2026). Each record represents a verified closed residential sale with fields like `sale_price`, `living_area`, `bedrooms`, `bathrooms`, `zipcode`, `year_built`, and textual property descriptions.

Key details:
- Total observations after initial load: ~10,893
- Sampled numeric fields: `sale_price`, `living_area`, `bedrooms`, `bathrooms`, `year_built`
- Geographic stratification: `zipcode`
- Data cleaning: removed rows missing `sale_price`, `living_area`, or `zipcode`; trimmed top/bottom 1% of `sale_price` to reduce extreme outlier influence.

## 3. Methods
### 3.1. Environment and reproducibility
- Python 3.x, numpy, pandas, matplotlib, seaborn, scipy.
- Reproducibility steps: `np.random.seed(2026)`, captured package versions, and saved `requirements.txt`.
- Following reproducible research principles and transparent computational workflow guidance [1], with emphasis on effect-focused interpretation and uncertainty reporting [2].

### 3.2. Descriptive analysis
- Summary statistics (`mean`, `median`, `std`) for `sale_price` and `living_area`.
- Zip-code frequency table for top locations.

### 3.3. Visualizations
- Histogram of sale price distribution.
- Scatter plot of living area vs sale price.
- Bar chart of mean sale price for top 6 zip codes by count.

### 3.4. Hypothesis testing
Hypothesis test A: ANOVA for sale price by ZIP (top 3 ZIP groups)
- Null (H0): mean sale price is equal across ZIP groups.
- Alternative (H1): at least one group mean differs.
- Result: significant at α=0.05, implying location matters for average sale price differences across ZIP groups [2], [3].

Hypothesis test B: Pearson correlation between living area and sale price
- Null (H0): correlation ρ = 0
- Alternative (H1): ρ ≠ 0
- Result: significant positive correlation (p < 0.05), with 95% confidence interval reported in the notebook [2], [3].

## 4. Results
- `sale_price` exhibits positive skew; mean > median, indicating a tail of high-end properties.
- Average sale price differs by ZIP code: geographical segmentation is statistically meaningful.
- Living area shows a statistically significant moderate-to-strong positive correlation with sale price (p < 0.001) with a reported 95% CI.

## 5. Interpretation for a Non-Technical Audience
Home values in Florida are not uniform: both location (ZIP code) and house size matter. Larger homes generally sell for higher prices, and properties in some ZIP codes command significantly different average prices. This suggests that customers and appraisers should consider neighborhood and floor area when estimating resale value.

## 6. Limitations and Potential Bias
- Dataset only includes closed sales, so it does not capture unsold listings or temporal market shifts.
- Price trimming (1%-99%) removes extreme high-end or distressed sales, which may affect model boundaries.
- ANOVA on top ZIP codes simplifies spatial complexity; additional interaction terms (e.g., zipcode*bedrooms) may be warranted.
- Unobserved variables (e.g., lot size, school quality, renovation status) may confound observed relationships.
- Statistical significance does not by itself imply practical significance, so effect sizes and confidence intervals should be interpreted alongside p-values [2].

## 7. References
1. Peng, R. D. (2011). Reproducible Research in Computational Science. *Science*, 334(6060), 1226-1227.
2. Cumming, G. (2014). The new statistics: Why and how. *Psychological Science*, 25(1), 7-29.
3. McElreath, R. (2016). *Statistical Rethinking: A Bayesian Course with Examples in R and Stan* (for hypothesis testing reasoning).
4. McKinney, W. (2018). *Python for Data Analysis* (for pandas practices and data cleaning).