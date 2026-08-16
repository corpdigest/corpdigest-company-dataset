---
title: A Minimum Metadata Standard for Company Reference Data
description: Three fields that decide whether a company revenue figure is usable or misleading - reporting currency, fiscal year, and last-reviewed date. An open standard proposal.
---

# A Minimum Metadata Standard for Company Reference Data

Company reference data is abundant and almost never citable. This is a proposal for the smallest
set of metadata that makes a revenue figure safe to use, written after compiling profiles for 414
companies from SEC EDGAR filings and annual reports.

It is a written standard. There is no dataset to download here and no code to install - the point
is the convention, not an artefact.

---

## The problem

Consider a single cell in a spreadsheet of company financials: `Siemens, 78914000000`.

Almost every company database on the internet will give you that number. Almost none will tell you
the three things you need in order to use it responsibly.

### 1. Which currency is that?

Siemens reports in euros. If that figure sits in a column beside four hundred US-dollar figures with
no currency label, every aggregate built on the column is quietly wrong - a total, a mean, a
"largest companies" ranking, a market-share calculation.

The failure is worse than it sounds, because it is silent. Nothing errors. The numbers still add up.
They are just describing different units.

The extreme cases are instructive. An Indian bank reporting total income of ₹3.12 trillion, if that
figure is silently treated as dollars, ranks as the largest company on earth - roughly four times
Amazon. The true figure is nearer $37 billion, an error of about eighty-fold. No amount of
downstream analysis recovers from that, and no reader of the final chart can detect it.

**The standard: every monetary column carries a sibling currency column.** Not a footnote, not a
convention documented elsewhere, not an assumption that "everything is USD." A column, on every row.

### 2. Which fiscal year does it cover?

A company with a March year-end publishes an "FY2026" figure that mostly describes calendar 2025. A
company with a June year-end publishes an "FY2026" figure covering the second half of 2025 and the
first half of 2026. A calendar-year filer's "FY2026" is 2026.

Put those three in the same "2026" column and you have three different periods presented as one.
For a single company tracked over time this is harmless. For cross-company comparison in a single
year - which is what almost everyone actually does with this data - it introduces a systematic
error that grows with how much the business cycle moved that year.

**The standard: state the fiscal year on the row, and state in the documentation that
cross-company comparison within one year column is approximate.** The second half matters as much
as the first. A field without a caveat invites misuse.

### 3. When was it last checked against a filing?

Market capitalisation moves daily. Chief executives change. Employee counts are restated. A company
profile is a point-in-time snapshot pretending to be a fact.

Most aggregators publish a figure with no indication of whether it was verified last week or
scraped from a competitor three years ago. The reader cannot distinguish fresh from stale, so
either trusts everything or trusts nothing.

**The standard: an ISO-format last-reviewed date on every record.** It costs one column and it
transfers the staleness judgement to the person best placed to make it.

---

## Why these three and not more

Richer provenance schemas exist and are better in every respect except adoption. The argument for
stopping at three fields is that they are the ones whose absence produces *silently wrong answers*
rather than merely incomplete ones.

Missing an industry classification makes a dataset less useful. Missing a currency label makes it
actively misleading, and misleading in a way the end reader cannot detect. That is a different
category of defect, and it is worth treating as a minimum bar rather than an enhancement.

## A fourth field worth considering: coverage depth

One convention that emerged while compiling company histories is worth passing on, though it is
less universal than the three above.

When a profile documents acquisitions or historical milestones, the count of documented items is
not the same as the count of real-world events. A company profile listing five acquisitions means
five acquisitions are documented in that profile - not that the company made exactly five.

Publishing that count as an explicit field, clearly labelled as coverage depth rather than fact,
lets a downstream user weigh the qualitative narrative separately from the financial figures.
Conflating "we have five records" with "five things happened" is one of the more common failure
modes in scraped company data, and it is entirely avoidable with honest labelling.

## A sanity check for any company dataset

Rank companies by revenue per employee. The top of that list should be real-estate investment
trusts, oil refiners and pharmaceutical distributors - businesses where enormous revenue flows
through very few people.

If a retail bank or a consumer brand appears at the top, you have almost certainly found a currency
error rather than an insight. It is a thirty-second check and it catches the single most damaging
class of error in this kind of data.

## Worked examples

These are company profiles built to the convention described above - each figure carries its fiscal
year and the date it was last reviewed against a filing, and each page lists the primary sources
behind it:

- [Microsoft acquisitions](https://corpdigest.com/company/microsoft/acquisitions) - deal history with documented coverage depth
- [Toyota company history](https://corpdigest.com/company/toyota/company-history) - dated milestones, non-calendar fiscal year
- [Oracle financials](https://corpdigest.com/company/oracle/financials) - multi-year revenue records
- [Starbucks founders](https://corpdigest.com/company/starbucks/founders) - founder backgrounds and roles
- [JPMorgan Chase business model](https://corpdigest.com/company/jpmorgan-chase/business-model) - fee income vs. net interest income split
- [IBM acquisitions](https://corpdigest.com/company/ibm/acquisitions) - integration cadence across a century of deals

The full sourcing rules, review cadence and correction policy are documented in the
[CorpDigest methodology](https://corpdigest.com/methodology), and the profiles themselves are at
[CorpDigest](https://corpdigest.com).

## Corrections

If you think part of this standard is wrong, or you have a case where the three fields are
insufficient, open an issue on this repository. Disagreement about where the minimum bar sits is
the useful conversation.

## License

This document is released under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) - quote it, adapt it, adopt it, with
attribution.
