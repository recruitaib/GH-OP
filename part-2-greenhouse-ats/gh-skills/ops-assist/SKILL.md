---
name: ops-assist
description: >
  The org-specific context layer that sits underneath all Greenhouse skills.
  Tells your LLM the things the ATS can't infer: how your org is actually
  structured, what your department IDs mean, which job titles map to which
  teams, which reqs are real vs. templates, and how you want data returned.
  Load this skill alongside gh-talent-context, gh-role-intelligence,
  gh-ats-hygiene, or gh-compound-workflows to make every prompt more accurate
  without re-explaining your org every session. Trigger any time a question
  involves your specific recruiting data — this is the briefing doc your LLM
  reads before touching your ATS.
---

# OpsAssist

Your LLM knows how Greenhouse works. It doesn't know how *your* Greenhouse
works. That's what this file is for.

OpsAssist is a context layer you fill in once and maintain over time. It
closes the gap between what your ATS stores and what your team actually
means — department IDs vs. team names, req naming conventions, known data
quirks, stage names, the tribal knowledge that currently lives only in
your head.

**The other four Greenhouse skills get significantly more accurate when
this file is populated. The compound workflows skill in particular assumes
this context is available.**

---

## How This Works

This file has two parts:

**Part 1 — Instructions for your LLM.** Read once, applied to every
Greenhouse query in the session.

**Part 2 — Your Org Context.** The sections you fill in. Start with
Department Mappings and Known Quirks. Add the rest as you go.

When you ask a recruiting question, your LLM reads this file first, then
queries Greenhouse with the correct IDs, filters, and output format —
without you having to explain your setup each time.

---

## Part 1: Instructions for Your LLM

When answering any question involving Greenhouse data, follow these rules:

**1. Read this file before querying anything.**
Use the org context below to resolve ambiguous team names, department
references, and role titles before making any API call. Never rely on
free-text job title search when a department ID or user ID is available —
IDs are stable, titles drift.

**2. Resolve team references to ATS identifiers.**
If the user says "show me Engineering roles" and the org context maps
Engineering to department IDs [X, Y, Z], query all of them. A single
colloquial team name often spans multiple ATS departments — especially
after reorgs.

**3. Exclude known noise from results.**
Apply the template/test job exclusions and known evergreen req filters
listed in section 6 before returning results. Never surface template jobs
in pipeline counts unless explicitly asked.

**4. Use stage names exactly as they appear in this org's Greenhouse.**
Stage names are case-sensitive in Greenhouse API filters. Use the exact
stage names listed in section 5. If a stage name isn't listed, use
`list_job_interviews` to discover the actual names before filtering.

**5. Return results in the preferred format.**
Use the default output format in section 7. If the user specifies a
different format in their request, use that instead.

**6. Flag gaps and surface them.**
If a role, team, or department appears in results that doesn't match
anything in this file, note it and flag that the org context may need
updating. Don't silently drop unknown data.

**7. Apply the hygiene flags automatically.**
When pulling pipeline data, automatically apply the stale candidate
threshold, minimum opening count visibility, and aging flag thresholds
defined in section 7. Don't wait to be asked.

---

## Part 2: Your Org Context

> **Setup instructions:**
> Fill in the sections below. You don't need to complete all sections
> to get value — start with sections 3 (Department Mappings) and 6
> (Known Quirks). Those two sections have the highest leverage on
> accuracy. Add the others as you encounter gaps.
>
> To find your Greenhouse IDs: use `list_departments`, `list_offices`,
> `list_users`, and `list_sources` to pull the actual IDs for your
> instance. Run those once and paste the results into the relevant
> sections below.

---

### 1. Instance & Integrations

```
Greenhouse instance URL: [e.g., app.greenhouse.io/your-org]
Greenhouse MCP endpoint: [e.g., mcp.us.greenhouse.io/mcp]

Other connected tools and what they add:
  [Tool name]: [what it contributes — e.g., "Metaview: interview transcripts
                and AI notes for all screened candidates"]
  [Tool name]: [...]

HRIS: [e.g., Workday, BambooHR, Rippling — for cross-referencing headcount]
Comp tool: [e.g., Radford, Levels.fyi, internal bands doc — for offer workflows]
```

---

### 2. Org Structure Overview

A brief description of how your company is organized for recruiting purposes.
This helps the LLM understand the relationship between business units,
departments, and teams before diving into IDs.

```
Company stage: [e.g., Series D, ~400 employees, aggressive hiring plan]
Hiring volume: [e.g., ~80 open reqs at any time across 6 departments]
TA team structure: [e.g., 4 recruiters, 1 coordinator, 1 sourcer —
                   recruiters own reqs end-to-end]
Key hiring priorities right now: [e.g., "Applied AI and GTM are highest
                                   priority through end of year"]
```

---

### 3. Department & Team Mappings

**This is the most important section.** Map every colloquial team name
to the exact Greenhouse department IDs. Run `list_departments` to get
the full ID list for your instance, then fill in below.

```
[Team name as your team says it]:
  Greenhouse Department ID(s): [id1, id2]
  Common job title variations: [title A, title B, title C]
  Notes: [e.g., "India-based roles for this team use office ID 99 —
          filter by office_id separately"]

[Team name]:
  Greenhouse Department ID(s): [id]
  Common job title variations: [...]
  Notes: [...]

[Team name]:
  Greenhouse Department ID(s): [id]
  Common job title variations: [...]
  Notes: [...]
```

**Example (fill in with your own):**
```
Engineering:
  Greenhouse Department ID(s): [1001, 1002, 1003]
  Common job title variations: Software Engineer, SWE, MLE, Staff Engineer,
                                Principal Engineer, Engineering Manager
  Notes: Platform and Infrastructure are sub-departments under Engineering
         with their own IDs — always include all three for "Engineering" queries

GTM / Revenue:
  Greenhouse Department ID(s): [2001, 2002]
  Common job title variations: AE, Account Executive, CSM, Customer Success,
                                Solutions Engineer, SE
  Notes: Enterprise and Mid-Market are separate department IDs despite being
         the same sales team colloquially
```

---

### 4. Key Users: Recruiters & Hiring Managers

Run `list_users` to get user IDs. Fill in the people most commonly
referenced in pipeline questions.

```
Recruiters:
  [Name] — Greenhouse user ID: [id] — owns: [teams/departments]
  [Name] — Greenhouse user ID: [id] — owns: [teams/departments]

Hiring Managers (most active or most commonly referenced):
  [Name] — Greenhouse user ID: [id] — team: [team name] — open reqs: [count if known]
  [Name] — Greenhouse user ID: [id] — team: [team name]
```

---

### 5. Stage Names (Exact)

Stage names are case-sensitive in Greenhouse API filters. List your actual
stage names here so filters work without discovery overhead.

Run `list_job_interviews` on a representative job to pull your stage names,
then paste them here.

```
Standard pipeline stages (in order):
  1. [Exact stage name]
  2. [Exact stage name]
  3. [Exact stage name]
  4. [Exact stage name]
  5. [Exact stage name]

Stages that vary by department or role type:
  [Department/Role type]: uses [stage name] instead of [standard stage name]
  [Department/Role type]: has an additional [stage name] stage between X and Y
```

---

### 6. Known Quirks & Data Rules

The tribal knowledge that makes your ATS make sense. This section prevents
the most common LLM errors on your instance.

```
Template / test jobs to exclude:
  - [Pattern or ID — e.g., "Any job with 'Template' or 'TEST' in the title"]
  - [e.g., "Job IDs 10001–10015 are evergreen pipeline reqs, not active roles"]

Evergreen / pipeline reqs:
  - [How to identify them — e.g., "Req ID starts with 'EG-' or opening_id is null"]
  - [Whether to include or exclude in standard pipeline queries]

Custom fields that matter for your workflow:
  - [Field name]: [What it contains and when it's populated]
  - [Field name]: [...]

Source naming quirks:
  - [e.g., "LinkedIn Recruiter and LinkedIn Jobs are separate source IDs —
     always pull both when reporting on LinkedIn performance"]
  - [e.g., "Referrals tagged before [DATE] used source ID 55; after that, 78"]

Office / geography rules:
  - [e.g., "All EMEA roles use office ID 44 regardless of department"]
  - [e.g., "Remote roles may have no office ID — don't filter by office_id
             when looking for remote pipeline"]

Rejection reason rules:
  - [e.g., "Rejection reason 'Withdrew — Compensation' (ID: 12) means the
             candidate declined; 'Not a Fit' (ID: 8) is the catch-all we're
             trying to eliminate — flag when it exceeds 25% of rejections"]

Confidential reqs:
  - [e.g., "Confidential reqs are flagged in GH but should not appear in
             any summaries shared outside the TA team"]

Known data gaps:
  - [e.g., "Applications imported via Agency Portal before [DATE] have no
             source data — don't include in source performance reports"]
```

---

### 7. Default Output Format & Hygiene Thresholds

```
Default table columns for open req lists:
  [e.g., Job Title | Req ID | Hiring Manager | Department | Days Open |
          Active Candidates | Open Openings]

Group results by default:
  [e.g., department, then hiring manager]

Automatically flag:
  - Reqs open more than [X] days
  - Candidates with no activity in more than [X] days
  - Jobs with active candidates but zero open openings
  - Pending scorecards older than [X] days

Include in default output:
  - Salary/comp ranges: [yes / no / only when asked]
  - Open opening count per req: [yes / no]
  - Recruiter name: [always / only when asked]
  - Confidential reqs: [yes / no / only when asked directly]

Preferred summary style:
  [e.g., "Lead with exceptions and flags, then supporting detail" or
   "Always end with a recommended action list"]
```

---

### 8. Glossary

Internal terms, acronyms, and org-specific language Claude should recognize
without explanation.

```
[Term] = [Definition in your org context]
[Term] = [Definition]

Examples:
  KLT = Key Leadership Team — your exec org, used as a custom field on jobs
  "Planned HC" = headcount approved in the annual plan
  "Unplanned HC" = backfill or incremental hire not in original plan
  "Mavens" = a specific business unit within the GTM org
  "Req open" = job status is open AND at least one opening is unfilled
```

---

## Maintaining This File

OpsAssist compounds in value over time. Every time you correct your LLM,
that correction belongs here so it doesn't happen twice.

**Update triggers:**

- **After a reorg** — update department IDs and team-to-ID mappings immediately;
  stale mappings are the #1 source of bad pipeline pulls
- **When you correct Claude** — if the LLM pulled the wrong roles or missed
  a team, add the mapping to section 3 before closing the conversation
- **When a new team stands up** — add the department ID before the first req
  opens so it's never missing from queries
- **After a rejection reason cleanup** — update section 6 with new reason IDs
  and any catch-all patterns to flag
- **When source attribution changes** — if you add a new job board or agency
  source, add it to section 6 so reports stay accurate
- **Quarterly minimum** — review sections 3 and 4 for stale IDs, departed
  HMs still in the user list, or teams that have been renamed

---

## Relationship to Other Skills

```
ops-assist
  └── gh-talent-context     ← org context makes org-wide snapshots accurate
  └── gh-role-intelligence  ← stage names and HM IDs make single-req dives precise
  └── gh-ats-hygiene        ← quirks section defines what "clean" means for your org
  └── gh-compound-workflows ← full context layer enables compound prompts to run
                               without session setup overhead
```

This file is the foundation. The four Greenhouse skills work without it —
but they work significantly better with it populated.
