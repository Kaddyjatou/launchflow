Two sample invocations matching Sitrep's real request contract (`{task, summary, attendees}`), covering the two outcome branches and the channel-selection logic. Use these to sanity-check LaunchFlow in Sitrep Studio's test/preview feature before publishing — paste the `task` + `summary` + `attendees` values into whatever test inputs the Studio UI exposes.

---

## Example 1 — channel unspecified, expect both SMS + Email drafted

```json
{
  "task": {
    "id": "task_501",
    "title": "Customer campaign for Pro Plan Autumn Discount",
    "description": "Prepare the customer launch campaign for the Pro Plan Autumn Discount."
  },
  "summary": "Product launch meeting, September 2. The team reviewed two items. Pro Plan Autumn Discount: finalized as 20% off the Pro Plan for the first 3 months. Launch date confirmed as September 15. Price confirmed at $39/month (down from $49). Offer targets existing Basic Plan subscribers only. Key benefit: unlocks advanced analytics and priority support. Promotional window confirmed to run until October 15. Customers redeem with the code AUTUMN20 at checkout. Everyone in the room signed off on these details. Weekend Flash Sale: raised as a possible follow-up promotion for all customers, offering a discount on annual plans, but the discount percentage was not settled — two options (15% or 20%) are still pending finance sign-off, and no launch date was agreed. That item was tabled for a future meeting.",
  "attendees": [
    { "id": "u1", "name": "Maria (Marketing Lead)" },
    { "id": "u2", "name": "Chen (Product Manager)" },
    { "id": "u3", "name": "Priya (Finance)" }
  ]
}
```

## Example 2 — email explicitly requested, expect unconfirmed price flagged

```json
{
  "task": {
    "id": "task_502",
    "title": "Launch email for Weekend Flash Sale",
    "description": "Prepare the customer launch email for the Weekend Flash Sale."
  },
  "summary": "Product launch meeting, September 2. The team reviewed two items. Pro Plan Autumn Discount: finalized as 20% off the Pro Plan for the first 3 months. Launch date confirmed as September 15. Price confirmed at $39/month (down from $49). Offer targets existing Basic Plan subscribers only. Key benefit: unlocks advanced analytics and priority support. Promotional window confirmed to run until October 15. Customers redeem with the code AUTUMN20 at checkout. Everyone in the room signed off on these details. Weekend Flash Sale: raised as a possible follow-up promotion for all customers, offering a discount on annual plans, but the discount percentage was not settled — two options (15% or 20%) are still pending finance sign-off, and no launch date was agreed. That item was tabled for a future meeting.",
  "attendees": [
    { "id": "u1", "name": "Maria (Marketing Lead)" },
    { "id": "u2", "name": "Chen (Product Manager)" },
    { "id": "u3", "name": "Priya (Finance)" }
  ]
}
```

Note both examples share the same meeting `summary` (as would happen in reality — Sitrep would call LaunchFlow once per task extracted from the same launch meeting) but differ in which `task` is being asked about, and Example 2 explicitly names the channel. That combination — right item, right channel, no invented figures — is exactly what LaunchFlow has to get right.

## Example 3 — channel unspecified, fully approved, automatic redemption (no code to invent)

```json
{
  "task": {
    "id": "task_403",
    "title": "Customer campaign for Buy & Get Data Bonus promotion",
    "description": "Prepare the customer campaign for the Buy & Get Data Bonus promotion."
  },
  "summary": "Marketing promotion meeting, September 20. The team finalized the \"Buy & Get Data Bonus\" promotion: customers who top up with GMD 100 of airtime credit in a single purchase automatically receive 1GB of free data. Launch date confirmed as October 1. Offer targets all prepaid customers. Promotional window confirmed to run for the whole month, ending October 31. No manual redemption is needed — the bonus data is credited automatically within 24 hours of the qualifying top-up. Everyone in the room approved these details.",
  "attendees": [
    { "id": "u1", "name": "Fatou (Marketing Lead)" },
    { "id": "u2", "name": "Ousman (Product Manager)" }
  ]
}
```

Since no channel is named in the task, this should draft **both** SMS and Email. It also tests something Examples 1 and 2 don't: a fully-approved offer with **no redemption code at all**, because the mechanism is automatic. The risk here is a model that reflexively invents a dial/promo code out of habit (a common telecom SMS pattern) instead of accurately reporting that no action is needed. A correct response drafts both channels, keeps the SMS near 160 characters, states in both that the bonus is automatic, and shows "Missing Information: None" since every field was actually confirmed.
