---
name: jd-builder
description: Draft job descriptions for any role at any company. Researches company tone via web search when needed. Calibrates requirements to market conditions. Produces ready-to-post JDs without rewrite. ALWAYS use when user mentions write a JD, draft job description, create JD for, job posting, JD writer, write a req, draft a role description, job ad, recruit for role, or hiring for role.
---

# JD Builder

## Purpose

Compose job descriptions ready to post to an ATS without rewrite. Calibrate requirements to market conditions. Match company tone.

## Required Inputs

Collect these before composing. If any REQUIRED field is missing, ask up to 3 clarifying questions in one message.

| Field | Required | If Missing |
|---|---|---|
| Role title | YES | Ask user |
| Level (IC level / senior / staff / manager / director) | YES | Ask user or infer from title |
| Company name | YES | Ask user |
| Company stage and product | YES | Web-search if user does not provide |
| Must-have skills (3-5) | YES | Ask user. Do not invent. |
| Years of experience range | YES | Ask user |
| Comp range or pay band | NO | Ask user. Leave placeholder if unavailable. |
| Market urgency | NO | Default to WARM |
| Nice-to-have skills | NO | Leave blank if none |
| Hiring manager voice preferences | NO | Default to plain technical voice |
| Education baseline | NO | Default to "Bachelor's in relevant field or equivalent experience" |
| Pay transparency jurisdiction | NO | If user is in CA, WA, CO, NY, IL, MD, treat pay band as REQUIRED |

## Execution Protocol

### Step 1: Intake

Ask up to 3 clarifying questions in one message if REQUIRED fields are missing. Do not ask more. If user provides incomplete answers after one round, proceed with WARNING BANNER listing what is missing.

### Step 2: Company Research (if needed)

If user provides company name but not stage/product, run web search:
- `[Company name] about us`
- `[Company name] funding stage`
- `[Company name] mission`

Extract: mission statement, funding stage, product description, recent news (last 6 months).

If web search returns nothing useful, ask user for a 2-3 sentence company description.

### Step 3: Market Calibration

Apply requirement calibration based on urgency:

| Urgency | Rule |
|---|---|
| DROUGHT | Drop ALL nice-to-haves. Broaden must-haves. Use "or equivalent experience" for education. Expand years range. |
| HOT | Drop 50%+ of nice-to-haves. Broaden 1-2 must-haves. Use "or equivalent experience" for education. |
| WARM | Keep input structure as-is. |
| COOL | Keep input structure as-is. May tighten 1-2 nice-to-haves to must-haves. |
| COLD | Specify tools by name. Promote 1-2 nice-to-haves to must-haves. |

Document the calibration decision in a single line before composing.

### Step 4: Compose

Output structure:

```
# [Role Title] — [Company Name]

## About [Company Name]
[2-3 sentences. Lead with mission. Reference momentum if available.]

## The Role
[1 paragraph. What this person owns. Why this role exists now.]

## What You'll Do
- [Action-verb bullet 1]
- [Action-verb bullet 2]
- [5-7 total bullets, each a concrete deliverable]

## What You'll Bring

### Must-Have
- [Skill 1]
- [Skill 2]
- [Skill 3]
- [X+ years experience in domain]
- [Education baseline]

### Nice-to-Have
- [Skill 4]
- [Skill 5]

## Who You'll Work With
[1 paragraph naming cross-functional partners if known. Skip if unknown.]

## Compensation
**Pay Range:** [Range or placeholder flagged]

[Benefits summary if user provided]

## Equal Employment Opportunity
[Placeholder for user's company EEO statement]
```

### Step 5: Voice Check

Before delivery, verify:
- No generic startup language: "rockstar", "ninja", "fast-paced environment", "wear many hats", "guru", "self-starter"
- Action verbs in responsibilities
- Specific deliverables (not "support the team")
- Pay range present OR placeholder explicitly flagged

### Step 6: Deliver

Output JD as a markdown block. Append a "Notes for User" section:
- Any WARNING BANNERS from missing inputs
- Recommendation: validate with hiring manager before posting
- Recommendation: confirm pay band if in transparency jurisdiction

## Quality Gates

Before delivering, verify:
1. Every must-have skill traces to user input (none invented)
2. Company description traces to user input or web search
3. Market urgency calibration was applied and documented
4. Pay band present OR explicitly flagged
5. No generic startup language present

## Behavior Rules

1. **Compose, do not invent.** If user did not provide a skill, ask. Do not guess.
2. **One-pass approval is the target.** Compose for clarity and confidence the first time.
3. **Pay transparency is a hard requirement in CA, WA, CO, NY, IL, MD.** Flag explicitly if missing in these jurisdictions.
4. **Skip generic startup language by default.**
5. **Match HM voice preferences if provided.** Otherwise default to plain technical voice.
