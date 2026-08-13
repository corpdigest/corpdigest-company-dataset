---
title: CorpDigest Company Profiles Dataset
description: Open CC BY 4.0 dataset of 414 company profiles — revenue, headcount, founders, CEO and acquisition counts, with currency and fiscal-year labels on every row.
---

# CorpDigest Company Profiles Dataset

An open dataset of **414 companies** across 26 countries and 135 industries, compiled from SEC EDGAR
filings, annual reports and official investor-relations disclosures. Released under CC BY 4.0.

[Download the CSV](https://github.com/corpdigest/corpdigest-company-dataset/blob/main/data/corpdigest-company-profiles.csv)
· [Data dictionary](https://github.com/corpdigest/corpdigest-company-dataset/blob/main/data/DATA_DICTIONARY.md)
· [Source profiles](https://corpdigest.com)

---

## The problem this dataset is trying to fix

Most company reference data is unusable for research, and the reason is almost always the same: the
figures arrive without the metadata needed to interpret them.

Take a single cell — `Siemens, 78914000000`. To use that number responsibly you need to know three
things that are usually missing:

**Which currency?** Siemens reports in euros. If that figure sits in a column alongside 400 USD
figures with no currency label, every aggregate built on it is quietly wrong. In this dataset, twelve
of the 414 companies report in EUR, GBP or INR, and `revenue_currency` says so on every row. The
extreme case is instructive: ICICI Bank's ₹3.12 trillion, if silently treated as dollars, ranks it
as the largest company on earth — roughly four times Amazon. It is closer to $37 billion.

**Which fiscal year?** A company with a March year-end publishes an "FY2026" figure that mostly
covers calendar 2025. Two companies in the same "2026" column can be describing periods a year
apart. `revenue_fiscal_year` is on every row, normalised to a four-digit year, and the data
dictionary is explicit that cross-company comparison inside one year column is approximate.

**When was it last checked?** Market caps move daily and CEOs change. `last_reviewed` gives an ISO
date so a user can judge staleness rather than assume freshness.

None of this is sophisticated. It is just metadata that aggregators tend to drop because it makes
the dataset look messier than a clean grid of numbers. The clean grid is the lie.

## What's in the file

| Field group | Columns |
|---|---|
| Identity | `slug`, `company_name`, `ticker`, `exchange`, `profile_url` |
| Classification | `industry`, `country`, `headquarters` |
| Origin | `founded_year`, `founders` |
| Leadership | `ceo` |
| Financials | `revenue_reported`, `revenue_currency`, `revenue_fiscal_year`, `net_income_reported`, `market_cap_usd` |
| Scale | `employees` |
| Coverage depth | `acquisitions_documented`, `milestones_documented`, `last_reviewed` |

Summary statistics:

- 414 companies, 26 countries, 135 industries
- Founding years from **1727** to **2024**
- 1,362 documented acquisitions and 3,349 dated history milestones
- 39.2 million employees in combined headcount
- 233 listed companies with tickers; 181 private or unlisted

## Loading it

```python
import pandas as pd

df = pd.read_csv("corpdigest-company-profiles.csv")

# Always filter on currency before aggregating revenue.
usd = df[df.revenue_currency == "USD"]

usd.nlargest(10, "revenue_reported")[
    ["company_name", "revenue_reported", "revenue_fiscal_year", "employees"]
]

# Revenue per employee — a rough proxy for capital vs labour intensity.
usd = usd.assign(rev_per_employee=usd.revenue_reported / usd.employees)
usd.nlargest(10, "rev_per_employee")[["company_name", "industry", "rev_per_employee"]]
```

That last query is a decent sanity check on any company dataset. The top of the list should be
REITs, oil refiners and pharmaceutical distributors — businesses where enormous revenue flows through
very few people. If a retail bank or a consumer brand appears at the top, you have found a currency
error, not an insight.

## Coverage depth as a first-class column

`acquisitions_documented` and `milestones_documented` are unusual columns and worth explaining. They
do not claim to count a company's real acquisitions or real history. They count what the underlying
profile documents.

That distinction matters for anyone doing comparative work. A company showing 12 milestones has a
deeper editorial record than one showing 5 — which tells you something about how much weight to put
on the qualitative narrative, separately from how much to trust the financial figures. Conflating
"we have five records" with "five things happened" is one of the more common failure modes in
scraped company data.

## Underlying profiles

Every row links to a full profile with per-figure source URLs. Representative examples:

- [Microsoft acquisitions](https://corpdigest.com/company/microsoft/acquisitions) — deal history and strategic rationale
- [Toyota company history](https://corpdigest.com/company/toyota/company-history) — dated milestones from the loom works onward
- [Oracle financials](https://corpdigest.com/company/oracle/financials) — multi-year revenue records
- [Starbucks founders](https://corpdigest.com/company/starbucks/founders) — founder backgrounds and roles

Sourcing rules, review cadence and correction policy are documented in the
[CorpDigest methodology](https://corpdigest.com/methodology).

## Citation

```
CorpDigest (2026). CorpDigest Company Profiles Dataset (Version 1.0) [Data set].
https://corpdigest.com
```

## Corrections

Wrong figures get fixed. Open an issue with the company `slug` and a link to the primary filing that
contradicts the value. Corrections are applied to both this dataset and the source profile.

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — commercial use, modification and
redistribution permitted with attribution.
