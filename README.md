
# Medicare Geographic Variation, 2014–2024

This project uses CMS Medicare Geographic Variation data to explore differences in Original Medicare spending and hospital use across states and counties. USDA Rural–Urban Continuum Codes support comparisons between rural and urban counties.

## Analysis

- Spending trends and geographic differences across 2014–2024.
- Changes in Original Medicare and Medicare Advantage enrollment.
- Correlations between spending, inpatient stays, readmissions and emergency department visits.
- Rural–urban comparisons and spending differences by Medicaid eligibility share.
- National spending forecasts using 2-year and 3-year moving averages.

Results are presented in an HTML report with figures, tables, interpretations and policy recommendations. Findings are descriptive and do not establish causation.

## Reproducing the analysis

1. Run `notebooks/01_cleaning.ipynb` to clean the raw data.
2. Run `notebooks/02_analysis.ipynb` to generate the analysis and figures.
3. Open the final HTML report in a browser.

The notebooks use Python with pandas, matplotlib, seaborn and Plotly.

## Professor feedback: beneficiary health risk

**Nini’s comment:** Ask whether high-spending places have sicker patients. The file includes an average beneficiary risk score, so check whether the relationship survives after accounting for it.

**Our response:** We checked the original 2014–2024 CMS dataset and found no column containing “risk” or “HCC.” The Average HCC Score (`BENE_AVG_RISK_SCRE`) appears in the earlier 2014–2022 CMS dictionary but is not listed in the current 2014–2024 dictionary. We therefore cannot adjust our spending–hospital-use analysis using this score.

Differences in beneficiary health needs may partly explain the observed relationships. Standardizing geographic payment rates does not account for these health differences, so we acknowledge this limitation in the report.

## Authors

Avanika Sane and Kanishka Agarwal
