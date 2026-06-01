---
name: gh-role-intelligence
description: >
  Delivers a complete intelligence picture on a single Greenhouse req — pipeline
  health, stage velocity, interviewer feedback, rejection patterns, and candidate
  status — in one pass. Use this skill whenever a recruiter or TA leader needs
  to deep-dive a specific role: pipeline review prep, hiring manager updates,
  "why is this req stuck" analysis, scorecard audits, or candidate-level stage
  tracking. Trigger for questions like "give me a full pipeline on [role]",
  "why is [req] not moving", "who's left in the [job] pipeline", "what are
  interviewers saying about candidates on [role]", "show me rejections on
  [req]", or any time a single job needs more than a surface-level count.
---

# Greenhouse Role Intelligence

Every req has a story. This skill reads it — pulling applications, stage
movement, scorecard feedback, rejection reasons, and time-in-stage data to
give you the complete picture on a single role.

**The core insight:** A pipeline count tells you how many. This skill tells
you why, where, and what to do next.

---

## What This Unlocks

- Full stage-by-stage pipeline breakdown for any single req
- Time-in-stage analysis: where candidates are actually stalling vs. moving
- Scorecard intelligence: what interviewers are saying, completion rates,
  and whether feedback patterns reveal a calibration problem
- Rejection analysis: why candidates are falling out and at which stage —
  the single most underused signal in most ATSs
- Source-to-stage funnel: which channels are producing candidates that
  actually convert, not just apply
- HM briefing docs: structured pipeline summaries ready to share before
  a pipeline review

---

## Key Endpoints

| Endpoint | What It Gives You |
|---|---|
| `list_jobs` / `search_jobs` | Job details, opened date, hiring team, custom fields |
| `list_applications` | All applications for the job with stage, status, source, dates |
| `list_application_stages` | Stage entry timestamps — enables true time-in-stage |
| `list_scorecards` | Interviewer feedback, overall recommendation, completion status |
| `list_scorecard_question_answers` | Granular per-question scorecard responses |
| `list_scorecard_candidate_attributes` | Focus attributes scored per candidate |
| `list_rejection_details` | Why each rejected candidate was rejected |
| `list_rejection_reasons` | Rejection reason catalog for your instance |
| `list_interviewers` | Who is scheduled/assigned to interview which candidates |
| `list_openings` | How many actual seats are open on this req |
| `list_interview_kits` | What interviewers are supposed to be evaluating |

---

## Core Prompts

Copy and run these as-is, replacing `[JOB NAME OR ID]`.

---

**1. Full Pipeline Breakdown**
```
Give me a complete pipeline breakdown for [JOB NAME OR ID]. Show all
active candidates by current stage. For each stage show: candidate name,
days in current stage (from application_stages entry timestamp), source,
and last activity date. Flag anyone who has been in the same stage more
than 14 days.
```

---

**2. Rejection Analysis**
```
For [JOB NAME OR ID], pull all rejected applications. Cross-reference
with rejection_details and rejection_reasons to show: total rejections
by reason, which stage most rejections are happening at, and whether
any rejection reason accounts for more than 30% of total. This is my
"where is the pipeline leaking" view.
```

---

**3. Scorecard Audit**
```
For [JOB NAME OR ID], pull all scorecards. Show: total submitted vs.
pending, overall recommendation breakdown (strong yes / yes / no / strong
no), and flag any interviewer with more than 2 pending scorecards. If
scorecard_question_answers are available, summarize the most common
themes in written feedback.
```

---

**4. Time-in-Stage Analysis**
```
For [JOB NAME OR ID], use application_stages timestamps to calculate
average time spent at each stage. Compare to the current active pipeline
to identify which stage is the current bottleneck. Flag any candidate
whose time in their current stage is more than 2x the average for
that stage.
```

---

**5. HM Pipeline Briefing**
```
I have a pipeline review for [JOB NAME OR ID] with the hiring manager.
Prepare a briefing doc with: (1) pipeline summary by stage, (2) top 3
candidates most likely to advance based on scorecard signals, (3) pending
actions needed from the HM or interviewers, (4) any rejection patterns
worth discussing, (5) recommended next steps. Keep it concise — this is
a talking document, not a report.
```

---

## Power Prompts

---

**Calibration Problem Detector**
```
For [JOB NAME OR ID], pull all scorecards with question_answers. Look
for: (1) high disagreement between interviewers on the same candidate,
(2) candidates with strong written feedback but low overall ratings or
vice versa, (3) any attribute being scored "no" consistently across
all candidates. Surface these as potential calibration issues to bring
to the hiring team.
```

---

**Source Quality Analysis for This Req**
```
For [JOB NAME OR ID], pull all applications with source data. For each
source calculate: application-to-screen conversion rate,
screen-to-interview conversion rate, and average time-in-pipeline for
candidates from that source. Rank sources by pipeline quality, not
application volume. Tell me where to invest sourcing effort.
```

---

**Re-Engagement Candidate List**
```
For [JOB NAME OR ID], pull all rejected or withdrawn applications from
the last 12 months. Filter for candidates who reached recruiter screen
or beyond before falling out. Cross-reference rejection reasons to
exclude hard disqualifiers. Return a list of candidates worth
re-engaging with their last stage, rejection reason, and days since
last activity.
```

---

## Honest Limitations

**Scorecard text quality varies by org.** If your interviewers write
one-word answers, `list_scorecard_question_answers` won't surface much.
The signal is only as good as the feedback culture.

**Time-in-stage requires pagination for large pipelines.** For reqs with
100+ applications, pulling all `application_stages` records takes multiple
API calls. Scope to active applications first for faster results.

**Interview kit content is separate from scorecard responses.** What
interviewers were *supposed* to evaluate (`list_interview_kits`) and what
they actually submitted (`list_scorecard_question_answers`) can diverge.
Comparing these is a powerful calibration signal but requires joining
two data sets.

**Rejection reasons are only as clean as your GH config.** If your team
uses a single catch-all reason ("Not a fit"), rejection analysis won't
tell you much. Use this as a forcing function to clean up your rejection
reason taxonomy.

---

## Build Your Org Layer

Add your stage names, custom fields (e.g., requisition type, priority
flag, target start date), and HM preferences to your `ops-assist` skill.
That lets you filter by your actual stage names instead of generic ones,
and surface the custom fields that matter for your pipeline reviews.
