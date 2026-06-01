---
name: gh-compound-workflows
description: >
  Orchestrates Greenhouse data with other connected tools — interview platforms,
  HRIS, Slack, email, benchmarks, calendar — to answer questions no single
  system can answer alone. Use this skill when a TA leader needs to cross-
  reference ATS pipeline data with interview notes, compensation data, market
  benchmarks, recruiter communications, or hiring manager context. Trigger for
  questions like "why is this candidate stuck", "prep me for this debrief",
  "compare our pipeline to market benchmarks", "give me a full candidate
  dossier", "cross-reference what the interviewer said with where the candidate
  is in the pipeline", or any workflow where Greenhouse is one input but not
  the whole answer. This is the highest-ceiling skill in the set — it requires
  more connected tools but produces intelligence that no dashboard can replicate.
---

# Greenhouse Compound Workflows

Greenhouse tells you what happened in your pipeline. The answers to *why*
and *what to do next* live in the intersection of your ATS and everything
else — interview notes, market data, recruiter emails, hiring manager Slack
threads, comp bands.

**The core insight:** Your LLM can hold all of these contexts simultaneously
and reason across them. No human can do this as fast or as consistently.
That's the compound advantage.

---

## What This Unlocks

- Pre-debrief candidate intelligence: ATS stage data + interview notes +
  scorecard feedback assembled into a decision-ready brief
- Market-calibrated pipeline analysis: your pipeline conversion rates
  benchmarked against industry data
- Candidate re-engagement lists with context: not just "who fell out" but
  what interviewers said and whether the role has changed since they did
- Recruiter morning briefing: overnight application volume + stage
  movement + calendar context in one pass
- Offer intelligence: ATS offer data + comp bands + candidate
  expectations synthesized before the offer conversation
- Cross-tool root cause analysis: "why is this req stuck" answered by
  pulling GH pipeline + interview notes + HM communication patterns

---

## Workflow Patterns

Each workflow below names the tools it connects. You need those tools
connected and accessible for the workflow to run. GH MCP is assumed
for all of them.

---

### Pattern 1: Pre-Debrief Candidate Brief
**Tools:** Greenhouse MCP + Interview notes tool (Metaview, Grain, etc.)

```
I have a debrief for [CANDIDATE NAME] on [JOB] in [X] minutes. Pull
their application from Greenhouse: current stage, source, days in
pipeline, and all scorecard submissions with overall recommendations.
Then pull their interview transcripts or notes from [INTERVIEW TOOL].
Synthesize into a 1-page brief: (1) where they are in the process,
(2) what interviewers have said so far, (3) any disagreements between
interviewers, (4) open questions that should drive the debrief,
(5) my recommended next step.
```

---

### Pattern 2: Recruiter Morning Briefing
**Tools:** Greenhouse MCP + Calendar + Email/Slack

```
Give me my recruiting morning briefing. From Greenhouse: (1) new
applications in the last 24 hours by job, (2) any candidate who moved
stages yesterday, (3) pending scorecards due today. From calendar:
interviews scheduled for today across all reqs. From email/Slack: any
candidate communications that need a response. Format as a prioritized
action list — what needs to happen today, in order.
```

---

### Pattern 3: Market-Calibrated Pipeline Review
**Tools:** Greenhouse MCP + Benchmark data source

```
Pull the pipeline for [JOB NAME]. Calculate our conversion rates at
each stage: application-to-screen, screen-to-interview, interview-to-
offer. Compare these to industry benchmarks for [ROLE TYPE] roles.
For any stage where we're converting at less than 70% of benchmark,
identify the likely cause based on rejection reasons and scorecard
patterns. Give me a "where we're losing vs. where the market loses"
breakdown.
```

---

### Pattern 4: Offer Intelligence Brief
**Tools:** Greenhouse MCP + Comp data + Email/Calendar

```
I'm preparing an offer for [CANDIDATE NAME] on [JOB]. From Greenhouse:
pull their full application history, any comp expectations captured in
notes or custom fields, and the offer record if one exists. From comp
data: pull the band for this role and level. Cross-reference with any
email or calendar context from our recent conversations with this
candidate. Output: recommended offer structure, risk flags (expectation
gaps, competing offers mentioned), and talking points for the offer call.
```

---

### Pattern 5: "Why Is This Req Stuck" Root Cause
**Tools:** Greenhouse MCP + Interview notes + Slack/Email

```
[JOB NAME] has been open [X] days and we're not closing. Do a root
cause analysis. From Greenhouse: pull stage conversion rates, rejection
reasons by stage, time-in-stage averages, and source performance. From
interview notes: summarize what interviewers are citing as concerns.
From Slack/email: look for any hiring manager feedback or context on
changing requirements. Synthesize into: (1) the most likely root cause,
(2) what the data supports, (3) three specific actions to unstick this
req.
```

---

### Pattern 6: Candidate Re-Engagement Intelligence
**Tools:** Greenhouse MCP + Interview notes + Email

```
Build a re-engagement list for [JOB NAME OR ROLE TYPE]. From Greenhouse:
pull all rejected or withdrawn applications from the last 18 months that
reached recruiter screen or beyond. Filter out hard disqualifiers using
rejection reasons. From interview notes: pull any qualitative feedback
on these candidates. Cross-reference with email to see if we've stayed
in touch. Output: ranked re-engagement list with (1) candidate name,
(2) why they fell out, (3) what interviewers said, (4) whether the
role or requirements have changed in ways that might change the outcome,
(5) suggested re-engagement approach.
```

---

### Pattern 7: Hiring Manager Intelligence Report
**Tools:** Greenhouse MCP + Interview notes + Calendar + Slack

```
Give me an intelligence report on [HIRING MANAGER NAME]'s open reqs
before my check-in with them. From Greenhouse: all open jobs they own,
pipeline status per role, pending scorecards from their team, and offer
activity. From interview notes: any recent feedback patterns from
interviewers they've selected. From calendar: scheduled interviews this
week. From Slack: any recent messages about their roles or candidates.
Format as a briefing I can use to run a productive 20-minute check-in.
```

---

## Building Your Compound Workflows

The patterns above are starting points. The most powerful workflows are
the ones you build from your recurring, high-friction TA tasks. A good
test: if you find yourself pulling from three or more tools to answer
the same question every week, that's a workflow worth systematizing.

**How to build one:**
1. Name the question you're answering
2. List every tool you currently touch to answer it
3. Write the compound prompt that pulls all of those sources
4. Run it, note where it falls short, refine
5. Add it to this skill file so it runs automatically next time

---

## Honest Limitations

**Compound workflows are only as strong as your weakest integration.**
If interview notes live in a tool without an MCP connector, you'll need
to paste transcripts manually. The workflow still works — it's just
a copy-paste step rather than automatic.

**Context window matters for large pipelines.** Pulling full pipeline
data + interview transcripts + email threads for 50+ candidates at once
can exceed what a single LLM call can reason over well. Scope to
specific roles, time windows, or candidate subsets for best results.

**Sensitive data crosses tool boundaries in these workflows.** Candidate
compensation expectations, interview feedback, and communication history
are all in scope. Be intentional about who has access to these compound
outputs and where they're stored.

**LLM reasoning on compensation is informational, not advisory.**
Offer intelligence workflows surface data and patterns — final offer
decisions should involve your comp team and follow your internal process.

---

## Build Your Org Layer

Compound workflows are where your `ops-assist` context layer pays off
the most. When your LLM already knows your department mappings, stage
names, HM preferences, and custom field semantics, the compound prompts
above require no modification — they just run. Without that context,
you'll spend time in each session re-explaining your org structure.

Invest in your `ops-assist` skill proportional to how often you run
these compound workflows.
