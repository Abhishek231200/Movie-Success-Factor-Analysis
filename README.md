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
source("src/movie_analysis_qmd.qmd")
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

- **Total films analyzed:** 3,783 movies (1996-2024) with complete data
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

## Project Structure

```
movie_success_analysis/
├── README.md                              # This file
├── src/
│   ├── movie_analysis_qmd.qmd            # Main analysis script (Quarto/R Markdown)
│   ├── data/
│   │   ├── raw/                          # Place downloaded IMDb files here
│   │   │   ├── name.basics.tsv.gz
│   │   │   ├── title.basics.tsv.gz
│   │   │   ├── title.principals.tsv.gz
│   │   │   └── title.ratings.tsv.gz
│   │   └── processed/
│   │       └── movies_analysis.csv       # Cleaned data with features
│   └── LICENSE
├── results/
│   ├── preliminary/
│   │   └── exploratory_stats.txt
│   └── final/
│       ├── regression_coefficients.csv
│       ├── model_diagnostics.txt
│       └── session_info.txt
└── figures/
    ├── 01_rating_distribution.png        # Histogram of IMDb ratings
    ├── 02_revenue_distribution.png       # Log-scale revenue distribution
    ├── 03_success_by_genre.png           # Dual success by genre
    ├── 04_rating_vs_revenue.png          # Scatter plot with LOESS trend
    ├── 05_temporal_trends.png            # Trends 1996-2016 vs 2017-2024
    ├── 06_regression_coefficients.png    # Model coefficient plot
    └── 07_correlation_heatmap.png        # Correlation matrix
```

---

## Running the Analysis

### Requirements
- **R** 4.0 or higher
- **Required packages:** tidyverse, ggplot2, gridExtra, scales, corrplot, FactoMineR, car, lmtest
- **Optional:** Quarto (for rendering Quarto documents)

### Installation

```R
# Install required packages
packages <- c("tidyverse", "ggplot2", "gridExtra", "scales", 
              "corrplot", "FactoMineR", "car", "lmtest")
install.packages(packages)
```

### Execution

```R
# Option 1: Source the main script directly
source("src/movie_analysis_qmd.qmd")

# Option 2: Render as Quarto document (if using Quarto)
quarto::quarto_render("src/movie_analysis_qmd.qmd")
```

**Expected output:**
- 7 publication-ready figures saved to `figures/`
- Processed data saved to `data/processed/movies_analysis.csv`
- Statistical results saved to `results/final/`
- **Runtime:** 5-10 minutes on modern hardware

---

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

## Limitations

1. **Budget Estimation:** We estimated budgets as 30% of revenue due to missing data. This is systematic but introduces measurement error.

2. **Selection Bias:** Box Office Mojo includes only theatrical releases. Streaming-first films (Netflix, Disney+) and independent films are underrepresented.

3. **U.S.-Centric:** IMDb and Box Office data dominated by U.S./Hollywood market. Findings may not generalize to international cinema (Bollywood, Chinese, European).

4. **Causality:** Our models identify predictors, not causal mechanisms. Does Drama cause ratings, or do great writers choose drama?

5. **Unmeasured Variables:** Marketing spend, critic reviews, awards buzz, release timing, and cultural moments all influence success but aren't in our dataset.

6. **Temporal Confounding:** The 2017-2024 period includes COVID-19 pandemic disruption. Future work should isolate pandemic effects.

---

## Future Research Directions

1. **Sentiment Analysis:** Apply NLP to user reviews for thematic content beyond genre
2. **Franchise Effects:** Isolate sequels and shared universes (MCU, DCEU) separately
3. **Streaming vs. Theatrical:** Compare success factors for streaming-first films
4. **International Comparison:** Replicate for Bollywood (Douban) and Chinese cinema
5. **Pandemic Deep Dive:** Separately analyze 2020-2021 disruption period
6. **Predictive Modeling:** Use pre-release features (cast, director, budget, marketing)

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

---

## References

1. Gao, Z., Malic, V., Ma, S., & Shih, P. (2019). How to Make a Successful Movie: Factor Analysis from Both Financial and Critical Perspectives. *Proceedings of iConference 2019*, LNCS 11420, 669-678.

2. Box Office Mojo. (2024). Annual Box Office Reports. Retrieved from https://www.boxofficemojo.com/year/

3. IMDb. (2024). IMDb Datasets. Retrieved from https://datasets.imdbws.com/

4. The Movie Database (TMDb). (2024). API Documentation. Retrieved from https://www.themoviedb.org/settings/api

---

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

**Last Updated:** December 9, 2024  
**Status:** Complete and ready for publication
