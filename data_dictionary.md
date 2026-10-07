# Data Dictionary

Project: Medicare spending and hospital use across US states and counties, 2014–2024

## Files

| File | Folder | Description | Rows |
|---|---|---|---|
| `Medicare_GV_by_National_State_County_2024.csv` | `data/raw/` | CMS Medicare Geographic Variation file, as downloaded (unedited) | 36,994 |
| `Ruralurbancontinuumcodes2023.csv` | `data/raw/` | USDA ERS Rural-Urban Continuum Codes 2023, as downloaded (unedited) | 9,703 |
| `national_clean.csv` | `data/clean/` | US totals, one row per year, all ages | 11 |
| `state_clean.csv` | `data/clean/` | 50 states + DC, one row per state per year, all ages | 561 |
| `county_clean.csv` | `data/clean/` | Counties in the 50 states + DC, one row per county per year, with rural/urban label | [fill in] |

**Unit of observation:** one geography (nation, state or county) in one year.
**Years:** 2014–2024.

## Variables

All three clean files share the columns below. `county_clean.csv` has two extra columns (see the end of the table).

| Variable | Type | Description | Units / values | Original CMS column |
|---|---|---|---|---|
| `year` | integer | Calendar year of the data | 2014–2024 | Year |
| `name` | text | Name of the geography | "National"; 2-letter state abbreviation (e.g. `PA`); county name as given by CMS | State or County |
| `fips` | text | FIPS code identifying the state or county. **Must be read as text** to keep leading zeros. | 2 digits for states (e.g. `01`), 5 digits for counties (e.g. `01001`); empty for national | State and County FIPS Code |
| `benes_total` | number | Total Medicare beneficiaries | count of people | Total Medicare Beneficiaries |
| `benes_om` | number | Beneficiaries in Original Medicare (fee-for-service) | count of people | Original Medicare (OM) Beneficiaries |
| `ma_rate_pct` | number | Share of beneficiaries enrolled in Medicare Advantage | percent, 0–100 | MA Participation Rate |
| `avg_age` | number | Average age of beneficiaries | years | Average Age |
| `female_pct` | number | Share of beneficiaries who are female | percent, 0–100 | Percent Female |
| `medicaid_pct` | number | Share of beneficiaries also eligible for Medicaid (a marker of low income) | percent, 0–100 | Percent Eligible for Medicaid |
| `spend_pc_actual` | number | Actual Medicare payments per beneficiary | US dollars, nominal (not inflation-adjusted) | Actual Per Capita Medicare Payment |
| `spend_pc_std` | number | Standardized Medicare payments per beneficiary. CMS removes geographic differences in prices (e.g. local wage levels), so this reflects the amount of care used rather than local costs. **Main spending measure.** | US dollars, nominal | Standardized Per Capita Medicare Payment |
| `ip_spend_pc_std` | number | Standardized inpatient (hospital stay) payments per beneficiary | US dollars, nominal | IP Per Capita Standardized Medicare Payment |
| `ip_share_pct` | number | Inpatient share of total standardized payments | percent, 0–100 | IP Standardized Medicare Payment as % of Total Standardized Medicare Payment |
| `ip_stays_per_1000` | number | Covered inpatient hospital stays per 1,000 beneficiaries | stays per 1,000 | IP Covered Stays Per 1,000 Beneficiaries |
| `ip_users_pct` | number | Share of beneficiaries with at least one covered inpatient stay | percent, 0–100 | % of Beneficiaries Using IP |
| `readmit_rate_pct` | number | Hospital readmission rate, as defined by CMS | percent, 0–100 | Hospital Readmission Rate |
| `ed_visits_per_1000` | number | Emergency department visits per 1,000 beneficiaries | visits per 1,000 | Emergency Department Visits per 1,000 Beneficiaries |
| `rucc_code` | number | **County file only.** USDA Rural-Urban Continuum Code (2023) | 1–9 (1 = most urban, 9 = most rural) | from USDA file (`Value` where `Attribute` = `RUCC_2023`) |
| `rural_urban` | text | **County file only.** Rural/urban label derived from `rucc_code` | `Urban` = codes 1–3 (metro); `Rural` = codes 4–9 (nonmetro); empty if unmatched | created in cleaning |

## Planned derived variable (created in the analysis notebook)

| Variable | Type | Description | Formula |
|---|---|---|---|
| `non_ip_spend_pc_std` | number | Standardized spending per beneficiary on everything other than inpatient stays. Used to test the spending–hospital use relationship without the automatic overlap between total spending and hospital spending. | `spend_pc_std - ip_spend_pc_std` |

## Conventions and cleaning decisions

- **Percentages** are stored as 0–100 (e.g. `32.13` means 32.13%), not as 0–1.
- **Dollars** are nominal. They are not adjusted for inflation across years.
- **Missing values:** CMS hides small counts with `*`. These were converted to missing (empty) and **not** filled in. They are concentrated in small counties.
- **Age group:** only "All ages" rows are kept. Separate under-65 / 65+ rows exist only at national and state level.
- **Geographies dropped:** Puerto Rico (PR), US Virgin Islands (VI), "Territory" and "ZZ" (CMS placeholders with no FIPS code and all values hidden). Counties were filtered to the remaining 50 states + DC using the first two digits of their FIPS code.
- **Rural/urban join:** left join on 5-digit FIPS. [fill in] county rows had no USDA match (mainly Connecticut, which switched to planning regions in recent Census geography). These rows have an empty `rural_urban` and are excluded only from rural/urban comparisons.
- **Fixed classification:** the 2023 rural/urban codes are applied to all years (2014–2024).

## To verify before submission

- Confirm in the CMS methodology document which beneficiaries each measure covers (in particular, whether spending, utilization and demographic measures refer to Original Medicare beneficiaries only), and adjust the descriptions above if needed.
- Fill in the two `[fill in]` values from your notebook output.

## Sources

1. Centers for Medicare & Medicaid Services (CMS). *Medicare Geographic Variation – by National, State & County*. data.cms.gov.
2. U.S. Department of Agriculture, Economic Research Service (USDA ERS). *Rural-Urban Continuum Codes, 2023*. ers.usda.gov.
