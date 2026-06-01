# Prompt & Context Engineering for RecOps — Take-Home Kit
*Open for Ops 2026 · Greenhouse*

Everything from the session, ready to use. No coding required. If you can copy, paste, and drag a file, you can deploy all of this.

---

## What's in here

The session had two parts. This kit is split the same way.

| | Part | Who | Folder |
|---|---|---|---|
| **Part 1** | Prompt & Context Engineering — three tools you can use Monday | Bryan | [`part-1-prompt-context-engineering/`](part-1-prompt-context-engineering/) |
| **Part 2** | Greenhouse ATS — plug it into your live pipeline data | Matt | [`part-2-greenhouse-ats/`](part-2-greenhouse-ats/) |

---

## Part 1 — Your three tools

| Tool | What it does |
|---|---|
| **JD Builder** | Draft any job description without a rewrite cycle |
| **Sensei** | Translate a technical role into plain everyday language |
| **Oracle** | Read a hiring funnel, find the bottleneck, hand you the fix |

### How to deploy any of them (about 2 minutes)

You'll do this once per tool. The example uses Claude, but the same steps work in ChatGPT or Gemini.

1. **Open the tool's folder** above and open its prompt file (`SKILL.md`, `sensei.md`, or `oracle.md`).
2. **Copy everything** in that file.
3. In Claude, click **Projects → + New project**, give it the tool's name.
4. Open the project's **instructions** (the "Set instructions" / custom-instructions box) and **paste**.
5. **Save.** That's the whole setup — your tool is live. Start a new chat inside the project and use it.

### One extra step for Oracle

Oracle reads real benchmark reports, so give it the data:

6. In the Oracle project, find **Add knowledge / upload files** and upload **both** PDFs from
   [`part-1-prompt-context-engineering/oracle/data/`](part-1-prompt-context-engineering/oracle/data/):
   - `Greenhouse_Benchmark_Report_March2026_NA.pdf`
   - `Benchmarket_2026_State_of_Hiring.pdf`

Now ask Oracle about any role's funnel and it answers from those reports.

> **Tip:** every prompt has a placeholder company in it. Swap that one line for your own company and you're set.

---

## Part 2 — Greenhouse ATS (Matt's section)

This is the **second half** of the session: connecting the same pattern to your live Greenhouse data.

Everything's already unzipped in **[`part-2-greenhouse-ats/gh-skills/`](part-2-greenhouse-ats/gh-skills/)** — start by opening **[`GETTING-STARTED.md`](part-2-greenhouse-ats/gh-skills/GETTING-STARTED.md)** and follow the steps.

The five skills inside:
- `ops-assist` — your org context layer (load this once)
- `gh-talent-context` — org-wide pipeline intelligence
- `gh-role-intelligence` — single-req deep dives
- `gh-ats-hygiene` — find and fix ATS data debt
- `gh-compound-workflows` — Greenhouse + interview notes + benchmarks

*Prefer one file?* Download **[`gh-skills.zip`](part-2-greenhouse-ats/gh-skills.zip)** instead.

---

## Want the whole kit at once?

Click the green **`Code`** button at the top of this page → **Download ZIP**. That gives you every prompt and report in one download — unzip it and you're ready to deploy.
