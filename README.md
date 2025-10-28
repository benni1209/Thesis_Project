# Statistical Matching using a Non-Probability Sample as Auxiliary Dataset

This repository contains the complete analysis code and implementation scripts that were used in the thesis project.

## Overview

This project examines methods for estimating joint distributions of two categorical variables (Y and Z) when complete observations are unavailable and data is distributed across multiple samples:
- **Sample A**: Contains variables X and Y
- **Sample B**: Contains variables X and Z  
- **Sample C**: Contains all three variables (X, Y, Z) but with potential selection bias

The research evaluates various statistical methods under both Missing At Random (MAR) and Missing Not At Random (MNAR) scenarios, using bootstrap simulations with 1,000 iterations per scenario.

## Research Context

The study addresses the challenge of estimating the joint distribution P(Y,Z) when:
1. Direct observations of (Y,Z) pairs are limited or biased (Sample C)
2. Marginal information is available from separate samples (A and B)
3. An auxiliary variable X is available across all samples for calibration

This situation commonly arises in practice when combining data from different surveys or when privacy constraints prevent collecting all variables from the same respondents.

## Repository Structure

### Implementation Scripts
These files contain the core functions for data generation, sampling, and calibration:

- **`MAR_Implementation.Rmd`**: Implementation of Missing At Random mechanisms with multiple selection probability patterns (linear decreasing, U-shaped, step function, extreme bias)
- **`MNAR_Implementation.Rmd`**: Implementation of Missing Not At Random mechanisms including interaction effects between Y and Z
- **`YZ-Association_Implementation.Rmd`**: Implementation combining various association structures between Y and Z with different missing data mechanisms
- **`Calibration_Methods.Rmd`**: Calibration procedures using Iterative Proportional Fitting (IPF) for both univariate (X) and multivariate (X,Y) and (X,Z) calibration
- **`EM_Algorithm.Rmd`**: Expectation-Maximization algorithm for imputing the joint distribution of multinomial data

### Analysis Scripts
These files contain the bootstrap analysis and performance evaluation:

- **`MAR_Analysis.Rmd`**: Comprehensive analysis of method performance under MAR scenarios
- **`MNAR_Analysis.Rmd`**: Analysis of method performance under MNAR scenarios  
- **`YZ-Association_Analysis.Rmd`**: Analysis examining how the strength of Y-Z association affects method performance
- **`Association_Measure_Cramer_s_V_.Rmd`**: Implementation and analysis of Cramér's V as a measure of association strength
- **`Quantifying_Interaction.Rmd`**: Quantification of interaction effects in selection probabilities

## Methodology

### Data Generation
The implementation creates synthetic populations with:
- **Variable X**: Categorical auxiliary variable (6 categories)
- **Variables Y and Z**: Outcome variables of interest (3 categories each)
- Controllable association structures between all variable pairs

### Missing Data Mechanisms

#### MAR (Missing At Random)
Selection into Sample C depends on observed variables (X, Y, or Z) but not on the missing values:
- Linear decreasing patterns (moderate and extreme)
- U-shaped selection patterns
- Step function patterns
- Extreme bias scenarios

#### MNAR (Missing Not At Random)  
Selection into Sample C depends on both Y and Z simultaneously, including interaction effects:
- Pure main effects (classic, non-monotonic, Y-only)
- Weak, moderate, and strong interaction effects
- Extreme interaction scenarios

### Statistical Methods

The analysis evaluates several estimation approaches:

1. **Raw Sample C**: Unadjusted biased sample
2. **X-Calibration**: Calibrated to population X distribution
3. **Full Calibration**: Calibrated to X, (X,Y), and (X,Z) using IPF
4. **EM Algorithm**: Applied to raw and X-calibrated Sample C

### Performance Metrics

Methods are evaluated using:
- **Total Absolute Difference (TAD)**: Sum of absolute cell-wise errors
- **Root Mean Squared Error (RMSE)**: Overall distribution accuracy
- **Maximum Absolute Error**: Worst-case cell error
- **Cramér's V**: Association strength in estimated distributions
- **Worst Cell Bias**: Signed error for the most problematic cell

## Requirements

### R Version
- R 4.0.0 or higher recommended

### Required R Packages
```r
# Data manipulation and visualization
library(dplyr)
library(tidyr)
library(ggplot2)
library(gridExtra)
library(reshape2)

# Table formatting
library(knitr)
library(kableExtra)

# Sampling
library(sampling)
```

## Usage

### Running Implementation Scripts

1. **Generate data with MAR mechanism:**
```r
rmarkdown::render("MAR_Implementation.Rmd")
```

2. **Generate data with MNAR mechanism:**
```r
rmarkdown::render("MNAR_Implementation.Rmd")
```

3. **Apply calibration methods:**
```r
rmarkdown::render("Calibration_Methods.Rmd")
```

4. **Run EM algorithm:**
```r
rmarkdown::render("EM_Algorithm.Rmd")
```

### Running Analysis Scripts

The analysis scripts expect bootstrap results saved as RDS files:
- `bootstrap_results_MAR_scenarios(1000).rds`
- `bootstrap_results_MNAR_scenarios(1000).rds`  
- `bootstrap_results_association_mar_mnar_(1000).rds`

Run the analysis:
```r
rmarkdown::render("MAR_Analysis.Rmd")
rmarkdown::render("MNAR_Analysis.Rmd")
rmarkdown::render("YZ-Association_Analysis.Rmd")
```

### Customization

Key parameters can be modified in the implementation scripts:
- `N_pop`: Population size (default: 1,000,000)
- `n_bootstrap`: Number of bootstrap iterations (default: 1,000)
- `base_prob`: Base selection probability for Sample C
- Sample sizes for A, B, and C
- Selection probability patterns
- Association structures

## Key Features

- **Modular Design**: Separate implementation and analysis scripts for clarity and reproducibility
- **Comprehensive Scenarios**: Multiple MAR and MNAR patterns covering various real-world situations
- **Bootstrap Validation**: 1,000 iterations per scenario for robust performance assessment
- **Multiple Methods**: Comparison of naive, calibration, and EM-based approaches
- **Rich Diagnostics**: Extensive performance metrics and visualizations

## Output

Each analysis script generates:
- PDF report with tables and visualizations
- Summary statistics across bootstrap iterations
- Method comparison plots
- Bias and variance analysis
- Scenario-specific insights

## Author

Benedikt Sojka (s4089448)

