# HIV 2015 Impact Score Calculation (GHI Data Team Task)

This project reproduces the **HIV 2015 Impact Score** calculation workflow in Python using the `HIV2015` sheet from the ORS workbook.

## What this script does
1. Loads the `HIV2015` sheet from the ORS workbook
2. Extracts the country-level HIV table (217 countries)
3. Detects the HIV drug impact columns (11 drugs)
4. Extracts and parses the regimen proportion + efficacy table
5. Recomputes impact score for each country and drug using the HIV formula
6. Applies retention normalization
7. Exports final results to `impact_score.csv`

## Formula used (HIV 2015)
For each regimen and subgroup (adult/child):

`impact = (DALY * treatment_coverage * x) / (1 - treatment_coverage * x)`

Then:
- divide by number of drugs in the regimen
- allocate to each drug in the regimen
- apply retention normalization:

`final_impact = pre_norm_impact * (100 - Retention Rate) / 100`

### Special handling
- `Others TDF based` is allocated directly to `TDF`
- `Others` is ignored for the 11 tracked HIV drugs
- countries with no usable coverage data (e.g., high-income/no-data rows) are set to zero impact

## Output files
- `impact_score.csv` → final submission file (long format)
  - columns: `Country`, `Drug`, `Impact Score`
- `impact_score_wide_computed.csv` → QA/helper file (wide format)
- `impact_score_comparison_vs_sheet.csv` → comparison of computed values vs sheet values

## How to run
```bash
python hiv_2015_impact_score.py
```

## Requirements

- Python 3.9+
- pandas

Install:
```bash
pip install pandas openpyxl
```
## Notes

- The script was validated by comparing computed values to the embedded HIV2015 impact columns in the ORS sheet.
- Results match the sheet values closely (with handling for no-data coverage rows).