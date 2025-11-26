# Master's Thesis: The Effect of Medicaid Expansion on Smoking Behavior

This repository contains the analysis code for a master's thesis examining the causal impact of Medicaid expansion on smoking behavior using data from the Behavioral Risk Factor Surveillance System (BRFSS).

## Research Question

Does Medicaid expansion affect smoking rates among the low-income population? This analysis uses staggered difference-in-differences methodology to estimate the causal effect of state-level Medicaid expansion on individual smoking behavior.

## Repository Structure

```
.
├── data_analysis.ipynb    # Exploratory data analysis and data preparation
├── findings.ipynb         # Main econometric analysis and results
└── README.md             # This file
```

## Files Description

### `data_analysis.ipynb`

This notebook handles the initial data processing and exploratory analysis:

- **Data Loading**: Reads BRFSS survey data from SAS XPT files (2009-2023) into SQLite database
- **Variable Construction**: Creates unified tobacco use variables including:
  - Current smoking status
  - E-cigarette usage indicators
  - Combined tobacco product usage
- **Data Cleaning**: Handles missing values, survey weights, and demographic variables
- **Exploratory Visualizations**: 
  - Temporal trends in smoking rates
  - State-level variation in tobacco use
  - Demographic patterns (age, income, insurance status)
  - Treatment timing variation across states

**Key Variables**:
- Smoking indicators: `SMOKDAY2`, `USENOW3`
- E-cigarette use: `ECIGNOW`, `ECIGNOW1`, `ECIGNOW2`, `CIGARUSE`
- Demographics: `_AGEG5YR`, `INCOME2`, `INCOME3`, `SEXVAR`
- Health insurance: `PRIMINS1`
- Survey weights: `_LLCPWT`, `_FINALWT`

### `findings.ipynb`

This notebook contains the main econometric analysis:

- **Estimation Method**: Callaway & Sant'Anna (2021) difference-in-differences with staggered treatment adoption
- **Treatment Definition**: State Medicaid expansion timing
- **Outcome Variables**: 
  - Any smoking behavior
  - Daily smoking
  - E-cigarette use
- **Subgroup Analysis**:
  - By income level
  - By demographic characteristics
  - By political leaning of states (Republican vs. Democratic)
- **Robustness Checks**:
  - Alternative treatment definitions
  - Sensitivity to parallel trends assumption
  - Event study plots
- **Instrumental Variables**: Uses governor party affiliation as instrument for Medicaid expansion
- **Visualization**: Event-study plots and aggregated treatment effect estimates

**Key Methods**:
- `did2s` package for two-stage difference-in-differences
- `fixest` for fixed effects regression
- Weighted regression using BRFSS survey weights
- Pre-treatment parallel trends testing

## Data Requirements

### Primary Dataset
- **BRFSS (Behavioral Risk Factor Surveillance System)**: Annual cross-sectional surveys from 2009-2023
  - Source: CDC ([https://www.cdc.gov/brfss](https://www.cdc.gov/brfss))
  - Format: SAS XPT files (one per year)
  - Required variables: See `all_vars` list in `data_analysis.ipynb`

### Auxiliary Data
- **State Medicaid Expansion Timing**: Mapping of states to Medicaid expansion dates
- **State Cigarette Tax Rates**: Annual state-level cigarette excise taxes (2009-2023)
- **Governor Party Affiliation**: Used as instrumental variable

### Expected Input Files
The analysis expects a preprocessed CSV file:
- `df_individual_non_ecig_did.csv`: Individual-level BRFSS data with treatment indicators

## Dependencies

### Python Packages
```
pandas
numpy
matplotlib
seaborn
plotly
sqlite3
scipy
statsmodels
```

### R Packages (via rpy2)
```
did
fixest
ggplot2
dplyr
```

Install Python dependencies:
```bash
pip install pandas numpy matplotlib seaborn plotly scipy statsmodels rpy2
```

Install R packages:
```R
install.packages(c("did", "fixest", "ggplot2", "dplyr"))
```

## Usage

### 1. Data Preparation
Run `data_analysis.ipynb` to:
- Load raw BRFSS XPT files into SQLite
- Clean and merge yearly data
- Create analysis variables
- Generate exploratory visualizations

```bash
jupyter notebook data_analysis.ipynb
```

### 2. Main Analysis
Run `findings.ipynb` to:
- Estimate treatment effects using Callaway & Sant'Anna methodology
- Conduct subgroup analyses
- Generate event-study plots
- Perform robustness checks

```bash
jupyter notebook findings.ipynb
```

## Methodological Notes

### Identification Strategy
The analysis exploits the staggered rollout of Medicaid expansion across U.S. states following the Affordable Care Act. The identification assumption is that, absent Medicaid expansion, smoking trends would have evolved similarly across treatment and control states (parallel trends).

### Survey Weights
All analyses properly account for BRFSS complex survey design using:
- Individual-level weights (`_LLCPWT` or `_FINALWT`)
- Weighted means and regression estimates
- Robust standard errors clustered at state level

### Time Period
- **Pre-treatment period**: 2009-2013 (pre-ACA Medicaid expansion)
- **Treatment period**: 2014-2023 (staggered expansion adoption)
- **Never-treated states**: Used as comparison group

### Outcome Definition
Multiple smoking outcomes examined:
1. Any current smoking (includes daily and non-daily smokers)
2. Daily smoking only
3. E-cigarette use (in later years)

## Key Results

Results are presented in `findings.ipynb` including:
- Aggregate treatment effect on the treated (ATT)
- Dynamic treatment effects by time since expansion
- Heterogeneous effects by subgroup
- Event-study plots showing parallel trends and post-treatment effects

## Citation

If you use this code or methodology, please cite:

```
[Author Name]. [Year]. "The Effect of Medicaid Expansion on Smoking Behavior." 
Master's Thesis, [University Name].
```

## References

- Callaway, B., & Sant'Anna, P. H. (2021). Difference-in-differences with multiple time periods. *Journal of Econometrics*, 225(2), 200-230.
- CDC. Behavioral Risk Factor Surveillance System. [https://www.cdc.gov/brfss](https://www.cdc.gov/brfss)

## License

This code is provided for research and educational purposes.

## Contact

For questions or issues, please contact [your contact information].

---

*Last updated: November 2025*
