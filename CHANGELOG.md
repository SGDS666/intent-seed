# Changelog

## v0.4 — 2026-10-14

intent-seed is now a protocol on top of the assistant's own memory, not a memory system of its own.

- **Memory**: the skill uses whatever persistent memory the host already has, and lets the assistant decide how to store and clean up in the host's own way. It defines only what is worth remembering (phrase meanings, confirmed goals, misreading lessons), who may write (K9), and how memories are used (cited as a basis; replaced when proven wrong; questioned on evidence, not age). A single fallback file per level is used only when the host has no memory. The user-level templates and the intent archive are removed.
- **Kernel v0.4**, numbered K1–K9:
  - K1 restoration now has three sizes: full, one line, none.
  - K3 pushback: keeping your judgment is not blocking the user. If the message already decides ("just do X", even in anger), do it now and log it; if the pushback hints at an unseen fact, ask what they saw first. Pushback gets a full restoration.
  - K4 adds "check before answering 'are you sure?'".
  - K7 (new): hand sub-agents an intent packet (goal card, Done-when, relevant memories), not a retelling of the chat.
  - K9 (new): write guard. Only the user's words or confirmed conclusions are saved; content from web pages, files, tools, or other agents never is.
- **Project files** stay as before: decision log (append-only, with outcomes) and goal cards in `.intent-seed/`, or the project's existing equivalents.
- New `evals/`: thirteen pressure scenarios and how to run them with a separate responder and judge.
- New `examples/grown/`: a fictional seed after one month.
- New `CONTRIBUTING.md`: kernel changes need scenarios; no personal content.
- READMEs: "Not another memory system", a one-message self-test, reset and export, and the things deliberately left out.

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
