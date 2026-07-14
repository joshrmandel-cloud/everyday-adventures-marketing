# Flags & Rules

The heuristics the agent runs against every prospect. Each rule: **trigger**, **severity**, **implication**, **producer action**.

Severity taxonomy (mirrors the output template):
- `BLOCKER` — do not advance without leadership sign-off.
- `HIGH` — advance carefully; must be addressed before any proposal.
- `MEDIUM` — raise on discovery call; can proceed in parallel.
- `LOW` — be aware; no action required, but the producer should know.

> Every threshold in this file is a **business rule** — replace with your firm's actual policy. Values below are reasonable middle-market broker defaults.

---

## Eligibility & market-fit flags

### `sub-100-eligible-headcount`
- **Trigger:** 5500 or best-available headcount source shows < 100 eligible employees.
- **Severity:** `HIGH`.
- **Implication:** Below the standard mid-market cutoff for [Firm]'s benefits practice. May be a small-group / different-team fit.
- **Action:** Confirm headcount trajectory (are they growing through 100?). If not, hand off to the small-group team or decline.

### `over-5000-eligible-headcount`
- **Trigger:** Headcount > 5,000.
- **Severity:** `LOW` (informational).
- **Implication:** Large-market resources should be engaged early — different pricing, different service model, different consultants.
- **Action:** Loop in large-market team before SDP intake.

### `fully-insured-under-250`
- **Trigger:** Fully insured funding + between 100 and 250 EEs.
- **Severity:** `MEDIUM`.
- **Implication:** Level-funded / captive alternatives may be a compelling value proposition. Producer should be ready.
- **Action:** Frame the funding-arrangement conversation in the discovery call.

## Compliance flags

### `no-5500-on-file`
- **Trigger:** No Form 5500 found for the plan sponsor.
- **Severity:** `HIGH` (context-dependent — see below).
- **Implication:** Possible reasons: (1) welfare plan with < 100 participants (exempt), (2) governmental or church plan (exempt), (3) brand-new plan, (4) delinquent filer, (5) wrong EIN.
- **Action:** Confirm plan sponsor status on the discovery call. If they are a required filer and haven't, that's a compliance conversation the firm can lead with — but do not accuse.

### `delinquent-5500-filing`
- **Trigger:** 5500 exists but the most recent filing is more than 12 months past its deadline (7 months after plan year end, plus a 2.5-month extension).
- **Severity:** `HIGH`.
- **Implication:** DOL penalty exposure (~$2,739/day as of 2024). Immediate value-add opportunity via the Delinquent Filer Voluntary Compliance program.
- **Action:** Raise carefully — this is a "we can help" moment, not a gotcha.

### `mewa-or-multi-employer`
- **Trigger:** 5500 indicates MEWA (Multiple Employer Welfare Arrangement) or multi-employer plan participation.
- **Severity:** `MEDIUM`.
- **Implication:** More complex governance, different sales cycle.
- **Action:** Loop in the specialty practice before SDP intake.

### `pending-erisa-litigation`
- **Trigger:** News/SEC search returns active ERISA-related litigation against the prospect.
- **Severity:** `HIGH`.
- **Implication:** Fiduciary process and vendor selection just became a lot more sensitive.
- **Action:** Handle with care. Do not use as a wedge in the sales conversation.

## Sales flags

### `pipeline-overlap`
- **Trigger:** SharePoint, Client Directory, or Producer Inbox reveals another producer at the firm is actively working this prospect.
- **Severity:** `BLOCKER`.
- **Implication:** Two producers on the same prospect creates channel conflict.
- **Action:** STOP. Escalate to sales leadership before any outreach.

### `active-client-relationship`
- **Trigger:** Client Directory shows the prospect (or a subsidiary / parent) is a current client.
- **Severity:** `BLOCKER`.
- **Implication:** This is an existing account, not a prospect. The relationship owner leads.
- **Action:** Contact relationship owner, redirect the conversation.

### `related-entity-relationship`
- **Trigger:** A parent, subsidiary, or PE sibling of the prospect is a current or prior client.
- **Severity:** `MEDIUM`.
- **Implication:** Potential warm intro and reference — or potential political sensitivity.
- **Action:** Coordinate with the related-entity relationship owner before outreach.

### `incumbent-is-strategic-partner`
- **Trigger:** Inferred incumbent broker is a partner, JV counterparty, or otherwise strategically sensitive to the firm.
- **Severity:** `HIGH`.
- **Implication:** Competing on this account has strategic implications.
- **Action:** Leadership sign-off before advancing.

### `hostile-prior-interaction`
- **Trigger:** Producer Inbox or SharePoint indicates the prospect previously declined the firm or had a negative interaction.
- **Severity:** `MEDIUM`.
- **Implication:** Prior context reshapes the pitch. Acknowledge, don't ignore.
- **Action:** Read the prior thread. Decide whether to lead with the reset or let the prospect raise it.

## Vertical flags

Replace this list with your firm's actual industry appetite.

### `restricted-vertical`
- **Trigger:** Industry is on the firm's restricted list.
- **Severity:** `BLOCKER`.
- **Default restricted list (illustrative — REPLACE):** cannabis, adult entertainment, firearms manufacturing, gambling, cryptocurrency exchanges.
- **Implication:** Firm policy prohibits engagement.
- **Action:** Decline. Provide referral if appropriate.

### `high-hazard-workforce`
- **Trigger:** NAICS or industry indicates high injury frequency (construction, oil & gas, heavy manufacturing, logging, mining).
- **Severity:** `MEDIUM`.
- **Implication:** Workers' comp interplay with health plan is a first-order discovery item. Stop-loss underwriting will be more conservative.
- **Action:** Loop in the P&C team early if the firm has one.

### `heavily-unionized`
- **Trigger:** Industry filings, news, or LinkedIn indicate significant union representation.
- **Severity:** `MEDIUM`.
- **Implication:** Collectively bargained benefits require different discovery and different plan design flexibility.
- **Action:** Ask about CBAs and multi-employer plan participation on discovery call.

### `multi-state-complexity`
- **Trigger:** Operations in 10+ states or in states with distinct benefits regulation (CA, NY, WA, MA, CO minimum).
- **Severity:** `LOW`.
- **Implication:** Carrier network breadth, state-mandated benefits, and PFML programs are all in play.
- **Action:** Flag in Specs Sheet; SDP will handle in depth.

### `international-workforce`
- **Trigger:** Website, LinkedIn, or filings indicate meaningful non-US workforce.
- **Severity:** `LOW`.
- **Implication:** Global benefits scope. If the firm has a Global Benefits practice, loop them in.
- **Action:** Ask "how do you handle non-US employees today?" on discovery.

## Data-quality flags

### `headcount-mismatch`
- **Trigger:** 5500 EE count and LinkedIn / website / Benefit Flow disagree by >25%.
- **Severity:** `MEDIUM`.
- **Implication:** Something is off — stale filing, contractor-heavy workforce, subsidiary confusion, recent M&A. Discovery must reconcile.
- **Action:** Note the discrepancy, propose the likely reason, ask on the call.

### `stale-benefit-flow`
- **Trigger:** Benefit Flow record last updated > 18 months ago.
- **Severity:** `LOW`.
- **Implication:** Incumbent broker, carriers, and headcount may have all changed.
- **Action:** Treat Benefit Flow as directional; prefer 5500 and website for current facts.

### `wrong-entity-suspected`
- **Trigger:** EIN, DBA, and legal name don't reconcile cleanly (e.g., 5500 EIN maps to a different named entity).
- **Severity:** `HIGH`.
- **Implication:** You may be researching the wrong company. Everything downstream is suspect.
- **Action:** Stop, resolve entity identity with the producer, then re-run.

### `recent-material-event`
- **Trigger:** News or 8-K within 30 days indicating layoffs, plant closure, major litigation, C-suite change, or M&A announcement.
- **Severity:** `HIGH`.
- **Implication:** The benefits conversation just got harder — or just got easier. Either way, don't walk into the call blind.
- **Action:** Acknowledge the event in the discovery call opening; frame the conversation around it.

---

## How the agent applies these

- Run every rule. Emit every flag that fires — do not deduplicate or "soften" for tone.
- Rank flags by severity (`BLOCKER` first), then by relevance to the deal.
- If a `BLOCKER` fires, the Readiness Assessment must be `NOT YET` and the blocker must be listed as the first gap to close.
- If no flags fire, still emit the section with `None identified.` — silence is not the same as absence.
