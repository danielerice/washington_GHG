# Washington State GHG Emissions: Who Bears the Burden?

**AD450 Final Project**  
**Team Members:** Daniel Rice, Daniel Merced, Phakphoom, Veer

## Project Overview

This group project analyzes industrial greenhouse gas emissions across Washington State counties. Our goal was to move beyond raw emissions totals and ask a more meaningful question: **who actually bears the emissions burden?** We compared total county emissions, per-capita emissions, income, poverty, and sector-level emissions to understand how industrial pollution is distributed across the state. Our central finding is that the burden is not evenly shared. Smaller industrial counties often face a much heavier per-capita emissions load than larger and wealthier urban counties.

## Thesis

The burden of industrial greenhouse gas emissions falls unevenly across Washington State. Rural, lower-income counties bear a disproportionate per-capita emissions load compared to wealthier, more populated urban counties.

## Guiding Questions

1. Which counties emit the most greenhouse gases per person, and how does that differ from total emissions?
2. How have emissions changed over time across different sectors?
3. Is there a relationship between a county’s median income and its per-capita emissions burden?
4. Which industrial sectors dominate emissions in the counties that bear the heaviest per-capita burden?

## Datasets

We combined three datasets for this project:

1. **Washington GHG Reporting Program**  
   Facility-level greenhouse gas emissions reported to Washington State from 2012–2023.

2. **U.S. Census County Population Estimates**  
   Annual county population estimates for Washington counties from 2012–2023.

3. **SAIPE Income & Poverty Estimates**  
   County-level median household income and poverty estimates, using a 2024 snapshot.

## Data Cleaning and Preparation

Before analysis, all three datasets were cleaned and standardized.

### Greenhouse gas dataset
- Confirmed and corrected data types where needed
- Parsed the `Location` field into separate latitude and longitude values
- Converted `Primary NAICS Code` into a clean nullable integer type
- Filled missing gas-breakdown columns with `0` when total reported emissions existed
- Filled missing `Jurisdiction` values with `"Unknown"`
- Removed rows tied to invalid or out-of-state counties
- Removed `Supplier` sector rows because they do not represent physical emitters in a county
- Added a derived `Non_CO2_Emissions` column from non-CO2 greenhouse gas fields 

### Population dataset
- Renamed the first county column to `County`
- Removed the Washington statewide total row
- Standardized county names by stripping prefixes and suffixes
- Selected the needed year ranges from two Census files
- Melted both files from wide to long format
- Concatenated them into one `population_df` covering 2012–2023, with 468 rows total 

### Income dataset
- Filtered to Washington counties only
- Removed non-county or out-of-scope rows
- Standardized county names to match other tables
- Kept only the columns needed for income and poverty analysis

## Data Joining

We used multiple joining methods covered in class:

- **Concatenation:** combined two population files into one long county-year population table
- **Merge on columns:** joined emissions with population on `County + Year`
- **Merge on columns:** joined income onto the emissions-population table on `County`
- **Join on index:** created county-level summary tables and joined them on county index to compute per-capita emissions

## Techniques Used

This project was designed to satisfy the AD450 final project requirements by using techniques from each required category.

### Exploratory Data Analysis
- Data summaries with `.info()`, `.shape`, `.head()`
- Descriptive statistics with `.describe()`
- Value counts for categorical fields such as sector, county, and city
- Histograms of reported emissions and gas-specific columns
- Exploration of skew, missingness, and unusual categorical values

### Data Cleaning and Transformation
- Filled missing values
- Corrected dtype issues
- Removed inaccurate and out-of-scope data
- Standardized county name formatting
- Added derivative columns such as `Non_CO2_Emissions` 

### Data Joining
- Concatenated Census population files
- Merged dataframes on shared columns
- Joined aggregated dataframes on index 

### Aggregation and Grouping
- Grouped emissions by county, year, and sector
- Aggregated totals and averages
- Built pivot tables for county-year emissions
- Used cross-tabulation to examine sector presence by county

### Data Visualization
- Ranked bar charts for total vs per-capita emissions
- Time-series line charts of emissions by sector
- Scatterplots relating income and per-capita emissions
- Stacked bar charts for sector breakdowns in highest-burden counties
- Customized visuals with clear titles, labels, and interpretation for presentation use 

## Key Findings

- The counties with the highest **total** emissions are not always the counties with the highest **per-capita** emissions.
- Per-capita analysis reveals that emissions burdens are concentrated in smaller industrial counties.
- Income showed only a weak relationship with per-capita emissions burden.
- A small number of industries, especially power plants, pulp and paper, and natural gas systems, drive much of the burden in the hardest-hit counties. 

## Repository Structure

```text
.
├── README.md
├── data/
│   └── raw/
│       ├── co-est2020int-pop-53.xlsx
│       ├── co-est2024-pop-53.xlsx
│       ├── est24all.xls
│       └── GHG_Reporting_Program_Publication.csv
├── q1_total_vs_percapita.png
├── data_exploration.ipynb
└── sector_breakdown_of_emissions_top5_percapita.png
````

Adjust the folder names above to match your actual repo.

## How to Run

1. Clone the repository.
2. Install dependencies:

   ```bash
   pip install pandas numpy matplotlib seaborn xlrd openpyxl
   ```
3. Open the notebook and run the cells in order.
