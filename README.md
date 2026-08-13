# A Minimum Metadata Standard for Company Reference Data

**[Read the standard →](https://corpdigest.github.io/corpdigest-company-dataset/)**

Company reference data is abundant and almost never citable. This repository proposes the smallest
set of metadata that makes a company revenue figure safe to use.

It is a written standard. **There is no dataset here and no code here** - the point is the
convention, not an artefact.

## The three fields

| Field | Why it matters | What breaks without it |
|---|---|---|
| **Reporting currency** | Monetary figures are not all USD | Silent, undetectable errors in every aggregate. A ₹3.12T figure read as dollars ranks a mid-size bank as the largest company on earth. |
| **Fiscal year** | March and June year-ends are not calendar years | Two companies in the same "2026" column describing periods a year apart. |
| **Last-reviewed date** | Market caps and CEOs change | The reader cannot tell fresh from stale, so trusts everything or nothing. |

The full argument, including a fourth field on coverage depth and a thirty-second sanity check that
catches currency errors, is in [the standard itself](https://corpdigest.github.io/corpdigest-company-dataset/).

## Worked examples

Company profiles built to this convention:

- [Microsoft acquisitions](https://corpdigest.com/company/microsoft/acquisitions)
- [Toyota company history](https://corpdigest.com/company/toyota/company-history)
- [Oracle financials](https://corpdigest.com/company/oracle/financials)
- [Starbucks founders](https://corpdigest.com/company/starbucks/founders)

Sourcing and review process: [CorpDigest methodology](https://corpdigest.com/methodology).

## Contributing

Think the minimum bar sits somewhere else? Open an issue. Disagreement about where the line goes is
the useful conversation.

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
