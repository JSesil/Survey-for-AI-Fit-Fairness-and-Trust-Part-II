# AI Agent Deployment Decision Framework

*A tool for deciding whether, where, and how autonomously to deploy an AI agent for a given workflow — before you buy a platform or write a policy.*

---

## How to use this

Run each candidate workflow through the framework in order:

1. **Screen** — Score the workflow on four risk dimensions.
2. **Tier** — The score places it in an autonomy tier, which sets the required guardrails.
3. **Baseline** — Capture the metrics you'll need to judge ROI *before* you deploy anything.
4. **Gate** — Use the pilot-to-scale checkpoints to decide whether to expand, hold, or kill.

This is meant to sit *upstream* of any governance platform or orchestration tool. It answers "should we, and how tightly do we control it," not "which vendor."

---

## Step 1: Screen the workflow

Score each dimension 1 (low risk) to 4 (high risk). Be honest about the worst realistic case, not the average one.

| Dimension | 1 — Low | 2 | 3 | 4 — High |
|---|---|---|---|---|
| **Reversibility** | Fully undoable (a draft, a suggestion) | Undoable with minor effort | Costly or slow to undo | Irreversible (payment sent, offer extended, data deleted, person hired/fired) |
| **Blast radius** | Affects the agent's own output only | Affects one internal team | Affects external customers/partners at small scale | Affects many people at once, or affects a legally protected decision (hiring, credit, healthcare, pay) |
| **Detectability of error** | A human reviews every output before it matters | Errors surface quickly through normal monitoring | Errors could persist for days/weeks before anyone notices | Errors could compound silently (e.g., agent-to-agent handoffs, no natural checkpoint) |
| **Data/regulatory sensitivity** | Public or synthetic data | Internal, non-sensitive data | PII, financial, or employee data | Legally regulated data or decisions (EEOC, GDPR, EU AI Act "high-risk" categories, health data) |

**Total score: ___ / 16**

---

## Step 2: Assign an autonomy tier

| Score | Tier | What it means |
|---|---|---|
| 4–6 | **Tier 1 — Autonomous** | Agent acts and reports afterward. Spot-check sampling is sufficient oversight. |
| 7–9 | **Tier 2 — Supervised** | Agent acts, but a human reviews outputs on a defined cadence (daily/weekly digest), not in real time. |
| 10–12 | **Tier 3 — Approval-gated** | Agent prepares/recommends; a human must explicitly approve before any action executes. No exceptions. |
| 13–16 | **Tier 4 — Advisory only / do not automate** | Agent may inform human judgment but should not be given execution authority. Revisit only if you can redesign the workflow to lower the score (e.g., add a mandatory checkpoint, reduce blast radius by scoping the agent narrower). |

A workflow that scores Tier 4 today isn't necessarily off-limits forever — it's a signal to redesign the workflow (smaller scope, added checkpoint, human-in-the-loop by default) rather than to buy a more sophisticated agent.

---

## Step 3: Set guardrails by tier

| Requirement | Tier 1 | Tier 2 | Tier 3 | Tier 4 |
|---|---|---|---|---|
| Human approval before action | No | No (post-hoc review) | **Yes, always** | N/A — no execution authority |
| Audit log of every action | Yes | Yes | Yes | Yes |
| Rollback/kill-switch tested before launch | Recommended | Yes | Yes | N/A |
| Named human owner accountable for the agent | Yes | Yes | Yes | Yes |
| Bias/fairness testing before launch | If decision-adjacent | Yes | Yes | Yes |
| Review cadence | Quarterly | Monthly | Every release/change | N/A |
| Formal governance platform registration (Credo AI, watsonx.governance, etc.) | Optional | Recommended | **Required** | Required if used at all |

---

## Step 4: Baseline before you deploy (this is the ROI step)

The single biggest failure mode organizations report is judging ROI from vendor claims instead of their own before/after data. Capture these **before** the agent goes live:

- **Time**: hours spent on the task per week, measured directly (not estimated)
- **Volume**: how many instances of this task occur per week/month
- **Error rate**: current error or rework rate, and how errors are currently caught
- **Cost of an error**: rough dollar or reputational cost if this task is done wrong
- **Cycle time**: how long from task start to completion today

Re-measure the same five metrics 30, 60, and 90 days after launch. If a vendor's dashboard reports different numbers than your own baseline, trust your own.

---

## Step 5: Pilot-to-scale gate

Before expanding a pilot beyond its original scope, answer all four:

- [ ] Did the tier-appropriate guardrails hold in practice (no approval bypassed, no untracked action)?
- [ ] Did the 30/60/90-day metrics show a real improvement over baseline — not just adoption or usage volume?
- [ ] Has at least one near-miss or error been caught by the guardrails, and was it caught *before* it caused harm?
- [ ] Is there still a single named human owner who can explain what the agent does and why, in plain language, without checking a dashboard?

Any "no" is a hold, not a kill — fix the specific gap and re-test rather than scaling around it.

---

## Quick reference: red flags that should stop a deployment regardless of score

- No one can name who is accountable if the agent makes a consequential mistake.
- The workflow touches a legally protected decision (hiring, pay, credit, healthcare eligibility) and there is no human sign-off step.
- The only evidence of ROI is a vendor's own reported statistics.
- The agent hands off to other agents with no human checkpoint anywhere in the chain.
