# Movie Success Analysis Project

## Overview

This project reproduces and extends the 2019 analysis by Gao et al. on "What Makes a Successful Movie?" from both financial and critical perspectives. We analyze **3,783 Hollywood theatrical releases from 1996-2024** with complete data (IMDb ratings + revenue), extending the original 1996-2016 dataset with 8 additional years capturing the streaming era and COVID-19 pandemic disruption.

**Research Questions:**
1. What factors contribute to both financial AND critical success in movies?
2. How have the determinants of movie success evolved from 1996-2016 to 2017-2024?
3. Do actor/director career histories remain predictive in the modern era?
4. What role do genre, runtime, and creative characteristics play?

---

## Quick Start (5 minutes)

### Step 1: Download IMDb Data
Download the following files from [IMDb Datasets](https://datasets.imdbws.com/):
- [`name.basics.tsv.gz`](https://datasets.imdbws.com/name.basics.tsv.gz)
- [`title.basics.tsv.gz`](https://datasets.imdbws.com/title.basics.tsv.gz)
- [`title.principals.tsv.gz`](https://datasets.imdbws.com/title.principals.tsv.gz)
- [`title.ratings.tsv.gz`](https://datasets.imdbws.com/title.ratings.tsv.gz)

Place all files in the **`src/data/raw/`** folder.

### Step 2: Run the Analysis
Open R and execute the main analysis script:

```R
# Run from project root directory
source("src/movie_analysis.qmd")
```

**Expected runtime:** 5-10 minutes

### Step 3: View Results
- Publication-ready figures in `figures/`
- Processed data in `data/processed/movies_analysis.csv`
- Statistical results in `results/final/`

---

## Key Findings

### Result 1: Genre Dominates Success
- **Biography films**: 86.6% achieve dual success (financial + critical)
- **Documentary films**: 82.1% dual success (surged to #1 in recent period)
- **Animation films**: 65.8% dual success
- **Drama films**: 62.9% dual success
- **Horror films**: Only 28.0% dual success (lowest)

**Statistical Significance:** ANOVA F(6,3527) = 49.47, p < 0.001 (explains 7.8% of rating variance)

### Result 2: Popularity Predicts Quality
- **Q4 (High Popularity) films**: 73.2% dual success, avg rating 6.78
- **Q1 (Low) films**: 38.2% dual success, avg rating 5.82

**Statistical Significance:** ANOVA F(3,3779) = 187.4, p < 0.001 (explains 12.9% of variance)

### Result 3: Multi-Genre Films Rate Higher
- **Single-genre films**: Avg 6.08 rating, 49.3% dual success
- **Two-genre films**: Avg 6.29 rating, 55.8% dual success
- **Four-genre films**: Avg 6.41 rating, 59.1% dual success

**Statistical Significance:** t(3783) = 5.73, p < 0.001

### Result 4: Temporal Shift - Documentary Surge

| Metric | 1996-2016 | 2017-2024 | Change |
|--------|-----------|-----------|--------|
| Documentary Dual Success | 78.9% | 88.2% | +9.3 pp |
| Crime (Top 5 Entry) | N/A | 60.2% | NEW |
| Overall Dual Success | 52.8% | 57.6% | +4.8 pp |

### Result 5: Linear Regression - Predicting Ratings

**Model:** Rating ~ Runtime + log(Votes) + log(Revenue) + Genre indicators

Key Coefficients:
- **Horror Genre**: -0.97 (heavily penalized)
- **Drama Genre**: +0.19 (bonus for critics)
- **Popularity**: +0.09 per log(votes)
- **Runtime**: +0.087 per hour (longer = better)
- **Budget**: +0.062 (weakest effect)

**Model Performance:** R² = 0.165 (16.5% variance explained), F(7,3775) = 106.49, p < 0.001

---

## Dataset

- **Total films analyzed:** 7,783 movies (1996-2024) with complete data
- **Mean rating:** 6.24/10 (SD = 1.14)
- **Median revenue:** $53.3 million
- **Mean revenue:** $131.5 million
- **Time periods:** Historical (1996-2016) vs. Modern (2017-2024)

### Data Sources
- **IMDb Datasets:** Ratings (100-300k votes per film), cast/crew, metadata
- **Box Office Mojo:** Worldwide, domestic, foreign revenue
- **TMDb:** Budget information (where available)
- **Initial screening:** 284,528 movies reduced to 3,783 with complete data

### Data Processing
- Currency standardization: All revenue converted to USD
- Genre extraction: 15 binary indicators
- Temporal features: Decade and era classifications
- Budget estimation: 30% of revenue (industry standard)

---

## Methodology

### Statistical Techniques
1. **Exploratory Data Analysis (EDA):** Distribution plots, summary statistics, visual patterns
2. **ANOVA:** Testing genre effects on ratings (F-tests, Tukey post-hoc comparisons)
3. **Correlation Analysis:** Pearson and Spearman correlations, dimensionality reduction (PCA)
4. **Linear Regression:** Predicting ratings with full diagnostic testing
5. **Logistic Regression:** Predicting critical success (binary outcome)
6. **T-tests:** Comparing groups (multi-genre vs single-genre films)

### Model Diagnostics
All regression models validated for:
- **Normality:** Shapiro-Wilk test (W=0.979, residuals approximately normal)
- **Homoscedasticity:** Breusch-Pagan test (BP=246.24, minor heteroscedasticity acceptable)
- **Independence:** Durbin-Watson test (DW=1.98, no autocorrelation)
- **Multicollinearity:** VIF values (all < 2.0, no redundancy)

### Why Classical Statistics?
We chose classical statistics over machine learning for three reasons:
1. **Interpretability:** Anyone can understand regression coefficients
2. **Theory:** 100+ years of validated statistical foundations
3. **Transparency:** All assumptions can be checked and verified

---

## Running the Analysis

### Requirements
- **R** 4.0 or higher
- **Required packages:** tidyverse, ggplot2, gridExtra, scales, corrplot, FactoMineR, car, lmtest
- **Optional:** Quarto (for rendering Quarto documents)

___

## Key Findings Summary

**Three phenomena from the 2016 study persist in 2024:**
1. ✓ **Genre Matters** — Family-oriented genres (Biography, Documentary, Animation) dominate dual success
2. ✓ **Past Success Predicts Future** — Star power/popularity shows 35 pp gap in success rates
3. ✓ **Collaboration & Complexity** — Multi-genre films rate 0.22 points higher than single-genre

**New discoveries:**
- Documentary films surged 9.3 percentage points in importance (2017-2024)
- Budget has minimal correlation with ratings (r=0.14)
- Horror films systematically penalized (-0.97 rating penalty)

---

## Authors & Contact

**Project Team:**
- Abhishek Chavan (achavan@ucdavis.edu)
- Navya Gulati (ngulati@ucdavis.edu)

**Course:** STAT 204 - Introduction to Statistical Data Analysis  
**Institution:** UC Davis  
**Date:** December 2024

---

## Citation

If you use this analysis in your work, please cite:

```bibtex
@article{chavan2024movies,
  title={What Makes a Successful Movie? Reproducing and Extending Gao et al. (2019) with 2024 Data},
  author={Chavan, Abhishek and Gulati, Navya},
  journal={STAT 204 Final Project, UC Davis},
  year={2024}
}
```


