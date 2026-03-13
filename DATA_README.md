# Data Directory

This directory contains all datasets used in the malaria outbreak prediction research.

## Directory Structure

```
data/
├── raw/              # Original, immutable data from sources
├── processed/        # Cleaned and preprocessed data ready for modeling
└── README.md         # This file
```

## Data Sources

### Primary: Uganda National Health Observatory
- **URL:** https://nho.health.go.ug/themes/Malaria
- **Type:** Time-series malaria surveillance data
- **Format:** CSV
- **Download Date:** [Record when you downloaded]
- **Time Period:** [e.g., 2015-2024]
- **Geographic Coverage:** Uganda (district/regional level)

### Key Variables
Document the main columns/features in your dataset:

**Example:**
- `date`: Date of observation (YYYY-MM-DD)
- `district`: Geographic location
- `confirmed_cases`: Number of confirmed malaria cases
- `tested`: Number of people tested
- `positivity_rate`: Percentage of positive tests
- `population`: District population
- etc.

## Data Processing Pipeline

### Step 1: Raw Data Download
```bash
# Script to download data
python src/data_processing.py --download
```

Location: `data/raw/uganda_malaria_YYYY-MM-DD.csv`

### Step 2: Data Cleaning
- Handle missing values
- Remove duplicates
- Standardize date formats
- Fix data entry errors

Location: `data/processed/malaria_cleaned.csv`

### Step 3: Feature Engineering
- Create lagged features (previous weeks/months)
- Calculate moving averages
- Add seasonal indicators
- Encode categorical variables

Location: `data/processed/malaria_features.csv`

### Step 4: Train/Test Split
- Training set: [date range]
- Testing set: [date range]
- Validation strategy: Time-series cross-validation

## Data Ethics & Privacy

- ✓ All data is **publicly available** and **aggregated**
- ✓ No individual patient information
- ✓ No ethics approval required (public aggregate data)
- Data used for **research purposes only**

## File Naming Convention

Use consistent naming:
- Raw data: `uganda_malaria_raw_YYYY-MM-DD.csv`
- Processed data: `malaria_cleaned_YYYY-MM-DD.csv`
- Features: `malaria_features_YYYY-MM-DD.csv`

## Data Quality Notes

Document any issues or observations:

**Missing Data:**
- [District X] has no data for [date range]
- Testing rate data unavailable before 2018

**Anomalies:**
- Spike in cases in [date] due to [reporting change/outbreak]

**Data Limitations:**
- Geographic granularity varies over time
- Some districts changed boundaries in [year]

## Size Information

- Raw data: ~[X MB/GB]
- Processed data: ~[X MB/GB]
- Total: ~[X MB/GB]

**Note:** Large datasets are excluded from git (see `.gitignore`). Download instructions are in the main README.

## Last Updated

[Date] - [Brief description of what changed]
