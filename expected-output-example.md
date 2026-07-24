Reference outputs for `test-inputs.md`, hand-derived by applying `prompt.txt`. Use these to sanity-check the real agent's output once configured in Sitrep Studio — it shouldn't match word for word, but the channel(s) produced, the fields extracted, and what gets flagged under Missing Information should line up.

---

## Example 1 — Pro Plan Autumn Discount (channel unspecified → expect both SMS and Email, no missing info)

# 🚀 Launch Campaign Ready for Review

**Product:** Pro Plan Autumn Discount
**Launch Date:** September 15
**Audience:** Existing Basic Plan subscribers

**SMS**
🎉 Upgrade to Pro and save 20%! Just $39/mo for 3 months. Unlock analytics + priority support. Ends Oct 15. Use code AUTUMN20.
*Character count: ~130/160 — fits in one segment*

**Email**
**Subject:** Save 20% on Pro — your Autumn upgrade is here
**Body:** Hi there — for a limited time, upgrade from Basic to Pro for just $39/month (regularly $49) for your first 3 months. You'll unlock advanced analytics and priority support right away. This offer is available now through October 15. Use code AUTUMN20 at checkout to claim your discount.

**⚠️ Missing Information**
None

**Status:** 🟡 Pending Marketing Approval

---

## Example 2 — Weekend Flash Sale (email requested → expect missing price/date flagged, no draft forced)

# 🚀 Launch Campaign Ready for Review

**Product:** Weekend Flash Sale
**Launch Date:** Not confirmed
**Audience:** All customers

**Email:** Not generated — the offer's discount percentage was never finalized, so a customer-ready email can't be produced yet.

**⚠️ Missing Information**
- Price/discount — still pending finance sign-off between two options (15% or 20%), no final figure.
- Launch date — never agreed; the item was tabled for a future meeting.

**Status:** 🟡 Pending Marketing Approval

---

Both examples pull from the same shared meeting summary (see the note at the bottom of `test-inputs.md`) — the important thing to verify is that LaunchFlow correctly isolates Pro Plan Autumn Discount's approved details in Example 1 (producing both channels since none was specified, "Missing Information: None") and Weekend Flash Sale's unresolved details in Example 2 (producing only the requested email — but withheld, with the specific gaps named, rather than filled with an invented discount).
