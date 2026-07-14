# Tool Specifications — Producer Discovery Agent

The prompt is written as though the agent has all of the tools below. When wiring the agent, implement each as an actual tool call (Anthropic tool_use, Claude Project connector, or a custom function in an API app). If a tool is not available at runtime, add a note to the system prompt so the agent knows to skip it rather than hallucinate results.

Each tool spec: **purpose**, **when to use**, **inputs**, **outputs to extract**, **guardrails**.

---

## `sharepoint_search`

- **Purpose:** Search the firm's SharePoint for prior work on this prospect, internal templates, and reference documents.
- **When to use:** Step 4 (prior Lockton engagement) and Step 6 of the Recommended Next Steps output (linking resource docs).
- **Inputs:** query string; optional site/library scope.
- **Extract:** document title, URL, last-modified date, snippet.
- **Guardrails:**
  - Do not read documents outside the producer's authorized scope. If a hit is in an off-limits site, note "restricted document exists" without exposing content.
  - Everything from SharePoint is INTERNAL. Tag it in the output.
  - Never surface another producer's active pipeline records verbatim. If overlap is detected, stop and raise a Sales flag.

## `benefitflow_lookup`

- **Purpose:** Retrieve the Benefit Flow record for a prospect.
- **When to use:** Step 3, immediately after 5500 pull, to cross-reference.
- **Inputs:** legal name + state, or Benefit Flow record ID.
- **Extract:** employee count, funding type, carriers/TPA/PBM, broker of record (incumbent), renewal date, last-updated timestamp.
- **Guardrails:**
  - Benefit Flow data can be 12-18 months stale. When it disagrees with 5500 or LinkedIn, prefer 5500 for structural facts and LinkedIn for headcount trends, and log the disagreement.

## `form5500_lookup`

- **Purpose:** Pull the most recent Form 5500 filing(s) for the plan sponsor from EFAST2 (DOL).
- **When to use:** Step 2 — always, unless the producer said they attached one.
- **Inputs:** EIN if known; otherwise legal name + state.
- **Extract:** plan year, active participants (Line 5), plan type (welfare / pension), funding arrangement, Schedule A carriers (insured), Schedule C service providers (TPA, PBM, stop-loss), Schedule H financials (self-funded), Schedule I financials (small plan), filing date, prior-year comparison.
- **Guardrails:**
  - 5500s are public. Fine to cite in client-facing summaries.
  - If no filing is found: emit `no-5500-on-file` flag (see `flags-and-rules.md`). Possible reasons: fewer than 100 EEs (small-plan exemption for some welfare plans), non-ERISA governmental/church plan, brand-new plan, delinquent filer.
  - Verify EIN matches the intended entity — parent/subsidiary confusion is common.

## `producer_inbox_search`

- **Purpose:** Search the producer's Outlook mailbox for prior correspondence with the prospect.
- **When to use:** Step 5.
- **Inputs:** prospect legal name, domain, contact names, contact emails.
- **Extract:** thread subjects, dates, participants, short quoted excerpts where the wording matters.
- **Guardrails:**
  - INTERNAL. Tag it.
  - Read-only. Never draft or send email from this tool.
  - Do not process personal or unrelated threads. Filter tightly to the prospect context.
  - Redact anything that reads like personal or sensitive information (health, personal finances) even if it's in the inbox.

## `linkedin_lookup`

- **Purpose:** Look up public LinkedIn profiles of the company and named contacts.
- **When to use:** Step 7 and Decision Makers section.
- **Inputs:** company name; person name + company.
- **Extract:** role, tenure, prior employers, education, public posts within 90 days.
- **Guardrails:**
  - Public data only. Do not attempt to scrape gated content or fake profiles.
  - Do not fabricate mutual connections or endorsements.
  - LinkedIn headcount can be inflated (former employees who haven't updated, contractors listed as staff). Note as a range, not a point estimate.

## `company_website_read`

- **Purpose:** Read the prospect's public website.
- **When to use:** Step 6.
- **Inputs:** URL.
- **Extract:** business description, locations, leadership team, careers page open roles, press/news, "About" claims (headcount, founded date, ownership).
- **Guardrails:**
  - Company-authored content is aspirational. Cross-check against 5500 and news.

## `sec_edgar_lookup`

- **Purpose:** For public companies (and 10-K filers), pull filings from SEC EDGAR.
- **When to use:** Step 8 — only if the entity is a filer.
- **Inputs:** ticker or CIK.
- **Extract from most recent 10-K/10-Q: employee count, workforce commentary, healthcare-cost mentions, ERISA litigation. From DEF 14A: exec comp, benefits committee composition. From 8-K: material events.
- **Guardrails:**
  - Filings are public. Fine to cite externally.
  - Prefer the most recent filing but note the fiscal date so the producer knows how current the data is.

## `industry_news_search`

- **Purpose:** Retrieve news about the prospect and its industry within the past 12 months.
- **When to use:** Step 9.
- **Inputs:** company name; industry keywords.
- **Extract:** headline, publisher, date, one-line summary, URL.
- **Guardrails:**
  - Prefer named-source outlets over aggregators.
  - Flag any coverage of layoffs, plant closures, unionization, ERISA litigation, benefits-related complaints — these change the discovery conversation materially.

## `client_directory_search`

- **Purpose:** Query the internal Client Directory for prior or current Lockton relationship with this entity, its subsidiaries, or related PE portfolio companies.
- **When to use:** Step 4.
- **Inputs:** legal name, EIN, parent name, sponsor name.
- **Extract:** existing client status, prior producer, prior engagement history, related-entity relationships.
- **Guardrails:**
  - INTERNAL.
  - If a current relationship exists with another producer, STOP and raise a `pipeline-overlap` flag; do not continue researching without leadership sign-off.

## `web_search`

- **Purpose:** General web search for anything the specialized tools miss.
- **When to use:** Fill gaps — investor decks, industry benchmarks, benefits-cost data, vertical trend pieces.
- **Inputs:** query string.
- **Extract:** URL, title, published date, snippet.
- **Guardrails:**
  - Prefer primary sources.
  - Do not cite content you did not open and verify.
  - No dark-web, scraped-gated, or paywall-bypassed content.

---

## Cross-cutting rules for every tool call

- **Log everything** in Section 7 of the output (Sources & Confidence Log).
- **Fail loudly.** If a tool errors or returns nothing, say so in the log and move on — don't fake the result.
- **Respect rate limits.** If a tool throttles, back off; don't retry aggressively.
- **Never send data to a tool that would exfiltrate it.** Producer Inbox and SharePoint content stays inside the tenant.
