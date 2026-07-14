# Producer Discovery Agent

A pre-kickoff research assistant for business development producers. The producer feeds in what they know about a prospect; the agent returns a Prospect Specs Sheet, a ranked set of discovery questions, industry insights, immediate flags, and a readiness assessment for handoff to the Strategic Discovery Process (SDP) team.

## Audience & timing

- **Who runs it:** Producer (BD).
- **When:** After a prospect is identified, before the kickoff call, and — ideally — before SDP is engaged.
- **Why:** So the producer walks into the first call already familiar with the account, and so SDP receives a clean, structured brief instead of raw inputs.

## What's in this folder

| File | Purpose |
|---|---|
| `system-prompt.md` | The agent's operating instructions. Drop into a Claude Project system prompt, an API app, or wherever the model runs. |
| `input-template.md` | The structured form the producer fills in to kick off a run. |
| `output-template.md` | The exact shape of what the agent returns. Sections mirror the Prospect Opportunity record so results copy-paste cleanly. |
| `tool-specifications.md` | The integrations the agent expects to have (SharePoint, Benefit Flow, 5500 lookup, Producer Inbox, LinkedIn, SEC EDGAR, web). Defines what each tool does, when to reach for it, and what to extract. |
| `discovery-question-bank.md` | Starter library of SDP-aligned discovery questions, organized by topic and vertical. Meant to be replaced/augmented from the OneNote source of truth. |
| `flags-and-rules.md` | Heuristics that trigger immediate flags (missing 5500, sub-100 EE headcount, vertical restrictions, etc.). |
| `example-run.md` | Worked example — hypothetical prospect walked from input to output — so producers see what "good" looks like. |
| `improvements-and-roadmap.md` | Design gaps in the current spec, v2 enhancements, governance recommendations, and metrics for evaluating output quality. |

## Deployment options

Pick one; the files support all three.

1. **Claude.ai Project (fastest).** Paste `system-prompt.md` as the Project's custom instructions. Attach `discovery-question-bank.md`, `flags-and-rules.md`, and `tool-specifications.md` as Project knowledge. Share with the 35 producers. Web search + connectors (SharePoint, Outlook, Google Drive) get wired at the Project level.
2. **Custom API app.** Use `system-prompt.md` as the system message. Implement each tool in `tool-specifications.md` as an actual tool function. Wrap in whatever UI you prefer (Streamlit, Retool, internal portal).
3. **Model-agnostic.** The prompt and templates are written to work with any capable frontier model; nothing in here is Claude-specific.

## Before you ship — customization checklist

The prompt has been written against a set of reasonable assumptions. Confirm or correct these before rolling out:

- [ ] **"SDP" definition.** The prompt assumes SDP = Strategic Discovery Process, an internal handoff team that runs deep discovery after the producer's initial call. Correct in `system-prompt.md` if that's wrong.
- [ ] **Prospect Opportunity sections.** The output template mirrors a generic Prospect Opportunity structure (Company Profile, Financials, Current Benefits, Decision Makers, Sales Process, Competition, Next Actions). Replace with the actual field list from your CRM/OneNote.
- [ ] **Market-segment threshold.** `flags-and-rules.md` flags any prospect under 100 EEs as below the standard middle-market cutoff. Adjust if your firm's threshold differs, or make it configurable per producer/region.
- [ ] **Vertical restrictions.** `flags-and-rules.md` lists common restricted/complex verticals (cannabis, adult entertainment, gambling, high-hazard manufacturing). Replace with your firm's actual appetite.
- [ ] **Discovery question bank.** `discovery-question-bank.md` is a starter set. Replace with the SDP question library from OneNote so producers see the questions their SDP colleagues actually use.
- [ ] **Tool availability.** `tool-specifications.md` describes tools as if they all exist. Mark unavailable ones so the agent doesn't hallucinate a call.
- [ ] **Confidentiality boundaries.** Confirm that the agent may read the Producer Inbox and SharePoint. Add any specific document sets that are off-limits (e.g., HR, legal, other producers' pipelines).
- [ ] **Feedback capture.** Decide how producers will rate output quality so you can improve the prompt over time (see `improvements-and-roadmap.md`).
