# BRFSS 2022 Data Structure Documentation

## Dataset Overview

- **Source**: Behavioral Risk Factor Surveillance System (BRFSS) 2022
- **Observations**: 445,132 respondents
- **Variables**: 328 total variables
- **States/Territories**: 54 (all US states, DC, and territories)
- **File Size**: 1.1 GB (XPT format)
- **Memory Size**: 1,115.7 MB when loaded in R

## Survey Design Components

### Weighting Variables

**Primary Weight: `_LLCPWT`** (Final Weight)
- Range: 0.02 to 54,390.52
- Missing: 0 (0.00%)
- Purpose: Final raked weight combining design weight and post-stratification adjustments
- Usage: **This is the primary weight to use for all analyses**

**Other Weight Variables** (for reference):
- `_STRWT`: Design weight (0.27 to 1,694.11)
- `_CLLCPWT`: Cell-phone sample weight
- `_LLCPWT2`: Alternative weight
- `_RAWRAKE`: Raking adjustment factor (0.33 to 5.00)
- `_WT2RAKE`: Secondary raking weight

### Stratification Variable

**`_STSTR`** (Stratum Identifier)
- Range: 11,011 to 782,029
- Missing: 0 (0.00%)
- Purpose: Identifies sampling strata (combination of state and design cell)
- Structure: Likely formatted as SSGGG (SS=state FIPS, GGG=stratum within state)

### Clustering Variable

**`_PSU`** (Primary Sampling Unit)
- Range: 2,022,000,001 to 2,022,026,152
- Missing: 0 (0.00%)
- Purpose: Identifies PSUs (phone numbers or geographic areas) within strata
- Structure: Unique identifier for each PSU nested within strata
- Note: BRFSS uses a two-stage cluster design

## Survey Design Setup

```r
# Recommended survey design specification
library(survey)

design <- svydesign(
  ids = ~`_PSU`,           # Primary sampling units
  strata = ~`_STSTR`,      # Stratification variable
  weights = ~`_LLCPWT`,    # Final weight
  data = brfss_data,
  nest = TRUE              # PSUs are nested within strata
)
```

## Key Social Epidemiology Variables

### Socioeconomic Status

#### Income: `INCOME3`
- **Description**: Annual household income from all sources
- **Categories**:
  - 1 = Less than $10,000
  - 2 = $10,000 to < $15,000
  - 3 = $15,000 to < $20,000
  - 4 = $20,000 to < $25,000
  - 5 = $25,000 to < $35,000
  - 6 = $35,000 to < $50,000
  - 7 = $50,000 to < $75,000
  - 8 = $75,000 to < $100,000
  - 9 = $100,000 to < $150,000
  - 10 = $150,000 to < $200,000
  - 11 = $200,000 or more
  - 77 = Don't know/Not sure
  - 99 = Refused
- **Distribution**:
  - Valid responses: 408,227 (91.7%)
  - Missing/Refused: 36,905 (8.3%)
  - Modal category: $50,000-$75,000 (n=59,148)

#### Education: `EDUCA`
- **Description**: Highest grade or year of school completed
- Available as computed variable `_EDUCAG` (grouped categories)

#### Employment: `EMPLOY1`
- **Description**: Current employment status

### Race/Ethnicity

#### `_IMPRACE` (Imputed Race/Ethnicity)
- **Description**: Race/ethnicity category (imputed for missing values)
- **Categories**:
  - 1 = White, Non-Hispanic (n=333,514, 74.9%)
  - 2 = Black, Non-Hispanic (n=35,876, 8.1%)
  - 3 = Asian, Non-Hispanic (n=13,487, 3.0%)
  - 4 = American Indian/Alaska Native, Non-Hispanic (n=7,120, 1.6%)
  - 5 = Hispanic (n=42,977, 9.7%)
  - 6 = Other race, Non-Hispanic (n=12,158, 2.7%)
- **Missing**: 0 (imputed)

### Health Outcomes

#### General Health: `GENHLTH`
- **Description**: Self-rated general health status
- **Categories**:
  - 1 = Excellent (n=71,878, 16.2%)
  - 2 = Very good (n=148,444, 33.4%)
  - 3 = Good (n=143,598, 32.3%)
  - 4 = Fair (n=60,273, 13.5%)
  - 5 = Poor (n=19,741, 4.4%)
  - 7 = Don't know/Not sure (n=810, 0.2%)
  - 9 = Refused (n=385, 0.1%)
- **Missing**: 3 (0.0%)

#### Physical Health Days: `PHYSHLTH`
- **Description**: Number of days physical health was not good in past 30 days
- **Range**: 0-30 days

#### Mental Health Days: `MENTHLTH`
- **Description**: Number of days mental health was not good in past 30 days
- **Range**: 0-30 days

#### Diabetes: `DIABETE4`
- **Description**: Ever been told you have diabetes
- **Categories**:
  - 1 = Yes (n=61,158, 13.7%)
  - 2 = Yes, but female only during pregnancy (n=3,836, 0.9%)
  - 3 = No (n=368,722, 82.8%)
  - 4 = No, pre-diabetes or borderline (n=10,329, 2.3%)
  - 7 = Don't know/Not sure (n=763, 0.2%)
  - 9 = Refused (n=321, 0.1%)
- **Missing**: 3 (0.0%)

#### Asthma: `ASTHMA3`
- **Description**: Ever been told you have asthma

### Healthcare Access

#### Health Insurance: `_HLTHPLN`
- **Description**: Have any health insurance coverage

#### Could Not Afford Doctor: `MEDCOST1`
- **Description**: Could not see doctor in past 12 months due to cost

#### Last Checkup: `CHECKUP1`
- **Description**: How long since last routine checkup

### Demographics

#### State: `_STATE`
- **Description**: State FIPS code
- **Range**: 1-78 (includes territories)
- **Top 5 States by Sample Size**:
  1. Washington (53): n=26,152
  2. New York (36): n=17,800
  3. Minnesota (27): n=16,821
  4. Ohio (39): n=16,487
  5. Maryland (24): n=16,418

## Test Dataset

A stratified 2% sample has been created for development and testing:
- **File**: `data/brfss_test_sample.rds`
- **Size**: 8,876 observations
- **Sampling**: Proportional stratification by state
- **Purpose**: Faster development/testing while maintaining design structure

## Social Epidemiology Example Topics

Based on available variables, excellent examples for masters-level students include:

### 1. Health Disparities by Income
**Research Question**: Are diabetes prevalence and general health status associated with household income, adjusting for complex survey design?

**Variables**:
- Outcome: `DIABETE4` (diabetes) or `GENHLTH` (general health)
- Exposure: `INCOME3` (household income)
- Covariates: `_IMPRACE`, `EDUCA`, `_AGEG5YR`, `SEXVAR`

### 2. Healthcare Access and Social Determinants
**Research Question**: How do income and race/ethnicity affect access to healthcare services?

**Variables**:
- Outcomes: `MEDCOST1` (cost barrier), `CHECKUP1` (last checkup)
- Exposures: `INCOME3`, `_IMPRACE`
- Covariates: `_HLTHPLN`, `EMPLOY1`, `_URBSTAT`

### 3. Mental Health Disparities
**Research Question**: Do mental health outcomes vary by socioeconomic position and race/ethnicity?

**Variables**:
- Outcome: `MENTHLTH` (mental health days)
- Exposures: `INCOME3`, `_IMPRACE`, `EDUCA`
- Covariates: `_AGEG5YR`, `SEXVAR`, `EMPLOY1`

## Important Considerations

### Survey Design Complexities

1. **Nested Structure**: PSUs are nested within strata (use `nest=TRUE`)
2. **Large Weights**: Some weights >50,000 indicate rare populations
3. **State Variation**: Sample sizes vary dramatically by state
4. **Raking**: Final weights include post-stratification adjustments

### Missing Data

- Many variables use:
  - 7 = "Don't know/Not sure"
  - 9 = "Refused"
  - 77/99 = Missing/Refused (for numeric variables)
- These should typically be coded as NA for analysis

### Coding Recommendations

```r
# Recode function example for BRFSS
recode_brfss <- function(data) {
  data %>%
    mutate(
      # Diabetes (binary)
      diabetes = case_when(
        DIABETE4 == 1 ~ "Yes",
        DIABETE4 %in% c(2, 4) ~ "Borderline/Pregnancy",
        DIABETE4 == 3 ~ "No",
        TRUE ~ NA_character_
      ),

      # Income (grouped)
      income_cat = case_when(
        INCOME3 %in% 1:4 ~ "< $25,000",
        INCOME3 %in% 5:6 ~ "$25,000-$49,999",
        INCOME3 %in% 7:8 ~ "$50,000-$99,999",
        INCOME3 %in% 9:11 ~ "$100,000+",
        TRUE ~ NA_character_
      ),

      # General health (ordered)
      genhlth_cat = case_when(
        GENHLTH %in% 1:2 ~ "Excellent/Very Good",
        GENHLTH == 3 ~ "Good",
        GENHLTH %in% 4:5 ~ "Fair/Poor",
        TRUE ~ NA_character_
      )
    )
}
```

## References

- BRFSS Overview: https://www.cdc.gov/brfss/
- 2022 Survey Documentation: https://www.cdc.gov/brfss/annual_data/2022/pdf/overview-2022-508.pdf
- Codebook: https://www.cdc.gov/brfss/annual_data/2022/pdf/codebook22_llcp-v2-508.pdf
- Complex Sample Analysis Guide: https://www.cdc.gov/brfss/annual_data/2022/pdf/Complex-Smple-Weights-Prep-Module-Data-Analysis-2022-508.pdf
