# Data Dictionary

## Project

**Title:** Geographic Variation in Original Medicare Spending and Hospital Use
**Coverage:** United States, 2014–2024
**Geographic levels:** National, state, and county
**Population:** All ages; see the CMS methodology note below.

## Data files

| File                                              | Location        | Description                                                                   |   Rows |
| ------------------------------------------------- | --------------- | ----------------------------------------------------------------------------- | -----: |
| `Medicare_GV_by_National_State_County_2024.csv` | `data/raw/`   | CMS source file, retained as downloaded                                       | 36,994 |
| `Ruralurbancontinuumcodes2023.csv`              | `data/raw/`   | USDA ERS 2023 Rural-Urban Continuum Codes source file, retained as downloaded |  9,703 |
| `national_clean.csv`                            | `data/clean/` | National observations; one row per year                                       |     11 |
| `state_clean.csv`                               | `data/clean/` | State and DC observations; one row per state and year                         |    561 |
| `county_clean.csv`                              | `data/clean/` | County observations with USDA rural/urban classifications where matched       | 34,481 |

**Observation unit:** One geography in one calendar year.
**Year range:** 2014–2024, inclusive.

## Data types and conventions

- The tables below describe the cleaned CSV files.
- `year` is stored as an integer. `fips` must be read as text to preserve leading zeros.
- Percentages use a 0–100 scale; for example, `32.13` means 32.13%.
- Dollar measures are nominal US dollars and are not adjusted for inflation.
- Blank CSV cells represent missing values and should be read as null/`NaN`.
- CMS suppression markers (`*`) were converted to missing values; suppressed values were not imputed.
- Unless a field’s definition specifies otherwise, confirm its denominator and population coverage in the CMS methodology before interpreting it.

## Variables

| Variable               | Data type         | Description                                                                                                                                                                           | Units / valid values                                                | Source or derivation            |
| ---------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------- |
| `year`               | Integer           | Calendar year associated with the observation.                                                                                                                                        | 2014–2024                                                          | CMS                             |
| `name`               | String            | Name of the geography.                                                                                                                                                                | `National`; state abbreviation; or county name as supplied by CMS | CMS                             |
| `fips`               | String, nullable  | Federal Information Processing Standards (FIPS) geographic identifier. Leading zeros are significant.                                                                                 | 2 characters for states; 5 for counties; blank for national         | CMS                             |
| `benes_total`        | Numeric, nullable | Total number of Medicare beneficiaries reported for the geography and year.                                                                                                           | Beneficiaries                                                       | CMS                             |
| `benes_om`           | Numeric, nullable | Number of beneficiaries enrolled in Original Medicare (OM) reported for the geography and year.                                                                                       | Beneficiaries                                                       | CMS                             |
| `ma_rate_pct`        | Numeric, nullable | Medicare Advantage participation rate.                                                                                                                                                | Percent (0–100)                                                    | CMS                             |
| `avg_age`            | Numeric, nullable | Average age of beneficiaries.                                                                                                                                                         | Years                                                               | CMS                             |
| `female_pct`         | Numeric, nullable | Share of beneficiaries who are female.                                                                                                                                                | Percent (0–100)                                                    | CMS                             |
| `medicaid_pct`       | Numeric, nullable | Share of beneficiaries eligible for Medicaid.                                                                                                                                         | Percent (0–100)                                                    | CMS                             |
| `spend_pc_actual`    | Numeric, nullable | Actual Medicare payment per beneficiary, before geographic payment standardization.                                                                                                   | Nominal US dollars per beneficiary                                  | CMS                             |
| `spend_pc_std`       | Numeric, nullable | Standardized Medicare payment per beneficiary. CMS standardizes payment for geographic price differences. This measure is not, by itself, a direct measure of care volume or quality. | Nominal US dollars per beneficiary                                  | CMS                             |
| `ip_spend_pc_std`    | Numeric, nullable | Standardized inpatient payment per beneficiary.                                                                                                                                       | Nominal US dollars per beneficiary                                  | CMS                             |
| `ip_share_pct`       | Numeric, nullable | Inpatient standardized payments as a share of total standardized payments.                                                                                                            | Percent (0–100)                                                    | CMS                             |
| `ip_stays_per_1000`  | Numeric, nullable | Covered inpatient stays per 1,000 beneficiaries, as defined by CMS.                                                                                                                   | Stays per 1,000 beneficiaries                                       | CMS                             |
| `ip_users_pct`       | Numeric, nullable | Share of beneficiaries with at least one covered inpatient stay, as defined by CMS.                                                                                                   | Percent (0–100)                                                    | CMS                             |
| `readmit_rate_pct`   | Numeric, nullable | Hospital readmission rate, as defined by CMS.                                                                                                                                         | Percent (0–100)                                                    | CMS                             |
| `ed_visits_per_1000` | Numeric, nullable | Emergency department visits per 1,000 beneficiaries, as defined by CMS.                                                                                                               | Visits per 1,000 beneficiaries                                      | CMS                             |
| `rucc_code`          | Integer, nullable | USDA Rural-Urban Continuum Code for the county.                                                                                                                                       | Integer 1–9; see USDA documentation                                | USDA ERS, joined by county FIPS |
| `rural_urban`        | String, nullable  | Two-category county classification derived from the 2023 RUCC code.                                                                                                                   | `Urban` for RUCC 1–3; `Rural` for RUCC 4–9                    | Derived from`rucc_code`       |

## Keys and validation

- **National file:** `year` should uniquely identify a row.
- **State file:** `fips` and `year` should uniquely identify a row.
- **County file:** `fips` and `year` should uniquely identify a row.
- `name` is provided for readability; use `fips` rather than names to match county or state records.
- Before analysis, check for duplicate keys, expected year coverage, and unexpected or invalid FIPS values.

## Cleaning, exclusions, and limitations

- Only “All ages” records are retained. Separate age-group records are not included.
- Puerto Rico (`PR`), the US Virgin Islands (`VI`), and CMS placeholder records such as `Territory` and `ZZ` were excluded.
- Counties were limited to the 50 states and DC using their FIPS codes.
- Rural/urban classifications were added by a left join to the USDA 2023 RUCC file using five-character county FIPS codes.
- The 2023 RUCC classification is applied to every year from 2014 through 2024. It does not represent annual changes in a county’s rural/urban status.
- Counties without a matching USDA record have missing `rucc_code` and `rural_urban` values. They are excluded from rural/urban comparisons only.
- CMS-suppressed values remain missing. Analyses that require those values may therefore include fewer observations.
- Dollar values are nominal and are not inflation-adjusted. Changes over time should not be interpreted as real growth in spending without an inflation adjustment.

## Methodology note

CMS measure definitions may use different beneficiary populations or denominators. Before final submission, verify the population and denominator for spending, utilization, and demographic measures against the CMS documentation. Update the relevant variable descriptions if they do not refer to Original Medicare beneficiaries.

## Data sources

1. Centers for Medicare & Medicaid Services (CMS), *Medicare Geographic Variation — by National, State & County*, downloaded source file.
2. U.S. Department of Agriculture, Economic Research Service (USDA ERS), *Rural-Urban Continuum Codes, 2023*, downloaded source file.