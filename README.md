# Prompt & Context Engineering for RecOps — Take-Home Kit
*Open for Ops 2026 · Greenhouse*

Everything from the session, in one place, ready to use. **No coding. No setup beyond copy-and-paste.** If you can copy text and drag a file, you can do all of this.

*(RecOps = Recruiting Operations — the behind-the-scenes work that keeps hiring running.)*

---

## The one big idea (read this first)

This kit gives you three ready-made tools. But the real prize is the **method** behind them — a simple, repeatable way to turn an AI like Claude into a specialist for *your* job.

The method is four moves:

1. **Learn** how good prompting works — for *your* role, not in general.
2. **Build a "Prompt Engineer"** — a helper whose only job is to write good instructions for you.
3. **Use it to build your own tools** — starting with a job-description writer.
4. **Borrow from finished examples** — we've included three (JD Builder, Oracle, Sensei) so you can see the finished shape and use them today.

Follow the steps in order the first time. After that, jump to whatever you need.

---

## A 30-second vocabulary (so every step makes sense)

You'll do everything inside **Claude** (the same ideas work in ChatGPT or Gemini). Three words to know:

- **Project** — a saved workspace in Claude. Think of it as a labeled folder that remembers things. *(In Claude: left sidebar → **New project**.)*
- **Project Instructions** — a text box inside a Project. Whatever you paste here, the AI follows in *every* chat in that Project. This is where a tool's prompt "lives."
- **Knowledge / Files** — documents you upload into a Project so the AI can read them (like the benchmark PDFs Oracle uses).

**The 2-minute setup you'll repeat for each tool:**

1. Open the tool's prompt file (linked in each step) and **copy everything** in it.
2. In Claude, make a **New project** and name it after the tool.
3. Open **Project Instructions**, **paste**, and save.
4. Start a new chat inside that Project. The tool is now live.

That's the whole setup. A couple of tools have one extra step (uploading a file) — it's called out where it applies.

---

## Step 1 — Learn how to prompt *for your job*

Before building anything, spend ten minutes learning what good prompting looks like **for the kind of work you actually do**. Don't read a generic "top 10 prompt tips" list — ask the AI to research it for your situation.

Open a normal Claude chat and try this (use Claude's **research** mode if you have it):

> *"Research the best prompting techniques for someone in my role. I work in [your role — e.g., recruiting operations]. The information I deal with most is [e.g., job descriptions, hiring-pipeline numbers, interview notes]. The tasks I want help with are [e.g., writing job posts, spotting where candidates drop off]. Give me the techniques that matter for this, each with a short example."*

You'll get back a short, personalized guide — the vocabulary and habits that make everything below work better.

---

## Step 2 — Build your own Prompt Engineer

A **Prompt Engineer** is a helper with one job: it interviews you in plain language, then writes a clean instruction prompt *for you*. You build it once and reuse it forever.

Make a new Project called **Prompt Engineer**, paste this into its Project Instructions, and save:

```
You are my Prompt Engineer. Assume I'm not technical.
I want to build a helper that does one job, which I'll describe in a sentence.

When I tell you the job:
1. Ask me up to 5 simple, plain-language questions IN ONE MESSAGE — about my
   work, the information I deal with, and what a great result looks like.
   Never use jargon.
2. Then write me a clear, copy-paste instruction prompt I can save in a Claude
   Project so the helper behaves the same way every time.
3. Then list "Context worth adding" — the extra facts or files that would make
   this helper noticeably better.

Keep everything copy-paste clean.
```

Now you have a builder. Any time you want a new tool, tell it the job — it writes the prompt *and* tells you what context to feed it.

---

## Step 3 — Build the JD Builder (your first real tool)

JD = **Job Description**. It's the perfect first build: everyone needs it, and a good result is obvious.

**Try it yourself.** Open your Prompt Engineer (Step 2) and say:

> *"Build me a helper that writes job descriptions for my company without a rewrite cycle. Ask me what context would help."*

It will ask what to include. Here are the **things worth giving it** (you won't need all of them):

- Company name + a one-line description of what you build
- Company stage (early startup, growing, established) and your product
- A few sentences of your company's real writing / voice
- The role title and how senior it is (junior / mid / senior / manager)
- 3–5 must-have skills, plus any nice-to-haves
- Pay range (legally required in some places — CA, WA, CO, NY, IL, MD, NJ)
- Location + remote / hybrid / onsite
- Your EEO statement

**Or use the finished one we built this exact way:** [`part-1-prompt-context-engineering/jd-builder/SKILL.md`](part-1-prompt-context-engineering/jd-builder/SKILL.md). Run the 2-minute setup with it, then ask it to write a JD — it collects what it needs and hands back a ready-to-post draft in one pass, with no "rockstar / ninja" filler.

---

## Step 4 — Oracle, your RecOps Analyst

**What it does:** paste a hiring funnel (how many candidates make it through each stage) and Oracle finds the single biggest bottleneck, names the likely causes, and tells you how to confirm and fix it. Ask it a strategy question instead (with no funnel) and it writes a decision brief from real benchmark reports.

**The context that makes it smart** — Oracle reads two real benchmark reports. Set it up like this:

1. Run the 2-minute setup with [`part-1-prompt-context-engineering/oracle/oracle.md`](part-1-prompt-context-engineering/oracle/oracle.md).
2. **Extra step** — in the same Project, open **Add knowledge / upload files** and upload **both** PDFs from [`part-1-prompt-context-engineering/oracle/data/`](part-1-prompt-context-engineering/oracle/data/):
   - `Greenhouse_Benchmark_Report_March2026_NA.pdf`
   - `Benchmarket_2026_State_of_Hiring.pdf`
3. Near the top of `oracle.md` there's a clearly marked `Company:` placeholder (two short lines). Swap it for your own company before you start.

**The demo — try this:**

> *"Applied 500 → Recruiter screen 120 → HM screen 35 → Onsite 12 → Offer 4 → Hired 3. Role: Senior Backend Engineer."*

Oracle returns a one-line verdict, the funnel math, the one bottleneck, ranked causes, a 30-day plan to confirm the cause, and the fix — every number tied to a source.

**You'll know it's working when** every flag carries a number and every benchmark carries a little source tag like `[GH]` (Greenhouse report) or `[BM]` (the market report).

---

## Step 5 — Sensei (a bonus, and a blueprint)

**What it does:** Sensei takes a confusing technical role — say "GNC engineer" or "biostatistician" — and explains it in plain, everyday language: simple analogies, talking points for hiring managers, and screening questions a non-technical person can actually ask.

Run the 2-minute setup with [`part-1-prompt-context-engineering/sensei/sensei.md`](part-1-prompt-context-engineering/sensei/sensei.md), then give it a role title or paste a job description.

**Why it's also a blueprint:** under the hood, Sensei is just "explain a hard thing to a beginner, the same way every time." Open `sensei.md` to see how it's built, then use it as **inspiration** — point your Prompt Engineer (Step 2) at the same shape to build a decoder for anything confusing in your world.

---

## Part 2 — The Greenhouse ATS skills (the second half of the session)

The first half (above) was about building tools from prompts. The **second half**, contributed by a colleague, connects the same idea to your **live Greenhouse data**. This part isn't mine, so what follows is a plain explanation of what's included — the full how-to lives in its own guide.

**First, what a "skill" is:** a skill is just a saved instruction file that turns Claude into a specialist. You load it into a Project once; from then on it knows *when* to act and *what* to do, so you can ask normal questions ("how's hiring going?") and it does the precise work behind the scenes. *(These skills need the Greenhouse connector switched on so Claude can read your real data.)*

**The five skills, at a glance:**

| Skill | What it does |
|---|---|
| `ops-assist` | The "always-on" context layer. Fill it in once with how *your* Greenhouse is set up (team names, IDs, quirks). It makes every other skill more accurate. |
| `gh-talent-context` | The org-wide view — "where do we stand on hiring across everything, right now." |
| `gh-role-intelligence` | The single-role deep dive — "why is *this* job stuck, and who's left in the pipeline." |
| `gh-ats-hygiene` | The cleanup crew — finds messy data that throws off reports (ghost jobs, missing owners, stale candidates). |
| `gh-compound-workflows` | The combiner — mixes Greenhouse with other tools (interview notes, benchmarks, email) to answer "why" and "what next." |

**Where to start:** open [`part-2-greenhouse-ats/gh-skills/GETTING-STARTED.md`](part-2-greenhouse-ats/gh-skills/GETTING-STARTED.md) and follow it — it walks you through setup and your first query in about 30 minutes. Prefer a single download? Grab [`gh-skills.zip`](part-2-greenhouse-ats/gh-skills.zip).

---

## Want the whole kit at once?

Click the green **`Code`** button at the top of this page → **Download ZIP**. You'll get every prompt and report in one download — unzip it and you're ready to go.
