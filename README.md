# Detecting Healthcare Fraud, Waste, and Abuse in Medicare Claims Using Ensemble Machine Learning

**Author:** Aliance Tedonfouet
**Status:** In progress. Phase 1 (data acquisition and documentation).
**Results:** None yet. This README will be updated as each phase is completed.

## Research question

Can ensemble machine learning models trained on publicly available Medicare claims
patterns identify providers with fraud risk indicators more accurately than baseline
classification approaches, and which billing-pattern features are most predictive?

## Independence and data statement

This is an independent study. It uses only publicly available U.S. government data.
No employer data, proprietary data, or non-public information is used at any stage.

## Data sources

| Dataset | Publisher | Role |
|---|---|---|
| Medicare Physician and Other Practitioners, by Provider and Service | CMS (data.cms.gov) | Billing features |
| List of Excluded Individuals and Entities (LEIE) | HHS Office of Inspector General (oig.hhs.gov/exclusions) | Exclusion labels |

Exact download dates and file details are recorded in `docs/DOWNLOAD_LOG.md`.

## What the labels mean (and do not mean)

The positive label is "provider appears in the LEIE with a fraud-related exclusion type."
This is **not** a measure of all fraud. It captures only fraud that was detected and
acted on, so some providers labeled "negative" are undetected fraud. Every result in
this study must be read with that limitation in mind.

## Reproducing Phase 1 (Data acquisition and documentation)
1. Create and activate a virtual environment, then `pip install -r requirements.txt`.
2. Download the datasets as described in `notebooks/01_data_acquisition.ipynb`
   into `data/raw/` and `data/external/`.
3. Run the notebook top to bottom.


## Planned phases
1. Data acquisition and documentation (current)
2. Exploratory data analysis
3. Feature engineering
4. Model development and cross-validated evaluation
5. Interpretation, limitations, and write-up


### Decisions

**1. Study period of 2019 to 2023.** A provider who is excluded stops billing Medicare. Matching the current LEIE to one billing year therefore finds very few cases (105 in 2023 alone). Using five years lets a provider who billed in an earlier year and was excluded later count as a positive case for that earlier year. I did not use 2024 because exclusions tied to 2024 billing may not be recorded yet. 2020 and 2021 were affected by COVID-19, so I will keep year as a variable and compare results with and without those years.

**2. How a provider-year is labeled.** Each provider gets its earliest fraud-coded exclusion year. A provider-year is positive if that exclusion falls after the billing year and within 3 years of it. If the exclusion year is the billing year or earlier, the row is flagged as "already excluded" and kept out of modeling because the timing is ambiguous. Everyone else is negative.

**3. Working definition of "fraud-related."** I used the LEIE exclusion codes 1128(a)(1), 1128(a)(3), 1128(b)(1), and 1128(b)(7). [CONFIRM: I checked these against the OIG's published exclusion authorities on (date I checked).] This is a judgment call. 1128(b)(1) also covers fraud in non-health-care programs, and 1128(b)(7) covers "other prohibited activities" beyond billing fraud. License-related codes such as 1128(b)(4) are not included. A few codes in the file (1128aa, brchsa, brchcia, 1160) are not described on the OIG authorities page or in the Record Layout, so I left them out instead of guessing. Phase 4 will test narrower and wider definitions.

**4. Cross-validation will be split by provider (NPI).** The same provider appears in several years. If one year of a provider were used for training and another for testing, the model would be tested on a provider it has already seen, and the scores would look better than they should.

### Limitations

- The LEIE lists detected fraud only. Providers labeled negative include undetected fraud, so model scores measure similarity to providers who were caught and excluded, not fraud itself.
- The LEIE file is the current list, so providers who were excluded and later reinstated are missing and look like negatives.
- Most LEIE records have no usable NPI, so only a fraction of fraud-coded exclusions can be matched to billing data.
- The CMS data covers Medicare fee-for-service Part B only. Results may not apply to Medicare Advantage, Medicaid, or commercial claims.
- Exclusion codes are a proxy for fraud. They record the legal reason for exclusion, not a finding about billing behavior.


### Next steps
- Phase 2 builds provider-level features from the billing data. 
- Phase 3 trains and compares the models with NPI-grouped cross-validation. 
- Phase 4 tests the sensitivity checks listed above. 
- Phase 5 covers interpretation and limitations.


## License

MIT. See `LICENSE`.
