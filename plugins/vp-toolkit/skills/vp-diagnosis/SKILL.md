---
name: vp-diagnosis
description: >
  Diagnose and improve the value proposition (VP) on a specific web page — a landing page, homepage,
  product page, or any conversion-critical page where the visitor is asking "why should I buy from
  you?" Use whenever someone shares a URL or pastes page copy and wants it evaluated, audited, scored,
  critiqued, or improved as a value proposition; asks "why isn't this page converting?", "is this
  messaging any good?", "does my homepage give a reason to buy?", "score my landing page", or "what
  should I fix on this page?"; or wants testable copy fixes for an existing page. It extracts the VP the
  page actually communicates, scores each idea on a harsh 0–2 model, finds the failing axis, and returns
  prioritised, testable copy-rewrite hypotheses. Works on for-profit and general (non-donor) pages at
  the idea level. When the page lacks the target-customer or differentiation context to score honestly,
  it hands off to the vp-development skill instead of guessing.
---

# VP Diagnosis + Improvement — score a page's value proposition and fix it

## What this skill produces
A structured verdict on whether a page gives the visitor **strong, unique, believable reasons to
act**, which axis is failing, and a **prioritised list of testable copy-rewrite hypotheses**. Scope
is **copy-level only** — headline, subhead, proof, CTA. Structural/visual problems may be *named* as
observations, but the deliverable is copy. It uses the **same scoring model** as the vp-development
skill (`references/scoring-and-assembly.md`), so a VP built there can be re-scored here without
translation.

## Distinct lane (why this is not a re-skin of an existing VP scorer)
This skill is for **for-profit / general pages** (not nonprofit/donor copy), uses the **harsh 0–2
idea-level model with a dual Difference score**, and works by **extracting the whole page's implicit
VP and running a rewrite engine** — not section-by-section persona-friction simulation. If a task is
really donor-facing fundraising copy or a section-friction walkthrough, use the appropriate other
skill instead.

## Core stance (read before starting)
- **Observed vs inferred.** Separate rigorously what the **page states** (observed) from what you
  **infer** about the target customer, comparison set, or buyer motivation. Every inference is a
  labelled assumption. Never let an inference masquerade as a fact.
- **Judge from the target customer's perspective**, not the owner's or your own taste. A benefit is
  only a benefit if *that customer* wants it; a difference only if *that customer* sees it as unusual.
- **Diagnose the lowest axis, never the average.** Because the score is multiplicative, a single 0
  zeroes the idea. The bottleneck is the fix.
- **Copy fixes must be honest.** Never invent a benefit, differentiator, proof, or number. Where a
  rewrite needs a real fact the page doesn't give, write a placeholder for the owner to fill.
- **Critical coach.** Say plainly when a page communicates no coherent VP at all (common). Don't
  soften a 0 into a 1 to be nice.
- Respond in the **language of the page/request**; default English. Explain abbreviations on first
  use (VP = value proposition; CTA = call to action; CRO = conversion rate optimization).

## Ingestion — a cascade, not one path

Two different things can be missing, and they cost different amounts to recover:
- **The copy (the ideas)** — needed for extraction, scoring, and rewrites (≈90% of the job).
- **The visual (hierarchy, above-the-fold, scroll order, CTA styling)** — needed only for the
  **Clarity** layer and the "what leads the hero" judgement.

Recover **text first and cheapest**; reach for a **screenshot only when the visual layer is what's
missing.** Never assume one ingestion path — check what the user actually gave you. Work down this
cascade until you can score honestly:

1. **`web_fetch` the URL.** Read the visible copy — headline, subheads, body, button labels, proof.
2. **If the fetch is thin or blocked** — a JavaScript-rendered app yields only meta tags; a
   robots-blocked site refuses — and **Claude in Chrome is connected**, use it. It renders JS and
   loads the page in the user's own browser, returning **both** the page text (`get_page_text`) and a
   **screenshot** in one pass. This is the built-in "screen-capture" route — prefer it whenever the
   **visual layer** matters (hero order, above-the-fold, CTA styling), not just the words.
3. **Else recover the copy via `web_search`.** Search the domain plus "homepage / features / how it
   works", and read the site's own snippets, sub-pages, app-store listings, and reviews to
   reconstruct the hero, feature blocks, CTAs, and proof. This gives **text only** → **flag reduced
   proof-visibility**: testimonials, rating badges, logos, pricing, and exact CTA labels often can't
   be confirmed, so any Believability score carries an extra assumption, the Assumptions block must
   say the page wasn't seen in full, and **Clarity stays unscored**.
4. **Else ask the user** — for the **page text** if copy is what's missing, or a **full-page
   screenshot** if the visual layer is what's missing (name which, and why). Read a pasted screenshot
   by slicing it into legible horizontal sections; a full-page capture unlocks exact Clarity scores.

- **Pasted copy or screenshot up front:** work directly from what the user gave — don't assume a URL.

> An automated headless-browser screenshot (e.g. Playwright) only works where network egress is open
> (local runs); it can't reach arbitrary sites from a locked sandbox, so it is **not** a portable
> ingestion step. Claude in Chrome (step 2) and the user's own screenshot (step 4) are the reliable
> visual routes.

## Pipeline

### 1. Extract the implicit VP
From the page's actual words, pull out:
- the **outcome** the page promises,
- the **differentiation** it claims (what it says makes it better/different),
- the **proof** it actually shows.
Then **restate the page's current VP in one sentence** so the owner can see what their page really
says. Often the first useful shock is that there's no coherent VP at all — say that if it's true.
Break the page into its distinct **candidate ideas** (usually 3–8) for scoring.

### 2. Name the target customer & comparison set
These are rarely on the page, and **two axes (Benefit, Difference) can't be scored without them.**
- If they're **plausibly inferable** from the page (who it addresses, what alternatives it implies),
  proceed and record them in the **Assumptions block**.
- If they're **not inferable**, go to the confidence gate (step 7).

### 3. Score each candidate idea
Apply the shared model (`references/scoring-and-assembly.md`): **Benefit (0–2)**,
**Difference (0–2, scored twice — non-direct and direct)**, **Believability (0–2)**. Show every axis
in the score table — never just the product. Compute both products
(Benefit × Diff(non-direct) × Believability and Benefit × Diff(direct) × Believability).

### 4. Clarity/Expression check (separate page-diagnosis layer — NOT part of the score)
For each idea, additionally flag how clearly the **page expresses** it: is it instantly graspable in
the headline/subhead, or buried, abstract, jargon-heavy, or below the fold? This is a real page
failure mode that the idea-score doesn't capture, so it's tracked **separately** (🟢 instant /
🟡 needs effort / 🔴 buried or absent) and drives Clarity rewrites. It is **not multiplied into the
0–2 idea score** — the multiplied model stays the pure three-axis model.

### 5. Diagnose the bottleneck
For each idea, name the **lowest axis** (and any 0) with the specific page evidence that earns that
score. This is what to fix.

### 6. Improvement suggestions (copy-level, testable)
For each weak axis, propose a specific **rewrite hypothesis** — headline / subhead / proof / CTA —
using the levers in `references/rewrite-levers.md`, each conditioned on the stated assumptions and
framed so it can be A/B tested. Fixing a 0 outranks polishing an idea already at 4+.

### 7. Confidence gate (ask-or-handoff) — decide honestly what you can deliver
| Context available | Action |
|---|---|
| **Enough** (target customer + alternatives inferable) | Full diagnosis + improvements, all assumptions labelled. |
| **Thin** | Ask **2–3 targeted questions** first: Who is the ideal buyer? What do they compare you to? What single action should this page drive? |
| **Too thin to score Benefit/Difference honestly** | Deliver the **Clarity + Believability** fixes that *are* possible from the page, then **hand off to the vp-development skill** for the customer + differentiation work — that's VP *development*, not diagnosis. State the handoff explicitly. |

**What's fixable from the page alone vs. what needs vp-development:**
| Axis | Fixable from page alone? | Why |
|---|---|---|
| Clarity/Expression | Yes | Comprehension lives in the copy itself |
| Believability | Mostly | Add/surface proof, flag unsupported claims — visible on the page |
| Benefit | Needs target-customer knowledge | "Wanted" is relative to *who* — page rarely reveals it |
| Difference | Needs comparison-set knowledge | "Unusual" is relative to *alternatives* — page never lists them |

### 8. Prioritise
Rank fixes by **expected multiplicative score lift** (biggest jump in the direct-competition product
first). Lead with any axis currently at 0.

## Output template (always use this structure)

```
# VP Diagnosis — [page / URL]

## Assumptions
- Target customer (inferred): …
- Comparison set (inferred): direct … | non-direct/alternatives …
- Intended page action (inferred): …
> Everything below is conditioned on these. Confirm them to sharpen the diagnosis.

## The VP this page currently communicates
"[one-sentence restatement]"  — or: "No coherent value proposition is communicated."

## Score table
| On-page idea | Benefit | Diff (non-direct) | Diff (direct) | Believability | Score (direct) | Clarity | Bottleneck |
|---|---|---|---|---|---|---|---|
| … | 1 | 2 | 0 | 1 | 0 | 🔴 | Difference (direct)=0 |

## Diagnosis
[per idea: the lowest axis + the exact page evidence for the score]

## Prioritised rewrite hypotheses (copy only)
1. [Hypothesis block from rewrite-levers.md — element, current→proposed, axis lift, assumption, test]
2. …

## What needs development, not diagnosis
[If Benefit/Difference can't be scored from the page → the specific questions, and a handoff to
vp-development.]
```

Keep the numeric work in tables (the owner values structured, verifiable scoring).

## Closing step — offer a formatted PDF (always)
After delivering the diagnosis in chat, **end your turn by offering to export it as a correctly
formatted PDF** — e.g. *"Want this as a formatted PDF you can share with the team?"* Do **not** build
the PDF unless the user says yes (never generate files unasked).

When the user confirms, produce a **consulting-grade PDF, not a raw Markdown dump**:
- **Read the environment's PDF skill first** (e.g. `/mnt/skills/public/pdf/SKILL.md`) and follow it.
- Render the **score table(s) as real bordered tables** with aligned columns — never raw `|` pipes.
- Preserve the hierarchy: title block (page/URL + date) → **Assumptions** → the VP the page
  communicates → **score table** → diagnosis → **prioritised rewrite hypotheses** → what needs
  development. Clear headings, page breaks between major sections, generous margins, readable type.
- Keep the traffic-light marks (🟢🟡🔴) or render them as labels; keep every rewrite framed as a
  testable hypothesis.
- Save to the outputs location and **present the file**. Offer HTML instead only if the user asks.
