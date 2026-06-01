---
name: gh-talent-context
description: >
  Maintains a living, current intelligence layer on your entire recruiting
  pipeline using Greenhouse data. Use this skill whenever a TA leader or
  recruiter asks about org-wide pipeline health, headcount status, role aging,
  source performance, recruiter throughput, or wants a snapshot of where hiring
  stands right now. Trigger for questions like "where do we stand on hiring",
  "what's our pipeline look like", "which roles are at risk", "how is [recruiter]
  performing", "what sources are working", or any time someone needs to understand
  the current state of talent across the org. This is your always-current talent
  context window — not a one-time report.
---

# Greenhouse Talent Context

Your ATS is a stream of signal, not a static database. This skill treats it
that way — pulling live data across jobs, applications, openings, sources, and
users to give you an accurate, current picture of where hiring stands.

**The core insight:** Most ATS reporting tells you what happened. This skill
tells you what is happening — and what needs your attention today.

---

## What This Unlocks

- Org-wide pipeline health in a single pass — stage distribution, volume by
  req, aging flags — without opening a single Greenhouse report
- Source attribution: which channels are producing candidates that actually
  move, not just apply
- Recruiter throughput: who owns what, and where candidates are stalling
- Opening-level headcount visibility: how many seats are actually open vs.
  how many reqs exist (these are often different)
- Aging intelligence: reqs that have been open too long, candidates who
  haven't moved in weeks, scorecards that are pending

---

## Key Endpoints

| Endpoint | What It Gives You |
|---|---|
| `list_jobs` | All open reqs, opened date, department, office, status |
| `list_openings` | Actual headcount openings per job — critical for accurate HC tracking |
| `list_applications` | Applications by job, stage, source, recruiter, status, date |
| `search_applications` | Filter applications by stage name, hiring manager, department |
| `list_application_stages` | Stage-level timestamps — enables true time-in-stage calculation |
| `list_sources` | Source catalog to map source IDs to readable names |
| `list_users` | Recruiter and hiring manager roster with IDs for filtering |
| `list_scorecards` | Scorecard completion status, submitted vs. pending |

---

## Core Prompts

Copy and run these as-is. They work without any modification.

---

**1. Org-Wide Pipeline Snapshot**
```
Pull all open jobs from Greenhouse. For each job, get the application count
by current stage and flag any req that has been open more than 90 days.
Group results by department. Surface: total open reqs, total active
candidates, and the top 3 roles with the most pipeline stall (candidates
not moving in 30+ days based on last_activity_at).
```

---

**2. Source Performance Report**
```
Pull all applications from the last 90 days. Cross-reference with the
sources list to map source IDs to names. For each source, calculate:
total applications, how many are still active vs. rejected, and how many
have reached interview stage or beyond. Rank sources by active pipeline
yield — not raw volume.
```

---

**3. Recruiter Load & Velocity Report**
```
Pull all open jobs and their assigned recruiters. For each recruiter, show:
number of active reqs owned, total active candidates across those reqs,
and candidates who have had no activity in the last 14 days. Flag any
recruiter carrying more than [X] reqs or with more than [X] stale
candidates.
```

---

**4. Headcount Reality Check**
```
Pull all open jobs and their openings. For each job, compare the number
of open openings to the application pipeline. Surface any job where
openings are open but the active pipeline has fewer than 3 candidates
at or beyond recruiter screen stage. These are your at-risk roles.
```

---

**5. Weekly Hiring Pulse**
```
Give me a weekly hiring pulse. Pull: (1) new applications in the last 7
days by job, (2) candidates who moved forward in the last 7 days by job,
(3) scorecards submitted in the last 7 days, (4) any offers sent or
resolved in the last 7 days. Format as a brief executive summary with
a "needs attention" flag for any role with zero movement.
```

---

## Power Prompts

These need one piece of context filled in — noted in brackets.

---

**Pipeline Review Prep for [Executive/HM Name]**
```
I have a pipeline review with [NAME] who owns [TEAM/DEPARTMENT]. Pull all
open jobs in their department. For each role show: days open, total
applicants, active candidates by stage, last candidate movement date,
and pending scorecard count. Format as a briefing doc I can share before
the meeting.
```

---

**Aging Pipeline Audit**
```
Pull all active applications across all open jobs. Flag candidates who
have been in the same stage for more than [X] days based on
last_activity_at. Group by job and stage. For each flagged candidate
show their name, current stage, recruiter, hiring manager, and days
since last activity. This is my stale pipeline cleanup list.
```

---

**Source-to-Stage Funnel for [Specific Source]**
```
Pull all applications attributed to [SOURCE NAME — e.g., LinkedIn,
Employee Referral, Agency]. Show how many reached each stage and
calculate the conversion rate from application to recruiter screen,
recruiter screen to interview, and interview to offer. Compare to
overall pipeline conversion rates across all sources.
```

---

## Honest Limitations

**Stage timestamps are available but indirect.** `list_application_stages`
returns entry timestamps per stage per application. For large pipelines,
computing precise time-in-stage requires paginating across many records —
ask your LLM to work through it in batches or filter to specific jobs first.

**`last_activity_at` is a proxy, not a timestamp.** It reflects the last
action logged in GH (note, email, stage move) — not necessarily meaningful
recruiter engagement. Use it for aging flags but validate outliers manually.

**Source data quality depends on your GH configuration.** If applications
are ingested via integrations without source tagging, they'll appear as
"No Source." Fix at the ATS configuration level, not the prompt level.

**Recruiter assignments must be set on the job.** If jobs don't have a
recruiter assigned in GH, recruiter-level filtering returns nothing. Surface
this as a hygiene issue (see `gh-ats-hygiene`).

---

## Build Your Org Layer

The prompts above work on any GH instance. To make them significantly more
accurate for your org — mapping department IDs to real team names, filtering
out template jobs, knowing which custom fields matter — build an `ops-assist`
skill on top of this one. That's where tribal knowledge lives.

See the `ops-assist` skill template for the scaffolding.
