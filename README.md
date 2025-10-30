# Descriptive Statistics with Complex Survey Weights: An Example for Social Epidemiology

A comprehensive tutorial demonstrating how to analyze complex survey data using R, focusing on the Behavioral Risk Factor Surveillance System (BRFSS) 2022 dataset. This repository provides practical examples of survey-weighted analyses for social epidemiology research using the full BRFSS dataset.

**IMPORTANT**: Download the BRFSS 2022 dataset (≈1.1 GB) from the CDC website: https://www.cdc.gov/brfss/annual_data/annual_2022.html and place the `LLCP2022.XPT` file in the `data/` directory.

## Overview

This repository contains a complete worked example analyzing health disparities by income using BRFSS 2022 data. It demonstrates proper handling of complex survey design features including sampling weights, stratification, and clustering.

### Research Question

**Are diabetes prevalence and general health status associated with household income in the US adult population?**

This fundamental social epidemiology question examines socioeconomic health disparities using nationally representative data.

## Key Features

- **Full BRFSS Dataset Analysis**: Complete demonstration using all 445,132 respondents
- **Proper Survey Design**: Demonstrates how to correctly specify weights, strata, and clusters
- **Design-Adjusted Statistics**: Uses Rao-Scott adjusted chi-square tests and survey-weighted proportions
- **Design Effects**: Calculates and interprets design effects (DEFF) and effective sample sizes
- **Publication-Ready Tables**: Creates formatted tables using `gtsummary` with survey weights
- **Complete Workflow**: From data loading through analysis to interpretation and reporting
- **Comparison with Naive Methods**: Explicitly shows why ignoring survey design gives wrong results

## Learning Objectives

By working through this tutorial, you will learn how to:

1. Load and prepare complex survey data (BRFSS)
2. Create a proper survey design object accounting for weights, strata, and clusters
3. Recode variables for social epidemiology analyses
4. Generate descriptive statistics that account for survey design
5. Conduct bivariate analyses with design-adjusted p-values
6. Interpret design effects and effective sample sizes
7. Create publication-ready tables
8. Understand why ignoring survey design leads to incorrect conclusions

## Requirements

### R Packages

```r
install.packages(c(
  "haven",       # For reading SAS/SPSS/Stata files
  "dplyr",       # Data manipulation
  "tidyr",       # Data tidying
  "survey",      # Complex survey analysis
  "gtsummary"    # Publication-ready tables
))
```

### R Version

R version 4.0.0 or higher recommended.

## Repository Structure

```
.
├── README.md                           # This file
├── example_social_epi_analysis.qmd     # Main tutorial (Quarto markdown)
├── example_social_epi_analysis.html    # Rendered HTML output
├── data/
│   ├── LLCP2022.XPT                   # Full BRFSS 2022 dataset (download from CDC)
│   └── data_docs/
│       └── 2022-Weighting-Description-508.pdf  # CDC weighting documentation
└── extract_results.R                   # Helper script for extracting results
```

## Getting Started

### Quick Start

1. **Clone this repository**
   ```bash
   git clone https://github.com/yourusername/survey-weighted-descriptive-stats.git
   ```

2. **Download the BRFSS 2022 dataset**
   - Visit: https://www.cdc.gov/brfss/annual_data/annual_2022.html
   - Download `LLCP2022.XPT` (≈1.1 GB, 445,132 observations)
   - Place it in the `data/` directory

3. **Install required R packages**
   ```r
   install.packages(c("haven", "dplyr", "tidyr", "survey", "gtsummary"))
   ```

4. **Open and run the tutorial**
   - Open `example_social_epi_analysis.qmd` in RStudio
   - Render the document (Ctrl/Cmd + Shift + K) or run code chunks interactively
   - Note: Initial data load takes 1-2 minutes

**Note:** The tutorial uses the full BRFSS dataset to demonstrate proper survey-weighted analysis methods with all survey design features preserved.

## Key Concepts Demonstrated

### 1. Complex Survey Design

BRFSS uses a complex, multistage sampling design:

- **Stratification**: States divided into strata to ensure representation
- **Clustering**: Phone numbers/geographic areas sampled (reduces costs, increases variance)
- **Weighting**: Adjusts for selection probability and non-response

### 2. Why Survey Design Matters

Ignoring the survey design leads to:

- Incorrect standard errors (usually too small)
- Invalid p-values (often falsely significant)
- Biased estimates if weights are not used

### 3. Design Effects

The tutorial demonstrates that design effects (DEFF) for BRFSS estimates can be substantial:

- Diabetes prevalence: DEFF ≈ 4.0 (effective sample size reduced by ~75%)
- This means standard errors are ~2× larger than simple random sampling
- Statistical power is substantially reduced

## Example Results

The analysis reveals strong socioeconomic health disparities:

- **Diabetes prevalence**: 18% among those earning <$25,000 vs 8% among those earning ≥$100,000
- **Fair/Poor health**: 37% in lowest income group vs 9% in highest income group
- All associations are statistically significant using design-adjusted tests

## Statistical Methods

### Tests Used

- **Rao-Scott Adjusted Chi-Square**: Design-adjusted test for associations between categorical variables
- **Survey-Weighted Proportions**: Properly weighted estimates accounting for sampling design
- **Design Effect Calculations**: Quantifies impact of complex design on variance

### Comparison with Naive Analysis

The tutorial explicitly compares:

- Naive analysis (ignoring survey design) → WRONG
- Survey-weighted analysis (accounting for design) → CORRECT

This demonstrates why proper survey methods are essential.

## Resources

### R Packages

- [survey package](https://r-survey.r-forge.r-project.org/survey/) - Complex survey analysis
- [gtsummary](https://www.danieldsjoberg.com/gtsummary/) - Publication-ready tables

### References

- [BRFSS Overview](https://www.cdc.gov/brfss/) - CDC's official BRFSS page
- [BRFSS 2022 Weighting Documentation](data/data_docs/2022-Weighting-Description-508.pdf)


## License

This project is open source and available for educational purposes.

## Acknowledgments

- CDC for providing BRFSS data
- Thomas Lumley for the `survey` package
- Daniel Sjoberg for the `gtsummary` package
