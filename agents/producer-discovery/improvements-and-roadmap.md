# Improvements & Roadmap

Suggestions for making the Discovery Agent materially better, organized by (a) design gaps in the spec you shared, (b) v2 feature ideas, (c) governance, and (d) how to measure whether the agent is actually working.

---

## Design gaps in the current spec

Things the original spec omits or under-specifies that would meaningfully change the quality of the output.

### 1. The initial input is heavy on identifiers, light on producer intent
The spec lists Prospect Name, 5500, Benefit Flow, Deal Source, Known Contacts, DBA, Past Lockton engagement. Those are raw materials — none of them tell the agent *why the producer is running this now* or *what specifically they need*. Without producer intent, the agent produces well-rounded but generic output.

**Recommendation:** Add three fields to the input (already in `input-template.md`):
- *Trigger — why now?*
- *Producer's hypothesis on the deal*
- *Top question the producer needs answered*

The last one is the single highest-leverage addition. It reorients the whole brief around what the producer actually needs.

### 2. "Employee Sentiment Illustrative Agent" is orphaned
The spec mentions it once under "Expectation of the Agent" but never defines it. Is it a separate agent this one calls? A section this agent produces? A tool?

**Recommendation:** Decide. If it's a separate agent, define the interface. If it's a section, name it in the output. Right now it's a floating capability that will get built inconsistently.

### 3. No confidence or source-labeling requirement
"The company has ~250 EEs based on 5500" and "the company appears to have ~250 EEs based on LinkedIn" are very different claims — the first is a public filing, the second is a scraped self-report. Without source labeling, the producer can't tell how much to trust each item.

**Recommendation:** Require source tags on every claim (built into `system-prompt.md` — tags like `[5500]`, `[LinkedIn]`, etc.). Make it enforceable in evaluation: any un-sourced claim is a defect.

### 4. "Immediate flags" is a great section but under-specified
The spec lists three examples (missing 5500, sub-100 EEs, verticals). That's not enough to run against 35 producers consistently.

**Recommendation:** Build a taxonomy of flags with severity, trigger, and producer action (see `flags-and-rules.md`). Categories: eligibility, compliance, sales, vertical, data-quality.

### 5. Missing high-value outputs
Three things the spec doesn't ask for but should:

- **Incumbent broker intelligence.** Often the single most important thing to know before the call — how entrenched, how long, any known dissatisfaction. The current spec has this only implicitly under "Benefit Flow Summary."
- **Renewal date and timing urgency.** The spec has no timing field, but timing drives everything: whether the deal is workable this year, when SDP needs to be ready, when the proposal must land.
- **Decision-maker map.** Who's on the buying committee? Who's economic buyer vs. gatekeeper vs. user? The spec has "Known Contacts" but no structure around roles.

All three are now in `output-template.md`.

### 6. No "what the producer should NOT ask" list
Producers embarrass themselves by asking questions they could have answered from a 30-second website check. The agent should proactively suppress questions the Specs Sheet already answers.

**Recommendation:** Enforced in `system-prompt.md` under Output Contract rule #4 and in `discovery-question-bank.md` under "Rules for how the agent selects questions."

### 7. Questions returned as a dump, not ranked
The spec says "Questions to drive discovery specific to this prospect." Producers won't read 40 questions. They'll read 5.

**Recommendation:** Output "Top 5 Must-Ask" (the questions the producer cannot answer from public sources *and* whose answers change how the deal is worked), plus a longer bank as reference.

### 8. No handling of internal vs. public information leakage
The spec calls for reading SharePoint and Producer Inbox — great. But if the agent's output gets pasted into a client-facing document by mistake, internal notes go with it.

**Recommendation:** Prefix internal-sourced items with an `**INTERNAL**` tag so they can be visually stripped. Longer term, produce two outputs: a full internal brief and a scrubbed client-facing summary.

### 9. "Prospect Specs Sheet (Sectioned Similar to Prospect Opportunity)"
The spec assumes the Prospect Opportunity structure is known. If the agent guesses wrong, the output doesn't paste cleanly.

**Recommendation:** Extract the actual Prospect Opportunity field list from the CRM/OneNote and mirror it exactly in `output-template.md`.

### 10. No feedback loop
There's no mechanism for producers to rate an agent run or for the firm to learn from bad outputs.

**Recommendation:** See "Governance" section below.

### 11. Under-100-EE threshold is a hardcoded assumption
The spec calls out "under 100 EEs" as a flag. That's a firm-specific business rule that varies by region and practice.

**Recommendation:** Make the threshold configurable per producer or team (see `flags-and-rules.md`).

### 12. No timing gate to SDP handoff
The spec positions the agent as "before SDP is engaged" — but doesn't define when the agent's output is complete enough to trigger handoff.

**Recommendation:** Add a Readiness Assessment section (built into the output template) that explicitly says "Ready for SDP: YES / NOT YET" and lists what must be closed first.

---

## V2 feature ideas

Not needed for launch. Consider in order of impact.

### V2.1 — Post-call debrief mode
After the discovery call, the producer feeds in notes; the agent produces an updated Specs Sheet, a revised readiness assessment, and a clean SDP intake package. Closes the loop between discovery and handoff.

### V2.2 — Draft SDP intake package
The output already has the raw material. One more prompt turn produces the actual SDP intake form filled in, ready for review.

### V2.3 — Renewal-cycle sentinel
For prospects that aren't ready this year, park them and re-run the agent 90 days before their next renewal window. Turns "not now" into "queued."

### V2.4 — Cohort briefings
For a producer working a vertical push (e.g., "we're targeting Illinois manufacturers this quarter"), run the agent across a list of 20 prospects and produce a cohort brief highlighting the most workable 5 plus the vertical trends across all of them.

### V2.5 — Peer benchmarking
Given the prospect, retrieve depersonalized data on peer clients the firm has served — plan design, funding, savings achieved — as an internal-only reference for the producer.

### V2.6 — Employee sentiment sub-agent
The orphaned line in the original spec. Given a prospect, pull public employee sentiment from Glassdoor, Indeed, LinkedIn, review sites, and produce a summary. INTERNAL-only; do not surface externally without HR-legal review.

### V2.7 — Live-brief mode
Instead of a static output, the agent joins the producer in a chat interface during pre-call prep and answers real-time questions ("what does their most recent 10-K say about workforce?", "who else on their exec team should I be looking at?").

### V2.8 — Handoff receipts
When SDP picks up the account, the agent produces a receipt showing what data was passed vs. what was inferred vs. what's still unknown, so SDP starts with an accurate picture.

---

## Governance

### Confidentiality and data boundaries
- The agent reads Producer Inbox and SharePoint. Confirm with IT/security what tenant boundaries apply, and log every tool call so an audit trail exists.
- Never send inbox or SharePoint content out of the tenant. If the model provider offers a "no training on your data" tier (Anthropic, OpenAI, and Google all do at enterprise), use it and confirm in writing.
- Producers should not paste agent output into client-facing documents without stripping INTERNAL-tagged items. Build that into training.

### Prompt versioning
- The system prompt is going to change as you learn what works. Version it. `system-prompt.md` should carry a `## Version` header once you have more than one revision, and old versions should be archived, not deleted.
- Every production change to the prompt should be reviewed by at least one producer who actually uses the agent.

### Producer training
- The agent is only as good as the input. A 30-minute training session on "how to fill in the input template well" will pay for itself in a week.
- Distribute `example-run.md` — producers who see a good output produce better inputs.

### Escalation for BLOCKER flags
- When the agent emits a `BLOCKER` (pipeline overlap, active client, restricted vertical), define who receives the escalation and how quickly. Don't let it sit in the producer's inbox.

### Content of the discovery question bank
- The starter bank in `discovery-question-bank.md` is intentionally generic. Replace with the SDP-team-owned OneNote content, then keep them in sync. If OneNote is the source of truth, consider making the agent read from OneNote directly rather than maintaining a copy.

---

## How to measure whether this is working

Vanity metric: "35 producers use the agent." Actual metrics:

### Adoption
- % of new prospects that have an agent run before the first discovery call
- Median producer satisfaction score per run (1-5 thumbs, one click)

### Quality
- % of agent outputs that trigger a producer edit before being used (edit rate — lower is better *if* trust is warranted, but a zero edit rate means producers are rubber-stamping)
- % of `BLOCKER` and `HIGH` flags that turned out to be right in retrospect
- Rate of "the agent missed something material" as reported by SDP on handoff

### Deal impact
- Win rate on prospects that had an agent run vs. baseline
- Time from first call to SDP handoff (should shrink)
- Producer NPS on the tool at 30, 90, 180 days

### Cost
- Tokens per run (watch out — the tool-use loop can get expensive; SharePoint and Inbox searches especially)
- Dollar cost per run and per producer per month
- Cost per won deal that touched the agent

### Failure modes to watch for
- **Producers stop reading the output because it's too long.** Enforce the 2-4 page budget in the system prompt.
- **Producers copy INTERNAL content into client-facing docs.** Discipline through training + visual tagging; consider a separate scrubbed output for v2.
- **The agent invents incumbent-broker names.** Enforceable: any `[Web]` or `[Inferred]` tagged claim about incumbent must be verified on the discovery call before it feeds a proposal.
- **35 producers = 35 slightly different prompts.** Lock the system prompt centrally; don't let individual producers fork it.

---

## What I'd do first if I were you

1. Confirm the Prospect Opportunity field list and swap it into `output-template.md`. Everything downstream flows from that structure.
2. Replace `discovery-question-bank.md` with the actual OneNote SDP question set.
3. Confirm the flag thresholds in `flags-and-rules.md` reflect firm policy.
4. Pilot with 3 producers for two weeks. Read every output. Every edit they make is a signal.
5. After the pilot, publish the revised prompt and roll to the full 35.
6. Instrument the metrics above from day one — you cannot improve what you don't measure.
