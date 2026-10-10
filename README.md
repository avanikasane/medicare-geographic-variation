# medicare-geographic-variation
Medicare spending and hospital use across US states and counties, 2014–2024


**Nini’s comment:**
Ask whether high-spending places have sicker patients. The file includes an average beneficiary risk score, so check whether the relationship survives after accounting for it.

**Response:**
We checked the original 2014–2024 CMS dataset and found no column containing “risk” or “HCC.” The Average HCC Score (BENE_AVG_RISK_SCRE) appears in the earlier 2014–2022 CMS dictionary, but it is not listed in the current 2014–2024 dictionary.
We therefore cannot adjust our current spending–hospital-use analysis using this risk score. We will acknowledge that differences in beneficiary health needs may partly explain the observed relationships. Standardizing geographic payment rates does not account for those health differences.