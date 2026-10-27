# VP Toolkit — Claude Skills for value propositions

Two Claude Skills that answer the only question a landing page has to answer: **"Why should I buy from you?"**

- **`vp-diagnosis`** — give it a page (URL, pasted copy, or a screenshot). It extracts the value proposition the page actually communicates, scores each idea on a harsh 0–2 model, finds the one axis that's killing it, and returns prioritised, testable copy-rewrite hypotheses.
- **`vp-development`** — an interview that *builds* a value proposition from scratch: target customer → competitors → benefits → differences → proof → assembly. It elicits your answers; it never invents them.

Both share the same scoring, so a VP built by `vp-development` re-scores in `vp-diagnosis` with no translation.

## Requirements
- A **paid Claude plan** — Pro, Max, Team, or Enterprise.
- **Code execution / file creation enabled** (Settings → Capabilities). Custom Skills don't run on the Free plan.

## Install

### A. Claude.ai (web or desktop) — for most people
1. Download **`dist/vp-diagnosis.zip`** and **`dist/vp-development.zip`** (or grab them from Releases).
2. In Claude, enable **Code execution and file creation** under Settings → Capabilities.
3. Go to **Customize → Skills → + → Create skill → Upload a skill** and upload **one zip**. Repeat for the second. (The uploader takes one skill per zip.)
4. Enable both in the list. Done — just mention them in a chat.

### B. Claude Code — for developers
```bash
/plugin marketplace add YOUR-USERNAME/vp-toolkit
/plugin install vp-toolkit@jochen-grunbeck
```
The two skills then resolve as `vp-toolkit:vp-diagnosis` and `vp-toolkit:vp-development`.
Validate locally before publishing with `claude plugin validate .`.

### C. Claude API
Upload each skill folder as a custom Skill via the Skills API.

## How to use
- **Diagnose a page:** *"Use vp-diagnosis on https://your-page.com"* — or paste the copy / a full-page screenshot.
- **Build a VP:** *"Use vp-development to build my value proposition."* Then answer the interview honestly.

Each skill ends by offering a formatted PDF of the result.

## The scoring, in one line
Every idea gets three marks (Benefit × Difference × Believability, 0–2 each), multiplied to a max of 8. A single 0 zeroes the idea. Aim for three ideas that score 8.

## Notes
- Provided **as-is**. Feedback and issues welcome.
- Use Skills only from sources you trust — the full source is right here to read.

*Built by Jochen Grünbeck — Conversion Psychologist & co-author of* Smart Persuasion.
