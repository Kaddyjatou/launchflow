# LaunchFlow

**Sitrep Agent URL:** https://app.joinsitrep.com/dashboard/marketplace/launchflow--50745813-3448-48cc-a432-b13b718affdf
**GitHub Repo:** [paste your public repo link here]

## Inspiration

Every product, feature, or promotion launch follows the same pattern: a launch meeting locks in the name, price, date, and offer — and then someone still has to manually turn those decisions into the actual campaign customers receive. That step is repetitive, time-sensitive, and an easy place for a stale or unapproved detail to slip into a real customer message. I wanted to build the simplest version of that missing link: product launch meeting in, launch campaign out — for any company running launch meetings, not just one industry.

## What it does

LaunchFlow turns a product launch meeting straight into a launch campaign. It reads the meeting summary and drafts a ready-to-review campaign — SMS, email, or both — using only the launch details the team actually finalized. It never invents a detail that was only proposed, still under debate, or requested by a task instruction without being confirmed in the meeting itself. If something core was never locked down, it says so plainly and names exactly what needs confirming. The draft posts directly into the marketing team's Slack review channel — a manager reviews it, edits if needed, and sends it through the company's own email/SMS platform.

## How I built it

This is a No-Code Sitrep agent — one carefully engineered system prompt, no hosting. Key decisions:

- **Approved vs. proposed.** A hard line between what the meeting decided and what was merely discussed, since a launch meeting often covers an undecided item in the same breath as an approved one.
- **Channel selection follows the task, not a default.** A specific channel if asked; both if not.
- **Format-aware.** The 160-character SMS segment is a real constraint, not a suggestion; email gets its own subject-line and body rules.
- **Task instructions don't bypass grounding.** A task can ask for a discount or testimonial to be included — but only the meeting summary can confirm a fact. This turned out to be one of the most important rules in the whole prompt (more below).
- **Low temperature (~0.2)** for consistency without sounding like a form letter.

## Challenges I ran into

The real challenge wasn't writing the first draft — it was testing it and discovering how many ways "don't invent" quietly breaks. Running real test cases through Sitrep Studio surfaced concrete failures a purely theoretical review would have missed:

- A task explicitly asked the agent to "include a limited-time discount," and the SMS invented one — while Missing Information *simultaneously* flagged that same discount as unconfirmed. Two contradictory statements in one output.
- A launch date described as "penciled in, pending final sign-off" still showed up plainly in the SMS as a settled fact.
- A stated benefit ("sustainable features") got dropped and wrongly marked missing, while an actual missing detail ("visit our website") got invented into the message anyway.
- A vague-but-real launch got fully rejected instead of drafted with gaps flagged — an over-correction in the opposite direction.

Each needed a different fix: a worked example showing the exact wrong-vs-right output (prose rules alone didn't reliably stop the model from complying with a task's factual request over the meeting's actual facts), an explicit rule that hedged language applies to every field including the header date, and a final self-check step requiring every claim to be verified against the summary one at a time before responding. The pattern across all of them: this agent's entire value is in what it *refuses* to say, and that discipline had to be tested adversarially, not just written once and trusted.

## Accomplishments that I'm proud of

I'm proud that this held up under real, adversarial testing instead of just looking good on paper. It's one thing to write a prompt that says "never invent a fact" — it's another to actually catch a model fabricating a discount it was explicitly asked to include, and fix it so it stops. That gap between intention and verified behavior is what separates a demo from something a business could trust with real customer messages.

## What I learned

Testing an agent like a skeptic, not a fan, is what actually makes it reliable. Every bug I found came from deliberately trying to break the "don't invent" rule, not from writing a nicer-sounding instruction. I also learned that concrete worked examples fix model behavior more reliably than abstract principles — telling the model exactly what the wrong output looks like next to the right one closed gaps that a well-reasoned paragraph didn't.

## What's next

- **Multi-language campaign variants** for companies serving customers in more than one language.
- **Direct integration with marketing platforms** for one-click publishing after approval — a natural Code Track extension that removes the manual copy-paste step entirely.
