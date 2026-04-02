# Florida Real Estate Sales Statistical Analysis Report

## 1. Overview
This report presents an exploratory and inferential analysis of 10,893 closed residential sales drawn from the Florida real-estate market. The work follows reproducible-research principles [1] and emphasizes effect-size interpretation alongside traditional significance testing [2].

### Research Questions
Two primary questions guide the analysis:

1. **Does average sale price differ across Florida ZIP codes?** If location alone is associated with meaningful price variation, geographic segmentation should be considered for valuation models.
2. **Is there a significant linear relationship between living area (square footage) and sale price?** Quantifying this relationship helps buyers, sellers, and appraisers understand how home size contributes to market value.

To answer these questions the report walks through data loading and cleaning, descriptive statistics, three supporting visualizations, and two formal hypothesis tests (one-way ANOVA and Pearson correlation).

## 2. Dataset Description
Dataset source: Kaggle, Florida Homes Sold – Real Estate Transactions (2026). Each record represents a verified closed residential sale.

### 2.1. Structure
- **Total observations after initial load:** 10,893 rows across 14 columns.
- **Key numeric fields (float64/int64):** `sale_price`, `living_area` (sqft), `bedrooms`, `bathrooms`, `year_built`.
- **Categorical/text fields:** `zipcode` (converted to string for grouping), property descriptions, address components.
- **Geographic stratification:** `zipcode` — used for spatial segmentation and group comparisons.

### 2.2. Data Cleaning
- Dropped rows missing `sale_price`, `living_area`, or `zipcode` → reduced to 10,135 rows [4].
- Trimmed the top and bottom 1 % of `sale_price` (values below $35,170 and above ~$3.71 M) to limit extreme outlier influence → final working set of **9,931 rows**.
- Zip codes cast from float to string to prevent numeric misinterpretation in grouping operations [4].

### 2.3. Data Quality Notes
- 757 rows had missing `living_area`, 676 missing `bedrooms`, 678 missing `year_built`, and 639 missing `bathrooms`. These were retained where possible and only excluded when required by a specific analysis step.
- 2 rows had missing `zipcode` and were dropped during cleaning.
- No duplicate-detection step was applied; duplicate listings sharing the same address could remain.

## 3. Methods
### 3.1. Environment and reproducibility
- Python 3.13.2, numpy 2.4.4, pandas 2.2.2, matplotlib 3.10.8, seaborn 0.13.2, scipy 1.17.1.
- Reproducibility steps: `np.random.seed(2026)`, captured package versions, and saved `requirements.txt` [1].
- All code resides in `analysis.ipynb` and can be re-executed end-to-end to regenerate every figure and statistic in this report [1].
- Interpretation follows effect-focused reporting and uncertainty quantification best practices [2].

### 3.2. Descriptive analysis
- Computed central-tendency and dispersion measures (`mean`, `median`, `std`) for the two primary numeric features.
- Tabulated zip-code frequencies to identify the most-represented geographic areas.

**Computed values (after cleaning):**

| Measure | `sale_price` (USD) | `living_area` (sq ft) |
|---------|-------------------:|----------------------:|
| Mean    | 511,347.06         | 1,828.20              |
| Median  | 378,000.00         | 1,675.00              |
| Std Dev | 461,695.97         | 839.81                |

The mean sale price substantially exceeds the median, confirming a right-skewed distribution driven by a tail of higher-priced properties. Living area shows a similar but less pronounced skew.

### 3.3. Visualizations
Three labeled visualizations were produced, each chosen to support a specific analytical conclusion:

1. **Histogram — Sale Price Distribution (trimmed 1 %–99 %):** Reveals the strong right skew discussed above. Most transactions cluster between $100 K and $500 K, with a long tail extending past $2 M. This shape justifies using both mean and median and informs the choice of parametric tests (ANOVA is fairly robust to moderate skew with large samples).

2. **Scatter Plot — Living Area vs Sale Price:** Visualizes the positive linear trend that the Pearson correlation test later quantifies. The dense cloud of points in the lower-left confirms that the majority of homes are under 3,000 sq ft and below $1 M, while a handful of large-area outliers appear in the upper-right.

3. **Bar Chart — Average Sale Price for Top 6 ZIP Codes:** Shows that average prices differ meaningfully across the most-represented ZIP codes (ranging from ~$290 K to ~$650 K), providing visual motivation for the ANOVA test. ZIP 34211 and 34787 stand out as the highest-average areas.

### 3.4. Hypothesis testing
**Hypothesis Test A — One-Way ANOVA: Sale Price by ZIP Code (top 3 groups: 34219, 34293, 32092)**
- **Null (H0):** μ₁ = μ₂ = μ₃ — mean sale price is equal across the three ZIP groups.
- **Alternative (H1):** At least one group mean differs.
- **Test statistic:** F = 5.455, **p = 0.00486**.
- **Decision:** Reject H0 at α = 0.05. There is statistically significant evidence that mean sale price differs among these ZIP areas [2], [3].

**Hypothesis Test B — Pearson Correlation: Living Area vs Sale Price**
- **Null (H0):** ρ = 0 (no linear relationship).
- **Alternative (H1):** ρ ≠ 0.
- **Test statistic:** r = 0.579, **p < 0.001**.
- **95 % Confidence Interval for r:** (0.566, 0.592) via Fisher z-transformation.
- **Decision:** Reject H0. A moderate-to-strong positive linear correlation exists between living area and sale price [2], [3].

## 4. Results

### 4.1. Answering Research Question 1 — Do prices differ by ZIP code?
Yes. The one-way ANOVA across the three most-represented ZIP codes yielded F = 5.455 and p = 0.005, well below the α = 0.05 threshold. This provides statistically significant evidence that mean sale price is not equal across geographic areas [2], [3]. The bar chart reinforces this finding visually, with average prices spanning from ~$290 K to ~$650 K across the top six ZIP codes.

### 4.2. Answering Research Question 2 — Is living area linearly related to sale price?
Yes. The Pearson correlation is r = 0.579 (95 % CI [0.566, 0.592], p < 0.001), indicating a moderate-to-strong positive linear relationship [2]. Approximately 33.5 % of the variance in sale price is explained by living area alone (r² ≈ 0.335). The scatter plot visually confirms the upward trend.

### 4.3. Supporting descriptive findings
- `sale_price` is right-skewed (mean $511 K vs median $378 K), confirming a tail of high-end properties [4].
- Living area averages 1,828 sq ft with moderate dispersion (std ≈ 840 sq ft).
- The histogram makes the skewed price distribution easy to see, justifying the use of both mean and median as complementary summaries [2].

## 5. Interpretation for a Non-Technical Audience

**What did we find?**
Home values in Florida are not uniform — both location and house size play a clear role in what a property sells for.

**Location matters.** When we compared average sale prices in the three most-common ZIP codes, we found a statistically meaningful difference. For example, homes in ZIP 34211 (Bradenton area) averaged roughly $650 K, while homes in ZIP 33844 (Haines City area) averaged about $290 K. A statistical test confirmed these differences are unlikely to be due to chance alone (p = 0.005).

**Bigger homes sell for more.** There is a clear upward trend: as living area increases, sale price tends to rise as well. The correlation coefficient of 0.58 (on a 0-to-1 scale) indicates a moderately strong relationship. In practical terms, about one-third of the variation in sale price can be attributed to differences in square footage.

**Most homes are moderately priced.** The typical (median) sale price is $378,000, though a small number of luxury properties push the average up to $511,000. The histogram in the notebook makes this skew easy to see.

**What does this mean for buyers, sellers, and appraisers?** Neighborhood and floor area are two of the most accessible factors for estimating resale value. However, many other variables (lot size, condition, school district) also influence price and are not captured here.

## 6. Limitations and Potential Bias

### 6.1. Data Quality Issues
- **Missing values:** 757 rows (~7 %) lacked `living_area`, 676 (~6 %) lacked `bedrooms`, and 678 (~6 %) lacked `year_built`. If missingness is non-random (e.g., older properties less likely to have digitized records), the cleaned subset may not fully represent the population.
- **No duplicate detection:** The pipeline does not de-duplicate records. Duplicate listings at the same address could inflate counts for certain ZIP codes and bias group-level statistics.
- **Zipcode as float:** The raw dataset stored ZIP codes as floating-point numbers (e.g., 34219.0), which were cast to strings during cleaning. Any ZIP codes that failed to parse or were entered incorrectly would not be caught.

### 6.2. Scope and Selection Bias
- The dataset contains only **closed sales** — it excludes withdrawn, expired, or active listings, so it reflects realized market prices rather than the full listing landscape.
- Price trimming (1 %–99 %) intentionally removes extreme high-end and distressed sales, which may understate the true range and variance of the market.
- ANOVA was applied to only the top 3 ZIP codes by record count; results should not be generalized to all Florida ZIP codes.

### 6.3. Analytical Limitations
- ANOVA assumes equal variances across groups (homoscedasticity) and approximate normality. With large samples ANOVA is robust, but formal Levene's test was not performed.
- Pearson correlation captures only **linear** relationships; a non-linear pattern between living area and price could be missed.
- Unobserved confounders (lot size, school quality, renovation status, proximity to amenities) may partially explain the observed correlations.
- Statistical significance does not by itself imply practical significance — effect sizes (r = 0.579, r² ≈ 0.335) and confidence intervals should be interpreted alongside p-values [2].

## 7. Next Steps
The following extensions would strengthen the analysis and address the limitations identified above:

1. **Multiple regression modeling.** Include `living_area`, `bedrooms`, `bathrooms`, `year_built`, and `zipcode` as predictors in a multivariate regression to quantify the independent contribution of each feature and reduce confounding [3].
2. **Non-linear exploration.** Fit polynomial or spline models for living area vs sale price to test whether a non-linear specification improves explanatory power beyond the current Pearson correlation.
3. **Assumption testing.** Run Levene's test for homogeneity of variances and Shapiro–Wilk tests on ANOVA residuals to formally validate parametric assumptions [3].
4. **Broader geographic scope.** Extend the ANOVA to more ZIP codes or use a mixed-effects model that treats ZIP as a random factor, improving generalizability.
5. **Temporal analysis.** If sale-date information becomes available, model price trends over time to account for market cycles and seasonal effects.
6. **Duplicate detection.** Add a de-duplication step keyed on address and sale price to prevent inflated counts in group-level statistics [4].
7. **External data enrichment.** Join school-district ratings, crime statistics, or proximity-to-amenity scores to capture unobserved confounders identified in the limitations.

## 8. References
1. Peng, R. D. (2011). Reproducible Research in Computational Science. *Science*, 334(6060), 1226-1227.
2. Cumming, G. (2014). The new statistics: Why and how. *Psychological Science*, 25(1), 7-29.
3. McElreath, R. (2016). *Statistical Rethinking: A Bayesian Course with Examples in R and Stan* (for hypothesis testing reasoning).
4. McKinney, W. (2018). *Python for Data Analysis* (for pandas practices and data cleaning).