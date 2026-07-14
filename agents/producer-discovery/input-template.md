# Input Template — Producer Discovery Agent

Copy this block, fill in what you know, paste into the agent. Leave `Unknown` for anything you don't have — the agent handles missing fields. Sections marked **Required** must have a real value.

```
=== PROSPECT DISCOVERY REQUEST ===

--- Identity ---
Prospect legal name:            [Required]
DBA / trade name:               [Unknown / value]
EIN (if known):                 [Unknown / value]
Website URL:                    [Required]
Headquarters city, state:       [Unknown / value]

--- What we already have ---
Form 5500 attached?             [Yes — attached / Yes — please pull / No]
Benefit Flow record ID or URL:  [Unknown / value]
Past Lockton engagement?        [None known / Yes — describe below]
  Details:                      [text]

--- Deal context ---
Deal source:                    [Warm intro / RFP / Cold outbound / Renewal opportunity / PE portfolio play / Other]
Trigger — why now:              [text — what event or moment created this opportunity]
Timing pressure:                [text — RFP due date, renewal date, no deadline, etc.]
My hypothesis on the deal:      [text — 1-3 sentences on what I think is going on and why we'd win]

--- Contacts I know ---
Primary contact:                [Name, title, email — or Unknown]
Other contacts:                 [Name, title, relationship — one per line, or Unknown]
Referrer (if applicable):       [Name, firm, relationship to prospect]

--- What I specifically want from this run ---
Top question I need answered:   [text — the one thing that would change how I work this deal]
Anything I want you to SKIP:    [text — e.g., "don't search Producer Inbox, it's a fresh account"]

=== END ===
```

## Notes on filling this in

- **"My hypothesis on the deal"** is the highest-leverage field. Without it, the agent produces generic output. Even a bad hypothesis is useful — the agent can pressure-test it.
- **"Trigger — why now"** matters more than most producers realize. Renewal dates, M&A activity, benefits complaints, leadership changes, and RFP releases all change the emphasis of the brief.
- **"Top question I need answered"** anchors the agent's priorities. If you leave it blank, you'll get a well-rounded brief; if you fill it in, you'll get a brief that leads with your answer.
- **"Anything I want you to SKIP"** is a real feature. Common uses: skip Producer Inbox if you know it's empty, skip SEC EDGAR if the prospect is private and small, skip industry news if you're an expert in the vertical already.
