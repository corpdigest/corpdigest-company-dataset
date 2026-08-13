# probe

line two
# Data Dictionary — CorpDigest Company Profiles Dataset

**File:** `corpdigest-company-profiles.csv`
**Rows:** 414 (one per company)
**Encoding:** UTF-8, comma-delimited, header row included
**Version:** 1.0 — 2026-08-13
**License:** CC BY 4.0

## Columns

| Column | Type | Description | Nulls |
|---|---|---|---|
| `slug` | string | Stable identifier used in the CorpDigest URL path. Primary key. | none |
| `company_name` | string | Legal entity name as it appears in the primary filing. | none |
| `ticker` | string | Primary listed ticker symbol. Empty for private companies. | 181 |
| `exchange` | string | Exchange on which `ticker` is listed. | 181 |
| `industry` | string | CorpDigest industry label. 135 distinct values. | few |
| `country` | string | Country of incorporation/headquarters. 26 distinct values. | few |
| `headquarters` | string | City and region of the head office. | few |
| `founded_year` | integer | Year of founding. Range 1727–2024. | none |
| `founders` | string | Semicolon-delimited founder names. | none |
| `ceo` | string | Chief executive as of `last_reviewed`. | few |
| `revenue_reported` | integer | Annual revenue **in the units of `revenue_currency`**. See caveat below. | none |
| `revenue_currency` | string | Reporting currency for `revenue_reported`. `USD` for 402 rows; `EUR`/`GBP`/`INR` for 12. | none |
| `revenue_fiscal_year` | integer | Fiscal year the revenue figure covers, normalised to a 4-digit year. | none |
| `net_income_reported` | integer | Net income, same currency convention as revenue. | some |
| `market_cap_usd` | integer | Market capitalisation in USD as of `last_reviewed`. Empty for private companies. | some |
| `employees` | integer | Full-time employee headcount. | none |
| `acquisitions_documented` | integer | Count of acquisitions with a documented entry in the profile. Not a complete deal history. | none |
| `milestones_documented` | integer | Count of dated company-history milestones in the profile. | none |
| `last_reviewed` | date | ISO date the profile was last editorially reviewed. | some |
| `profile_url` | string | Canonical URL of the full company profile. | none |

## Important caveats

**1. `revenue_reported` is not uniformly USD.** 402 of 414 rows are USD. Twelve rows report in the
company's native currency and are labelled accordingly in `revenue_currency`:

Deutsche Bank, DHL Group, GSK, ICICI Bank, IKEA (INGKA), Porsche AG, Puma, Renault, Sanofi,
Banco Santander, Siemens, Spotify.

Do **not** sort or aggregate `revenue_reported` without filtering on `revenue_currency` first, or
converting. No FX conversion is applied in this file because applying a single spot rate to figures
drawn from fiscal years spanning 2023–2026 would introduce more error than it removes.

**2. Fiscal years are not calendar-aligned.** A company with a March or June year-end reports a
"2026" fiscal year that overlaps calendar 2025. Cross-company revenue comparisons in a single year
column are approximate.

**3. `acquisitions_documented` and `milestones_documented` measure editorial coverage depth, not
corporate reality.** A count of 5 means five deals are documented in the profile, not that the
company made exactly five acquisitions.

**4. Private-company figures are estimates.** Where a company does not file publicly (IKEA, Rolex,
Huawei, OpenAI and others), revenue is drawn from company statements or credible press reporting and
carries wider uncertainty than a filed figure.

**5. Point-in-time snapshot.** Market cap and CEO fields change. Use `last_reviewed` to judge staleness.

## Provenance

Figures are compiled from SEC EDGAR filings (10-K, 20-F), company annual reports, and official
investor-relations releases. Per-company source URLs are listed in the "Sources & References"
section of each profile page linked in `profile_url`.

Methodology: https://corpdigest.com/methodology

## Citation

> CorpDigest (2026). *CorpDigest Company Profiles Dataset* (Version 1.0) [Data set].
> https://corpdigest.com
