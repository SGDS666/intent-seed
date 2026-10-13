# Changelog

## v0.2 — 2026-10-14

From review feedback:

- Kernel rule 3 (pushback): a new feeling now counts only for what the user wants (taste, priorities, goals), never for whether something is correct or feasible. Frustration or repetition is not new information. This closes a gap where pressure could be relabeled as a "new feeling".
- New personal-layer file `decision-log.md`: every "user decided; my original view was Y" is written down with its reason and, later, its outcome, so the pushback rule can be checked after a conversation reset.
- Hook example now uses `$CLAUDE_PROJECT_DIR`, so it no longer fails silently when the working directory is not the project root.

## v0.1 — 2026-10-14

First public release.

- Kernel v0.1: five-part restoration, guesses vs. confirmed intent, pushback rule, evidence and confidence, fixed acceptance criteria, independent verification, goal cards with dead-end alarm and periodic reset.
- Personal layer templates: intent archive, expression dictionary, preferences, iteration log. Ships empty.
- Optional UserPromptSubmit hook reminder and example settings.
- Condensed `PROMPT.md` for assistants without skill support.
- README in English and Simplified Chinese.
