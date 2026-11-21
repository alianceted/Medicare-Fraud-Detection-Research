# Download Log

This file records where each data file came from so the project can be reproduced. The raw files are large and are not stored in this repository (see `.gitignore`).

## Data files

| File | Source | Date downloaded | Size | Notes |
|---|---|---|---|---|
| `data/raw/cms_partb_2019_2023.csv` | CMS, [Medicare Physician & Other Practitioners, by Provider and Service](https://data.cms.gov/provider-summary-by-type-of-service/medicare-physician-other-practitioners/medicare-physician-other-practitioners-by-provider-and-service), billing years 2019 to 2023 | 2026-11-21 | 15 GB | 29 columns, including a `year` column. 48,891,840 rows. The file was created by combining the five yearly files together. |
| `data/external/hhs_leie_current.csv` | HHS OIG, [LEIE Downloadable Databases](https://oig.hhs.gov/exclusions/exclusions_list.asp), "10-2026 Updated LEIE Database" | 2026-11-20 | 10.8 MB | 84,224 records. Current exclusions only, so providers who were reinstated are not included. |

## Supporting documents

| File | Source | Notes |
|---|---|---|
| Three `MUP_PHY_RY25` PDFs | CMS | Documentation downloaded from the CMS dataset page. |
| `TBL_POS.xlsx` | CMS | Supporting table downloaded from the CMS dataset page. |


## Licensing note

The CMS file contains HCPCS procedure codes, which are CPT codes owned by the American Medical Association. The CPT license file that came with the download is excluded from the repository.
