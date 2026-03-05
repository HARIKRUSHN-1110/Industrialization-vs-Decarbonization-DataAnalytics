# Processed Data Folder

## Overview
This folder contains the cleaned and merged panel dataset ready for econometric analysis.

## Files

### `panel_dataset.csv`
Main analysis dataset with all variables merged.

**Structure:**
- **Rows:** Country-year observations (6 countries × 15 years = 90 obs)
- **Format:** Long panel format
- **Key ID variables:** `country_code`, `country`, `year`

### `panel_dataset.xlsx`
Excel version with multiple sheets:
- **Panel Data:** Full dataset
- **Country Summary:** Statistics by country
- **Year Summary:** Statistics by year

### `variable_codebook.csv`
Complete list of all variables with data types and sample values.

## Key Variable Groups

### 1. Economic Output
- `gva_industry_meur`: Industrial GVA (million EUR, 2015 prices)
- `gva_manufacturing_meur`: Manufacturing GVA
- `gva_total_meur`: Total economy GVA
- `industry_share_pct`: Industry share of total GVA
- `gva_industry_real_meur`: Inflation-adjusted industrial GVA

### 2. Carbon Pricing
- `carbon_price_eur_tonne`: EU ETS price (EUR per tonne CO2)
- `ets_phase`: ETS phase (2, 3, or 4)
- `msr_active`: Market Stability Reserve active (0/1)
- `high_price_regime`: High price period indicator (0/1)

### 3. Emissions
- `total_ghg_mt_co2eq`: Total GHG emissions (million tonnes CO2 equivalent)
- `verified_emissions_mt`: ETS verified emissions
- `emissions_yoy_pct`: Year-over-year emissions change
- `emissions_reduction_from_2010_pct`: Cumulative reduction since 2010

### 4. ETS Compliance
- `free_allocation_ratio_pct`: Free allowances as % of verified emissions
- `net_purchase_required_mt`: Net allowances that must be purchased
- `effective_carbon_cost_meur`: Actual carbon cost paid (million EUR)
- `carbon_cost_burden_pct`: Carbon cost as % of industrial GVA

### 5. Carbon Intensity
- `carbon_intensity_t_per_meur`: Emissions per million EUR of industrial output
- `carbon_intensity_gco2_kwh`: Power sector carbon intensity (g CO2/kWh)

### 6. Control Variables
- `ppi_index_2015`: Producer Price Index (base 2015=100)
- `ppi_inflation_pct`: PPI inflation rate
- `covid_period`: COVID-19 period indicator (2020-2021)
- `energy_crisis`: Energy crisis period indicator (2022-2023)

### 7. Growth & Performance
- `industry_yoy_growth_pct`: Industrial output YoY growth
- `gdp_growth_pct`: Total economy YoY growth
- `decoupling_index`: GDP growth minus emissions growth

### 8. Regression Variables
- `ln_gva_industry`: Log of industrial GVA
- `ln_carbon_price`: Log of carbon price
- `ln_emissions`: Log of emissions
- `carbon_price_lag1`, `carbon_price_lag2`: Lagged carbon prices

## Countries Included
- **DE:** Germany
- **FR:** France
- **IT:** Italy
- **PL:** Poland
- **ES:** Spain
- **NL:** Netherlands

## Time Coverage
- **Start:** 2010
- **End:** 2024
- **Frequency:** Annual

## Data Sources
1. **Eurostat** - GVA, PPI
2. **ICE Futures/Ember** - Carbon prices
3. **EDGAR v8.0** - GHG emissions
4. **EU Transaction Log (EUTL)** - ETS compliance
5. **Ember Climate** - Power sector data

## Processing Steps
All processing is documented in:
`notebooks/data_collection_processing/00_Data_Processing_Complete.ipynb`

## Usage Example

```python
import pandas as pd

# Load panel dataset
df = pd.read_csv('data/processed/panel_dataset.csv')

# Filter for specific analysis
germany = df[df['country_code'] == 'DE']
high_price_period = df[df['high_price_regime'] == 1]

# Regression variables
y = df['ln_gva_industry']
x = df[['ln_carbon_price', 'ln_emissions', 'ppi_index_2015']]
```

## Data Quality Notes
- All monetary values in constant 2015 EUR
- Missing values occur primarily in derived variables for early years (due to lagging)
- COVID period (2020-2021) shows unusual patterns in all economic variables
- 2024 data may be preliminary pending final official releases

## Next Steps
1. Exploratory Data Analysis (EDA)
2. Fixed Effects Panel Regression
3. Hypothesis Testing
