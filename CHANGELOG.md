# Changelog

## v0.3 — 2026-10-14

- The personal layer now has two levels. **User level** (`~/.intent-seed/`): expression dictionary, preferences, iteration log; it learns how you talk and travels with you. **Project level** (`<project root>/.intent-seed/`): intent archive, decision log, goal cards, and optional project-only meanings; it stays in the project.
- First run: the assistant offers to create the user level once, and asks once per project whether to set up a project-level seed (and add it to `.gitignore`). Nothing is created without agreement; a refusal is remembered.
- Goal cards move into the project level (`.intent-seed/goal-cards/`), with a template.
- Templates reorganised into `templates/user/` and `templates/project/`.

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
