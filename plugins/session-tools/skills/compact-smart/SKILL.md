---
name: compact-smart
description: Write a session-specific /compact instruction that preserves irreconstructible context — tickets, PRs, async state, rejected alternatives, the why — instead of the default generic summary. Use when the user asks to compact, says "compacta", "compact smart", "/compact-smart", or when a long session is about to lose its thread.
---

# Smart compaction

Plain `/compact` keeps *what* happened and drops *why it mattered*. This skill writes a better instruction before invoking it.

## Principle

One question filters everything: *if someone took over this repo in two minutes without the conversation, what would they need that isn't in the code or `git log`?*

Five things usually qualify — the **why** (problem, origin, success criterion), **external identifiers** (ticket IDs, PR numbers, branch names, URLs), **decisions and rejected alternatives** (so the future-self doesn't re-litigate them), **async state** (subagents, background tasks, CI, deploys — each paired with a follow-up verb, not just a mention), and **session-specific user corrections**. Everything else is reconstructible and should be dropped.

## Procedure

1. Scan the session for the five things. Be specific — real IDs, real names.
2. **Ask the user via `AskUserQuestion`** before drafting. In a long session the user has forgotten the first half, so they need to be *reminded and asked*, not handed a draft to approve.
   - **Always ask the anchor question:** "¿Cuál es el hilo principal al que vas a volver después de compactar?" Options are the 2–3 main threads you detected in the scan (use their real names), plus an "otro, te digo" escape.
   - **If you detected meaningful side threads**, ask a second question: "Además del hilo principal pasaron estas cosas. ¿Qué hago con ellas?" Options: "preserva todas", "solo las relevantes al anchor", "suéltalas".
   - Stop at two questions. Do not interrogate.
3. Draft the briefing **for an LLM consumer, not a human**. The user trusts the anchor from step 2 and will not re-read the output. Optimize for token density: labeled segments separated by periods, no narrative, no filler articles, no "we did X then Y" storytelling. Dense > readable.
4. Output the briefing inside a fenced code block prefixed with `/compact ` so the user can triple-click and paste. Example final message:
   ```
   /compact ANCHOR: ... IDS: ... DECISION: ... ASYNC: ... NEXT: ...
   ```
   Claude Code does **not** allow skills to invoke built-in `/compact` programmatically (not exposed to Skill or SlashCommand tool), so the user runs it themselves. Do not attempt to execute it via Bash or any other tool — that spawns a new session, not compacts the current one.

If the session is mid-way through an irreversible operation — deploy in progress, staged destructive command, pending force push, running migration — warn the user before starting.

## Shape

LLM-optimized format. Labeled segments, no prose flow. Example:

> ANCHOR: fix service X, root cause symptom→cause. IDS: LEAD-1234, PR#567, branch fix/.... DECISION: chose C; rejected A (reason), B (reason). ASYNC: subagent auditing [topic] → on finish [action]. CORRECTIONS: [list]. UNCOMMITTED: [files]. NEXT: [action]. DROP: tool output, resolved exploration, closed debug threads.

Omit any label whose content is empty. Keep content telegraphic — drop articles, drop verbs where a colon works, use arrows for causality and sequence.
