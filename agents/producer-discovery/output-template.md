# Output Template — Producer Discovery Agent

The agent returns **exactly** these sections, in this order. Section headers are stable so downstream tooling (Prospect Opportunity records, SDP handoff docs) can parse them. Empty sections still appear with `None identified.`

Source tags on every fact: `[5500]`, `[BenefitFlow]`, `[LinkedIn]`, `[Website]`, `[SEC]`, `[News]`, `[SharePoint]`, `[Inbox]`, `[ClientDirectory]`, `[Web]`, `[Producer-provided]`. Internal-only items also get an `**INTERNAL**` prefix.

---

## Header

- **Prospect:** [Legal name] (DBA: [name]) — [HQ city, state]
- **Deal source:** [source] · **Trigger:** [why now]
- **Prepared for:** [Producer name] · **Prepared on:** [date]
- **Identity confidence:** [High / Medium / Low — one-line explanation if not High]

## 1. Prospect Specs Sheet

Mirrors the Prospect Opportunity record so it copy-pastes cleanly. Replace subsection names with your CRM's actual field list.

### Company Profile
- Business description (1 paragraph, agent's own words)
- Industry / NAICS code
- Ownership (public / private / PE-backed / non-profit) + sponsor if applicable
- Locations / geographic footprint
- Headcount (5500 EE count vs LinkedIn vs website — reconcile if they disagree)
- Recent notable events (M&A, funding, leadership change, litigation) — 12-month window

### Financials (public or inferable only — no speculation)
- Revenue range
- Profitability signal (if public)
- Capital structure notes

### Current Benefits Landscape (from 5500 + Benefit Flow)
- Plan year
- Funding type (fully insured / self-funded / level-funded)
- Active participants
- Medical carrier / TPA
- Pharmacy (PBM)
- Stop-loss carrier (if identifiable)
- Ancillary lines (dental, vision, life, disability) — if data available
- Notable plan features or gaps

### Decision Makers (buying committee)
For each known or high-probability contact:
- Name, title, LinkedIn URL
- Relationship signal (tenure, prior employers, mutual connections)
- Likely role in decision (economic buyer / user / gatekeeper / champion / detractor)

### Sales Process
- Current stage of the opportunity
- Timing / deadlines
- Known evaluation criteria
- Known objections

### Competition
- Incumbent broker (if known or inferable — cite source)
- Other brokers likely competing
- Consultants involved

### Next Actions
Lifted from Section 6 below — kept here so the Specs Sheet is standalone-usable.

## 2. Immediate Flags

Ranked most-severe first. Each flag: **[SEVERITY] Flag name** — 1-2 sentences on what's flagged, what it implies, and what the producer should do.

Severity taxonomy: `BLOCKER` (do not advance), `HIGH` (advance carefully, address before proposal), `MEDIUM` (raise on discovery call), `LOW` (be aware).

Categories to check (see `flags-and-rules.md` for full rules):
- Eligibility flags (headcount, funding type, market segment fit)
- Compliance flags (missing 5500, delinquent filing, ERISA issues)
- Sales flags (pipeline overlap with another producer, incumbent is a Lockton relationship, hostile prior interaction)
- Vertical flags (restricted industry, high-hazard, regulated)
- Data-quality flags (large discrepancies between 5500, Benefit Flow, LinkedIn, website)

If none: `None identified.`

## 3. Top 5 Must-Ask Discovery Questions

Exactly five. These are the questions the producer *cannot* answer from public sources *and* whose answers will meaningfully change how the deal is worked.

Format each as:
- **Q:** [The question, phrased for the actual conversation]
- **Why:** [1 sentence on why this matters]
- **Listen for:** [what a good vs concerning answer sounds like]

## 4. Extended Discovery Question Bank (Prospect-Specific)

Drawn from `discovery-question-bank.md`, filtered and reworded for this prospect. Grouped by topic:

- **Business context**
- **Current benefits program**
- **Financial & budget**
- **Employee experience**
- **Compliance & risk**
- **Vendor & broker relationships**
- **Decision process**
- **Vertical-specific**

Each question includes a `Why` line. Do not include questions the producer can already answer from the Specs Sheet — that's a sign the agent didn't do its homework.

## 5. Industry Insights

Three to five insights *specific to this prospect*, not the industry in general. Each: one sentence claim + one sentence relevance to the deal + source tag. Examples of the right level of specificity:

- Good: "The company's largest peer just moved to a level-funded arrangement and cited a 12% first-year savings [News] — worth asking whether the prospect's benefits committee has debated the same shift."
- Bad: "The healthcare industry is experiencing rising costs."

## 6. Recommended Next Steps & Resources

- **Next steps:** ordered list of 3-7 concrete actions with owners and suggested timing. Format: `[ ] Owner — action — by when`.
- **Resource docs to send:** links to internal templates and reference docs the producer should have handy (lead submission form, pricing template, SDP intake form, etc.). Pulled from SharePoint.

## 7. Sources & Confidence Log

A table of every source consulted:

| Source | Query / doc | Retrieved | Confidence | Notes |
|---|---|---|---|---|
| 5500 | EIN 12-3456789, PY 2024 | 2026-07-14 | High | Filed on time |
| BenefitFlow | Record ID … | 2026-07-14 | Medium | Employee count 18mo stale |
| LinkedIn | CEO profile | 2026-07-14 | High | |
| … | | | | |

Also note steps you couldn't complete and why (tool unavailable, no data found, ambiguous entity).

## 8. Readiness Assessment

- **Ready for SDP handoff:** YES / NOT YET
- **If NOT YET, gaps to close first:**
  - Gap 1 (owner, how to close)
  - Gap 2 (owner, how to close)

---

## Formatting rules

- Use plain Markdown so output pastes cleanly into Word, Outlook, and CRM notes.
- Bullets, not paragraphs, wherever possible.
- Every claim carries a source tag.
- INTERNAL items are visibly marked and clustered so they can be stripped before a client-facing rewrite.
- Total length target: 2-4 pages. If longer, the agent is padding.
