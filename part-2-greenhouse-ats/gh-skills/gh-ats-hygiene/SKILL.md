---
name: gh-ats-hygiene
description: >
  Audits your Greenhouse instance for data quality issues that corrupt
  reporting, inflate pipeline counts, and hide accountability gaps. Use this
  skill whenever a TA leader or ops person wants to clean up their ATS, audit
  data quality, find ghost jobs, identify missing hiring team assignments,
  surface broken approval flows, or understand why their reports don't match
  reality. Trigger for questions like "why do my numbers look off", "find jobs
  with no recruiter assigned", "show me template jobs that are still open",
  "audit our rejection reason usage", "find candidates with no activity",
  or any time you suspect your ATS data doesn't reflect ground truth.
  Pairs with the ops-assist skill for org-specific nuance.
---

# Greenhouse ATS Hygiene

Your ATS data quality is the ceiling on your reporting quality. Dirty data
doesn't just produce bad reports — it hides accountability, inflates headcount
counts, and makes your LLM less useful because it's reasoning on bad inputs.

**The core insight:** Most TA leaders know their ATS is messy. The problem is
finding the mess. This skill makes that a one-pass operation instead of a
multi-day audit.

---

## What This Unlocks

- Ghost job detection: open reqs that are templates, tests, or should be closed
- Missing assignment audit: jobs with no recruiter, coordinator, or hiring
  manager — the accountability gaps hiding in plain sight
- Stale candidate identification: active applications with no movement for
  weeks or months, inflating your "active pipeline" numbers
- Rejection reason hygiene: whether your team is actually using rejection
  reasons, and whether the taxonomy is specific enough to be useful
- Opening vs. job mismatch: jobs with open status but no open openings,
  or openings that closed but the job is still active
- Approval flow gaps: jobs with no approval flow configured, or flows
  that are stalled
- Custom field completeness: which jobs are missing required fields that
  downstream reports depend on
- Duplicate or merged candidate issues: candidates who appear multiple
  times or have conflicting data

---

## Key Endpoints

| Endpoint | What It Gives You |
|---|---|
| `list_jobs` | All jobs including templates — filter `templates: include` to expose them |
| `list_openings` | Opening-level status — find open jobs with no open openings |
| `list_job_hiring_managers` | HM assignments — find jobs with no HM set |
| `list_job_owners` | Recruiter/coordinator assignments — find unowned jobs |
| `list_applications` | Applications with last_activity_at — find stale candidates |
| `list_rejection_details` | Actual rejection reason usage across your pipeline |
| `list_rejection_reasons` | Full rejection reason catalog — spot unused or vague reasons |
| `list_approval_flows` | Approval flow status per job — find stalled or missing flows |
| `list_users` | Full user roster — identify inactive users still assigned to jobs |
| `list_custom_fields` | Custom field definitions — audit completeness requirements |
| `list_departments` | Department structure — find jobs with no department assigned |
| `list_sources` | Source catalog — identify untagged or miscategorized sources |

---

## Core Prompts

Copy and run these as-is.

---

**1. Ghost Job Audit**
```
Pull all jobs from Greenhouse with status open, including template jobs.
Identify: (1) any job with "template", "test", "evergreen", or "TBD" in
the title, (2) any job with zero applications that has been open more
than 30 days, (3) any job with no open openings but still showing as
open status. Return a list with job name, ID, opened date, application
count, and open opening count. This is my close-or-archive list.
```

---

**2. Missing Hiring Team Audit**
```
Pull all open jobs. Cross-reference with job_hiring_managers and
job_owners to identify: (1) jobs with no hiring manager assigned,
(2) jobs with no recruiter assigned, (3) jobs with no coordinator
assigned. For each gap, show the job name, ID, department, days open,
and current application count. These are my accountability blind spots.
```

---

**3. Stale Active Candidate Audit**
```
Pull all applications with status active. Filter for candidates whose
last_activity_at is more than 21 days ago. Group by job and stage.
For each group show: candidate count, average days since last activity,
recruiter assigned to the job. Rank by total stale candidate count
descending. This is my pipeline inflation report.
```

---

**4. Rejection Reason Usage Audit**
```
Pull all rejection_reasons configured in this Greenhouse instance.
Then pull rejection_details for the last 90 days. For each rejection
reason calculate: how many times it was used, at which stages it was
applied, and what percentage of total rejections it represents. Flag
any reason used fewer than 3 times (potentially redundant) and any
reason used for more than 40% of rejections (potentially a catch-all
masking real data).
```

---

**5. Opening vs. Job Status Mismatch**
```
Pull all open jobs and their openings. Flag: (1) jobs where all
openings are closed but the job is still open status, (2) jobs where
open opening count is zero but the job has active candidates in
late-stage interviews, (3) jobs where opening count doesn't match
what the requisition approval was for (if custom field available).
These are my headcount tracking errors.
```

---

## Power Prompts

---

**Inactive User Assignment Audit**
```
Pull all users from Greenhouse. Identify any user marked as inactive
or with no login in the last 90 days (if available). Cross-reference
with job_owners and job_hiring_managers to find any open jobs still
assigned to inactive users. Return: job name, assigned user, user
status, and days the job has been open. These need reassignment before
pipeline stalls.
```

---

**Department Tagging Completeness**
```
Pull all open jobs. For each job check whether a department is assigned.
Return a list of jobs with no department assignment. Also check whether
jobs assigned to departments use the correct department IDs vs. a
catch-all or default. This affects every department-level report
downstream.
```

---

**Full ATS Health Report**
```
Run a complete ATS hygiene audit across all open jobs. Check for and
report on: (1) template/test jobs still open, (2) jobs missing recruiter
or HM assignment, (3) active candidates with 21+ days no activity,
(4) jobs with open status but no open openings, (5) rejection reasons
with no usage in 90 days, (6) jobs with no department assigned.
Return as a structured report with a count per issue type and a
combined "jobs needing immediate attention" list.
```

---

## Honest Limitations

**Greenhouse doesn't expose a "last login" field for users via Harvest.**
Inactive user detection relies on your HR/IT integration or manual
cross-reference with your HRIS. Use the user list to find candidates,
then verify status in your HRIS.

**Custom field completeness depends on your field configuration.**
Required vs. optional custom fields are enforced at the GH UI level —
the API will return empty values for missing fields. You'll need to
know which fields are supposed to be populated in your org (capture
this in your `ops-assist` layer).

**Template job detection is heuristic.** The API has a `templates`
filter but naming conventions vary. The ghost job prompt uses title
keywords — you'll want to add your org's specific template naming
patterns to make it more precise.

**Hygiene is a recurring task, not a one-time fix.** The value compounds
when you run these audits on a cadence (weekly for stale candidates,
monthly for the full health report) rather than as a single cleanup.

---

## Build Your Org Layer

This skill is the most dependent on org-specific context of the four.
The prompts above will find generic issues on any GH instance. To make
them precise for your org, add to your `ops-assist` skill:

- Your template job naming patterns
- Which custom fields are required for your workflow
- Your department ID → team name mappings
- Your stage names (so stale candidate flags use your actual stage names)
- Any known data quirks in your instance (e.g., India roles filed under
  a separate office ID, evergreen reqs that should be excluded)

The `ops-assist` skill template has the scaffolding for all of this.
