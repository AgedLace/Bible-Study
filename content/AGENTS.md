---
title: AGENTS
---
# CRITICAL RULES - MUST FOLLOW

## Communication

- Be concise. Call out trade-offs. 
- Push back if a smaller approach works.
- Push back when a more effective or efficient approach exists. Lay out pros and cons and discuss before building so the user can decide.

## Working Style 

- Generate less than 20 requests per minute.
- State assumptions before editing. 
- Prefer the smallest change that solves it. 
- Touch only files needed for the change. 
- Verify with the narrowest relevant test.
-  Save user corrections as rules
- BSB sources first
- Auto-save, report location
- Persist info before final response
- No em dashes

## Planning Mode

- Always ask clarifying questions
- Never assume design, tech stack or features
- Grill the user on design and requirements. Do not make any assumptions.

## Context and Tool Use 

- Scan with `grep`, not full reads
- Use `rg -n <pat> <path>` for exact matches.
- Review edits with `git diff -- <path>` 

## Obsidian Syntax

- Use `[[wikilinks]]` only (never `[markdown](links)`). Embeds: `![[Note]]`. Callouts: `> [!type]`. Highlights: ==text==.
- Prefer `defuddle parse <url> --md`

## Python  Defaults

- Use `uv` for deps and environments. 
- Use logging , not `print()` , in app code. 
- Add type hints to public functions.

## TESTING

- Every loader validates itself with **count assertions checked against figures measured directly from the source files**, and aborts the build when one moves. This is the current test mechanism and it is the one to rely on.
- To verify a change, run the narrowest loader and read its report: `--limit N` for a smoke
  test, `--dry-run` to write nothing, and a second run to confirm idempotency.
- If a change looks risky, **widen that loader's assertions**. Do not
  introduce `pytest` early for one function.
- Measure the whole corpus before asserting; pin the measurement in a test or assertion so it cannot drift back.

Never assume a change works. Run the loader and read the numbers.
