# LaunchFlow

**Turn product launch meetings into customer-ready marketing campaigns.**

LaunchFlow is a Sitrep no-code agent. Sitrep hands it one launch-related task at a time — a product/service/feature name, plus a summary of the launch meeting it came from — and LaunchFlow returns a ready-to-review marketing campaign, built only from the details the team actually approved, posted directly to a dedicated Slack review channel.

Built for the [Sitrep AI Hackathon](https://joinsitrep.com/hackathon) — No-Code Track, **Managed** agent (built entirely in Sitrep Studio, no repo or server).

## Problem

After product launch meetings, marketing teams manually read through the meeting notes, extract the approved launch details, and rewrite them into customer communications. This delays launches and increases the risk of inconsistent or outdated messaging — a stale price, a benefit that was actually still under debate, a missing promo code or link.

## Solution

LaunchFlow analyzes the product launch meeting, extracts only the final approved launch information, and generates customer-ready marketing campaign drafts (SMS, email, or both) — then automatically posts them to a dedicated Slack review channel for approval.

## What LaunchFlow does

- Detects product launch meetings.
- Extracts only approved launch details.
- Ignores ideas that were discussed but not approved.
- Generates customer-facing campaign drafts (SMS, email, or both).
- Flags missing information instead of guessing.
- Returns a campaign ready for marketing review, posted to Slack.

## Workflow

```text
Product Launch Meeting
        ↓
Sitrep records & transcribes the meeting
        ↓
Sitrep produces a meeting summary + task
        ↓
LaunchFlow analyzes the meeting summary
        ↓
Extracts approved launch details
        ↓
Generates SMS and/or Email draft
        ↓
Posts draft to #marketing-review on Slack
        ↓
Marketing Manager reviews
        ↓
Approve / Edit
        ↓
Copy or send through the company's SMS/Email platform
```

Note: LaunchFlow works from the meeting **summary** Sitrep hands it, not the raw transcript — it never sees more of the conversation than that summary contains.

## Slack example

Channel: `#marketing-review`

```
🚀 Launch Campaign Ready for Review

Product: January Beta Bundle
Launch Date: 15 January
Audience: Prepaid Customers

SMS
🎉 Introducing the January Beta Bundle! Get 20GB for GMD 250.
Available from 15 January. Dial *123# to subscribe.

Email
Subject: Introducing the January Beta Bundle
Body: We're excited to announce the launch of the January Beta
Bundle...

⚠️ Missing Information
None

Status: 🟡 Pending Marketing Approval
```

This answers the question every judge (and every marketing manager) will have — *where does the output go?* Straight into the Slack channel the team already collaborates in, where they approve, edit, or copy it into their existing SMS/email platform.

**Input:** a product launch meeting. **Output:** a customer-ready launch campaign, waiting where marketing already works.

## Repo contents

| File | Purpose |
|---|---|
| [`prompt.txt`](prompt.txt) | The exact system prompt to paste into Sitrep Studio's agent builder |
| [`test-inputs.md`](test-inputs.md) | Two sample `{task, summary, attendees}` invocations matching Sitrep's real request contract, for validating the agent before publishing |
| [`expected-output-example.md`](expected-output-example.md) | Hand-derived reference output for both test inputs, to sanity-check the real agent's output |
| [`kaggle-writeup-draft.md`](kaggle-writeup-draft.md) | Draft of the Kaggle submission writeup |

## Sitrep configuration (Managed agent, Sitrep Studio)

Set these up after signing in at [joinsitrep.com](https://joinsitrep.com) → build a Managed agent in Sitrep Studio:

- **Name:** LaunchFlow
- **Tagline:** Turn product launch meetings into customer-ready marketing campaigns.
- **Category:** Marketing
- **Description:** Turns product launch meetings into ready-to-send SMS and email campaigns — built only from what the team actually approved, never a guess. Flags anything still unconfirmed instead of inventing it, and lands straight in your marketing team's Slack channel for approval.
- **System prompt:** contents of [`prompt.txt`](prompt.txt), pasted verbatim
- **Model:** the most capable model offered — extracting *only what was actually approved* from a meeting, across two channel formats, is a precision task, not a place to trade accuracy for a cheaper model
- **Temperature:** low, ~0.2 — low enough that the same approved details produce the same campaign consistently, with just enough room for natural marketing phrasing
- **Trigger:** route tasks coming from meetings tagged/categorized **"Product Launch"** (or "Go-to-Market") to this agent
- **Delivery:** connect the Slack integration and set the output destination to `#marketing-review`, so each generated draft posts there directly instead of only appearing on the task board

## Validating before publishing

1. Paste `prompt.txt` into the system prompt field in Sitrep Studio.
2. Run the agent's test/preview feature against both examples in `test-inputs.md`.
3. Compare against `expected-output-example.md`: Example 1 (Pro Plan Autumn Discount, channel unspecified) should come back with **both** an SMS and an email draft, "Missing Information: None," status Pending Marketing Approval. Example 2 (Weekend Flash Sale, email explicitly requested) should come back with **no invented discount or date** — Missing Information should name both, and no email draft should be produced.
4. If the model ever fills in a price, discount, or date that wasn't actually finalized in the meeting, or drafts a channel that wasn't requested, tighten the temperature or re-emphasize the relevant instruction in the prompt.

## License

MIT — see [LICENSE](LICENSE).
