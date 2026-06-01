# Getting Started: Greenhouse + AI for TA Leaders

**Who this is for:** TA leaders and recruiters who have the Greenhouse MCP
connector set up and want to start pulling real value from it — but aren't
sure where to start or how the skill files work together.

**What you'll be able to do after reading this:** Pull live Greenhouse data,
run your first pipeline intelligence query, and understand exactly which skill
to reach for and when.

**Time to read:** 10 minutes. Time to first useful output: 30 minutes.

---

## First: Understand What You Have

You have five skill files. Think of them this way:

**ops-assist** is always on. It's a context file that tells your AI how your
specific Greenhouse instance is structured — your department IDs, your team
names, your known data quirks. It doesn't do anything on its own. It makes
everything else more accurate. Load it once, maintain it over time.

The other four are specialists you call situationally:

| When you're thinking... | Reach for... |
|---|---|
| "How is hiring going across the whole org?" | `gh-talent-context` |
| "I need to deep-dive one specific role" | `gh-role-intelligence` |
| "My data feels off / I need to clean up the ATS" | `gh-ats-hygiene` |
| "I need GH data plus something else — notes, benchmarks, email" | `gh-compound-workflows` |

**The simple mental model:** ops-assist knows your org. The other four know
what to do with your data. You never need all four at once — pick the one
that matches what you're trying to answer.

---

## Do These Overlap?

Occasionally, but not in ways that cause problems. Here's where the lines are:

`gh-talent-context` is **org-wide and strategic.** It answers "where do we
stand across all of hiring."

`gh-role-intelligence` is **single-req and tactical.** It answers "what's
happening on this specific job and why."

If you ask a question that could fit both — like "how is our Engineering
pipeline?" — start with `gh-talent-context` for the overview, then switch
to `gh-role-intelligence` if you need to drill into a specific Engineering req.

`gh-ats-hygiene` is **diagnostic.** It finds problems in your data. You
don't need it for day-to-day questions — run it when something looks off or
on a monthly audit cadence.

`gh-compound-workflows` is **additive.** It only comes into play when you
need Greenhouse data combined with another tool — interview notes, comp data,
benchmarks, email. If you're only touching GH, you don't need it.

---

## Step 1: Set Up Claude Projects

Skill files live in **Claude Projects**. A Project is a persistent workspace
in Claude where your files and instructions are always loaded — you don't have
to paste anything into every conversation.

1. Open Claude at claude.ai
2. In the left sidebar, click **New Project**
3. Name it something like "TA Intelligence" or "Greenhouse Workspace"
4. You'll see a **Project Instructions** field at the top — this is where
   your skill files live

> **Why Projects and not a regular chat?**
> Regular Claude chats have no memory. Every new conversation starts blank.
> A Project keeps your skill files loaded permanently so you never have to
> re-explain your setup.

---

## Step 2: Load ops-assist First

Before loading the four Greenhouse skills, load ops-assist — but you need
to fill it in first.

**To fill in ops-assist:**

Open the `ops-assist.md` file. You'll see sections with instructions in
brackets like `[fill this in]`. The two sections to complete first are:

**Section 3 — Department & Team Mappings.** This is the most important one.
Your AI will use these to correctly filter Greenhouse data when you reference
a team by name. To get your department IDs, paste this prompt into a regular
Claude chat with Greenhouse connected:

```
Run list_departments in Greenhouse and return every department with its
ID and name. Format as a simple list.
```

Copy the output and paste it into Section 3 of ops-assist, mapping each
department name to how your team actually refers to it.

**Section 6 — Known Quirks.** Add anything that would trip up someone new
to your Greenhouse instance: template jobs to exclude, evergreen reqs, custom
fields that matter, source naming oddities. Don't overthink it — add what
you know now and build over time.

Once filled in, paste the entire ops-assist content into the **Project
Instructions** field in Claude.

---

## Step 3: Load Your First Greenhouse Skill

For most TA leaders, start with `gh-talent-context` — it gives you the
broadest immediate value and is the most satisfying first experience.

In your Claude Project:
1. Below the Project Instructions, look for **Add Content** or **Upload File**
2. Upload `gh-talent-context.md` directly, or paste its contents as a
   second block in Project Instructions beneath ops-assist

Your Project Instructions should now have two things loaded:
- ops-assist (your org context)
- gh-talent-context (the intelligence skill)

---

## Step 4: Run Your First Query

Open a new chat inside your Project. The skill files are already loaded —
you don't need to reference them. Just ask naturally.

Start with this:

```
Give me an org-wide pipeline snapshot. Show all open jobs grouped by
department with active candidate counts by stage. Flag any req that's
been open more than 90 days.
```

**You'll know it's working when** the AI calls Greenhouse tools (you'll
see tool calls appear in the response), returns real job names and candidate
counts from your ATS, and groups them by your actual department structure.

If it returns generic or made-up data, it's not hitting Greenhouse — check
that your MCP connector is active in the session.

---

## Step 5: Add the Other Skills As You Need Them

Don't load all four Greenhouse skills at once on day one. Add them as the
need arises:

**Add `gh-role-intelligence` when** you have a pipeline review coming up
or a req that's stalling and you want to understand why.

**Add `gh-ats-hygiene` when** your pipeline numbers feel inflated, you're
prepping for a board report and want clean data, or you haven't done an
ATS audit in a while.

**Add `gh-compound-workflows` when** you have other tools connected
(interview notes platform, benchmark data, Slack) and want to pull them
together with GH data.

To add a skill: upload it to your Project the same way you added
gh-talent-context — either as an uploaded file or pasted beneath the
existing Project Instructions.

---

## Day-to-Day Usage: How to Think About It

Once you're set up, using this isn't about remembering skill names. It's
about asking questions the way you'd ask a sharp analyst sitting next to you:

> *"Which of my reqs have been open the longest with the least pipeline
> movement?"*

> *"Prep me for my pipeline review with [HM name] on [role] — I have
> 20 minutes."*

> *"Something feels off with our Engineering numbers — can you audit
> that department's data?"*

> *"Why is [role] stuck? What does the data say?"*

The skills translate those natural questions into precise Greenhouse API
calls and return structured answers. You don't need to know which endpoints
are running. You just need to ask the right question.

---

## When Something Doesn't Work

**The AI returns generic or hallucinated data:** Your MCP connector isn't
active. Check that Greenhouse is connected in your Claude settings and that
your Project has tool access enabled.

**The AI pulls the wrong teams or departments:** Your ops-assist mappings
need updating. Add the correct department IDs to Section 3 and re-paste
ops-assist into Project Instructions.

**Stage names return no results:** Greenhouse stage name filters are case-
sensitive. Run `list_job_interviews` on a real job to see your exact stage
names, then add them to ops-assist Section 5.

**The AI seems confused about which skill to use:** You don't need to tell
it — just ask your question naturally. The skill descriptions handle routing
automatically. If outputs feel off, try being more specific: "using
gh-role-intelligence, give me a full breakdown of [job name]."

---

## The Habit That Makes This Compound

The TA leaders who get the most out of this setup do one thing consistently:
**when they correct the AI, they update ops-assist.**

Every time you find yourself saying "no, that's not right — Engineering at
our company includes X and Y" — that correction belongs in ops-assist before
you close the session. Over time, your ops-assist becomes a precise map of
your org and your AI stops making those errors entirely.

The skill files are the starting point. Your ops-assist is what makes them
yours.
