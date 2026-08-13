# CorpDigest Company Profiles Dataset

An open, CC BY 4.0 dataset of **414 company profiles** — revenue, headcount, founding year, founders,
CEO, headquarters, industry and documented acquisition/milestone counts — compiled from SEC EDGAR
filings, annual reports and official investor-relations disclosures.

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-blue.svg)](https://creativecommons.org/licenses/by/4.0/)

## What's in it

| | |
|---|---|
| Companies | 414 |
| Countries | 26 |
| Industries | 135 |
| Founding years covered | 1727 – 2024 |
| Documented acquisitions | 1,362 |
| Documented history milestones | 3,349 |
| Combined headcount | 39.2 million |
| Fiscal years | mostly FY2025 / FY2026 |

Files:

- `data/corpdigest-company-profiles.csv` — the dataset (414 rows, 20 columns)
- `data/DATA_DICTIONARY.md` — column definitions, null counts, and the caveats that matter

## Quick start

```python
import pandas as pd

df = pd.read_csv("data/corpdigest-company-profiles.csv")

# Revenue is NOT uniformly USD — filter before you aggregate.
usd = df[df.revenue_currency == "USD"]
print(usd.nlargest(10, "revenue_reported")[["company_name", "revenue_reported", "employees"]])
```

## Read this before you aggregate

Three things will bite you if you skip the data dictionary:

1. **`revenue_reported` is not all USD.** 402 rows are USD; 12 report in EUR, GBP or INR and are
   labelled in `revenue_currency`. Sorting the raw column puts ICICI Bank above Amazon, because
   ₹3.12T is not $3.12T. No FX conversion is applied here on purpose — a single spot rate applied
   across fiscal years spanning 2023–2026 adds more error than it removes.
2. **Fiscal years are not calendar-aligned.** A March year-end "FY2026" mostly covers calendar 2025.
3. **`acquisitions_documented` measures editorial coverage, not corporate reality.** It is the count
   of deals documented in the profile, not the company's complete M&A history.

## Why this exists

Company reference data is widely available and almost never citable. Aggregators rarely say which
fiscal year a revenue figure covers, whether it was converted from a foreign currency, or when the
record was last checked against a filing. This dataset carries `revenue_currency`,
`revenue_fiscal_year` and `last_reviewed` on every row so a downstream user can decide whether a
number is fit for their purpose.

Each row links to a full editorial profile with per-figure source URLs. Some examples:

- [Microsoft acquisition history](https://corpdigest.com/company/microsoft/acquisitions)
- [Toyota company history](https://corpdigest.com/company/toyota/company-history)
- [Oracle revenue and financials](https://corpdigest.com/company/oracle/financials)
- [Starbucks founders](https://corpdigest.com/company/starbucks/founders)

Sourcing and review process: [CorpDigest methodology](https://corpdigest.com/methodology).

## Mirrors

The same dataset is published at:

- Zenodo (archival, DOI-citable) — *DOI added on release*
- Hugging Face Datasets
- Kaggle Datasets

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Use it commercially, modify it, redistribute
it — just credit the source.

```
CorpDigest (2026). CorpDigest Company Profiles Dataset (Version 1.0) [Data set].
https://corpdigest.com
```

## Contributing

Found a wrong figure? Open an issue with the company slug and a link to the primary filing that
contradicts it. Corrections backed by a filing get applied to both the dataset and the underlying
profile.
