# Pulse — AI-Powered Feedback Intelligence

Pulse turns raw user feedback into ranked product priorities. It collects
feedback across sources, classifies it with AI, tracks it through resolution,
and answers the only question that matters at triage time: **what should the
team fix first — and why?**

**Live demo:** https://darsh-pm.github.io/Feedback-tracker/ (one-click demo
mode, no signup required)
**Case study:** see the Pulse case study on my portfolio for the full product
narrative, key decisions, and tradeoffs.

## The problem

Every team collects feedback; almost none convert it into decisions at a
regular cadence. A crash report, an enterprise feature request, and a vague
onboarding complaint land in the same inbox — and the loudest item wins, not
the most important one. The decision step (weighing impact, urgency, and
frequency across items) stays manual, slow, and dependent on whoever runs the
spreadsheet.

## What it does

| Stage | Capability |
|---|---|
| **Collect** | Log feedback by source (interviews, tickets, surveys, app reviews) with AI triage: paste raw text and the model suggests a title, sentiment, and category — the human confirms or overrides before saving. |
| **Analyze** | Dashboard shows the live shape of the feedback: sentiment split, category breakdown, status counts. The Insights view tracks how that shape moves — sentiment trend, volume, and category mix by week. |
| **Prioritize** | On-demand AI analysis returns a ranked "prioritize first" list with written rationale per item, an explicit "can wait" list, top pain points by severity, and what users love. Every recommendation links back to its source feedback. |
| **Act** | Status workflow (open → in progress → addressed) with search, multi-filter, sort, and bulk updates. The analysis knows when it's stale: change the active feedback set and it flags itself for a re-run. |

## Product principles

- **AI recommends, the human decides.** The model never changes a status,
  closes an item, or saves a classification without review.
- **Every recommendation carries its evidence.** Ranked items click through
  to the underlying feedback. A ranking without reasons is a black box; a
  ranking with reasons is a conversation starter.
- **Fixed category taxonomy** (UX, Performance, Feature Request, Bug, Other)
  over model-generated categories — less nuance per item, but breakdowns stay
  comparable week over week. Consistency beats cleverness for trend data.
- **Insights are sentences, not just charts.** A rule-based signal engine
  turns the trend data into PM-readable callouts: category momentum,
  sentiment shifts, ageing backlog, loudest negative channel.

## Architecture

```
ingest → classify (AI-assisted) → store → aggregate → analyze → rank
```

- **React** (via CDN, deliberately no build step) — dashboard, inbox,
  insights, and the analysis panel. State stays thin; stored data owns truth.
- **Supabase** — PostgreSQL + Auth with Row Level Security for per-user data
  isolation.
- **Groq (Llama 3.3 70B)** — called through a Supabase Edge Function proxy so
  the API key never reaches the client. Powers both intake classification and
  the structured priority analysis.
- **Analytics** — weekly bucketing, trend charts (hand-rolled SVG, zero chart
  dependencies), resolution metrics, and signal generation all computed
  client-side from the feedback data.
- **Persistence of analysis** — the latest AI analysis is kept per user with
  a fingerprint of the feedback set it analyzed, enabling staleness detection
  without re-running inference on every view.

## Try it

Open the live demo and click **Try Demo** — it loads ~10 weeks of realistic
feedback so the trends, signals, and AI analysis have something meaningful to
chew on. Or sign up for a private workspace backed by Supabase RLS.

## Roadmap

- Source integrations (forms, support tools) to remove manual entry
- Analysis history — compare this week's priorities against last week's
- CSV export of feedback and analysis output
