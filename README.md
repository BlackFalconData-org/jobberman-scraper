# Jobberman Job Scraper

Extract structured data from [jobberman.com](https://jobberman.com) — jobberman.com — Nigeria's leading job board. Salary metadata, experience filters, and incremental monitoring.

**[Jobberman Job Scraper on Apify →](https://apify.com/blackfalcondata/jobberman-scraper)**

---

## Key features

**Search with filters** — Search by keyword and location. Filter by country, experience level, and more.

**Detail enrichment** — Fetch full job descriptions, salary data for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from jobberman.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from jobberman.com.

---

## Quick start

```json
{
  "query": "engineer",
  "maxResults": 50,
  "includeDetails": true
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Keyword query for Jobberman search. Use a JSON array string only if you want multiple keyword runs in one actor input. |
| `country` | enum | `"NG"` | Jobberman market to search. Current implementation supports Nigeria. |
| `location` | string | — | Location slug or label such as `lagos`, `abuja`, `remote`, or `rest-nigeria`. Use a JSON array string only if you want multiple locations. |
| `jobFunction` | string | — | Job function slug such as `engineering-technology`, `software-data`, or `accounting-auditing-finance`. Use a JSON array string for multiple values. |
| `industry` | string | — | Industry slug such as `it-telecoms`, `construction`, or `manufacturing-warehousing`. Use a JSON array string for multiple values. |
| `experience` | enum | `"mid-level"` | Experience filter for Jobberman. |
| `startUrls` | array | — | Optional direct Jobberman search or detail URLs. Useful for replaying a known search path or a single listing. |
| `maxResults` | integer | `25` | Maximum total job listings to return across all search sources. Use `0` for unlimited. Memory profile: up to 500 results uses 512 MB, above 500 uses 1024 MB. |
| `maxPages` | integer | `5` | Maximum Jobberman search result pages to scrape per search source. |
| `includeDetails` | boolean | `true` | Fetch each Jobberman detail page for full description, salary metadata, dates, and apply URL. |
| `includeCompanyProfile` | boolean | `false` | Reserved for later company profile enrichment. Current implementation does not fetch dedicated company pages yet. |
| `descriptionMaxLength` | integer | `0` | Truncate `description` to this many characters. Use `0` for no truncation. |
| `compact` | boolean | `false` | Return only core fields for AI-agent or MCP workflows. |
| `incrementalMode` | boolean | `false` | Compare the current run against stored state and emit change metadata. Requires `stateKey`. |
| `stateKey` | string | — | Stable identifier for the tracked search universe, for example `ng-engineer-lagos`. |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings that are still unchanged. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit jobs that disappeared from the current crawl. |

---

## FAQ

**Is it legal to scrape jobberman.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check the target site's terms of service for your specific use case.

**What kind of Jobberman data can I extract?**
You can extract core job fields such as title, company, category, industry, location, salary text, salary min/max, apply URL, posting dates, and full descriptions when detail enrichment is enabled.

**Can I monitor only new or changed jobs?**
Yes. Enable incremental mode and keep a stable `stateKey` for each search universe. Subsequent runs will only emit listings that are new, updated, or expired.

**Does it support AI-agent or MCP workflows?**
Yes. Compact mode returns a smaller payload with core fields only, which is useful for LLM and MCP tool calls where context size matters.

**Can I export the results to spreadsheets or databases?**
Yes. Apify datasets can be exported as JSON, CSV, Excel, XML, and other formats, or consumed directly through the Apify API in your own pipeline.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage.

---

## Known limitations

- Structured salary fields are strong but not universal. A small share of listings expose only salary text or confidential remuneration.
- Direct contact data is limited on Jobberman. `applyUrl` is reliable, while `contactEmail` and external apply links appear only on a minority of listings.
- `includeCompanyProfile` is reserved for future enrichment. The current actor does not crawl separate employer profile pages yet.
- Detail-side experience information is often phrased as years of experience rather than a normalized seniority label, so the actor keeps `experienceLevel` and `experienceRequirement` separate.

---

## Related products by Black Falcon Data

- [StepStone Scraper](https://github.com/BlackFalconData-org/stepstone-scraper) — Job listings from 18 European portals
- [Indeed Job Scraper](https://github.com/BlackFalconData-org/indeed-job-scraper) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://github.com/BlackFalconData-org/glassdoor-job-scraper) — Glassdoor listings with company ratings

---

*Last updated: 2026 03*
