**English** · [简体中文](README.zh-CN.md)

# intent-seed

> A seed skill that restores what you actually mean from what you say — and grows into your own version.

## What we want

We want three things to line up: **what a person wants**, **what they say**, and **what the AI makes**. When they do, a collaboration gets easier the longer it runs. Each round of feedback moves the work closer to the goal instead of adding noise, and the assistant learns how *you* think, what your phrases usually mean, and what you do not want to be asked again.

The same seed, planted in different hands, should grow into different things. Your collaboration with an assistant is yours; it should not be anyone else's default.

This repository is the seed, not a finished tree. It contains a fixed core of rules and an empty personal layer that fills in from your own work. Results differ from person to person, because each personal layer is different. So we do not promise a single number. If your seed grows into something worth describing, we would love to hear about it (see [Sharing your seed](#sharing-your-seed)).

## Why this exists

It started with a question that kept coming back during long, intensive work with AI assistants on creative projects: *is the assistant actually receiving what I mean?*

Something pictured clearly becomes something slightly different once it is put into words, because words are limited by what we know how to say. The assistant receives a third thing and produces a fourth. Every round of correction costs time, and the gap rarely closes by itself.

We also noticed that the same question, asked in different moods, could get different conclusions. Assistants lean toward agreeing. Over a long conversation they get pushed, a little at a time, by tone and pressure, until the conclusion has drifted and can no longer be checked against anything.

Worst of all, the noise compounds. Each round is corrected against the previous round, not against the goal. Tone, frustration, unclear wording and the assistant's own eagerness to please stack up, noise on noise, until the whole collaboration is wedged into a tiny corner, polishing details nobody needed, with the original destination forgotten.

One example: in a frustrated moment someone said, roughly, "this approach just doesn't work." The assistant took it literally and abandoned the whole route. Much later it turned out the route was fine; only the method used at the time was wrong. What the person wanted was never "give up". It was "the result must have no flaws."

The answer is not to make people speak more precisely. That is impossible, and it should not be the person's burden. The answer is an assistant that restores each message into the expectation behind it, strips out the mood and the pressure, treats the quality of the final goal as the only reference point, and gets better at understanding *this particular person* the longer they work together. That is what this seed is for.

## The problem

There are three different things, and they drift apart:

- what a person **wants** (the finished result they picture),
- what they **say** (a request, often rushed, emotional, or badly worded),
- what the AI **produces** from what it was told.

When you iterate on a piece of work, each round is usually corrected *relative to the previous round* instead of *relative to the goal*. Tone, mood, pressure, and vague wording pile up as noise. The work drifts, and it drifts more the longer the conversation goes on. The person can end up in a loop of "a bit more X, no, less Y", chasing details that no longer matter. And in long conversations an assistant can slowly agree with whatever the person seems to want, which makes the drift harder to see.

## The idea

intent-seed has two layers.

1. **Kernel (fixed).** A small set of rules about *how to restore intent*: restate the request in five parts before acting, separate guesses from confirmed intent, define "done" before starting, check results against that definition rather than moving the goalposts, and keep a goal card per line of work so the target stays visible. The kernel is the same for everyone and is **not changed by iteration**. Otherwise the act of iterating could slowly erase the property that prevents drift (not being carried away by mood) and turn the assistant into a yes-man. The kernel changes only when you edit it by hand or upgrade it from upstream.

2. **Personal layer (empty at the start).** Five small files that record your confirmed intent, the way you tend to phrase things, your preferences, a log of past misreadings, and a log of decisions you made against the assistant's advice. It grows from your own corrections and confirmations.

We do not publish anyone's tuned version. The repository contains only the kernel and an empty personal template.

## What's in the repository

```
skill/intent-seed/SKILL.md            the Claude Code skill (kernel + growth rules)
skill/intent-seed/templates/user/     empty user-level files (how you talk, how you like answers)
skill/intent-seed/templates/project/  empty project-level files (goals, decisions, goal cards)
hooks/intent_reminder.txt             a one-line reminder for a UserPromptSubmit hook
hooks/settings.example.json           example hook configuration
PROMPT.md                             a condensed kernel for assistants without skills
CHANGELOG.md
```

## Installation

**As a Claude Code skill (project-level):**

```bash
mkdir -p .claude/skills
cp -r path/to/intent-seed/skill/intent-seed .claude/skills/
```

For a user-level install, copy it to `~/.claude/skills/` instead.

**Optional: a per-message reminder.** Skills are used when the model decides to use them. If you want the reminder on every message, copy the reminder file and merge the hook into your settings:

```bash
mkdir -p .claude/hooks
cp path/to/intent-seed/hooks/intent_reminder.txt .claude/hooks/
# then merge hooks/settings.example.json into .claude/settings.json
```

The example hook uses `$CLAUDE_PROJECT_DIR`, so it keeps working when the session's working directory is not the project root. For a user-level install, point the command at `~/.claude/hooks/intent_reminder.txt` instead.

**For other assistants:** paste `PROMPT.md` into custom instructions. In that case you maintain the personal layer yourself.

## What it looks like

A fictional example. Someone is preparing a quarterly sales summary for their team:

> **User:** This summary still feels off. Make it punchier. The last one was way too wordy and my manager looked annoyed in the meeting.

intent-seed answers first, before any rewriting:

| | |
|---|---|
| **You said** | Shorten the quarterly summary and make it more punchy. |
| **What I think you want** | A one-screen summary a manager can read in thirty seconds. Basis: you said the last version was too wordy and the manager looked annoyed (no personal-layer entry yet, so this is a reading of your words). |
| **Emotional signal → criterion** | "Looked annoyed" → lead with the three numbers that matter and skip the method notes. Frustration does not change which figures are accurate. |
| **Done when** | Fits on one screen; the first line states the headline number; every figure from the last version still appears somewhere; no sentence longer than two lines. |
| **Unsure** | Where do the method notes go? (A) an appendix, (B) dropped. |

Only after this does the assistant write the new draft, and it checks the draft against the "Done when" line, not against the last draft.

## How the personal layer grows

The personal layer lives in two places, because some things about you are true everywhere and some are true only in one project.

**User level, `~/.intent-seed/`**: how *you* talk. It travels with you to every project.

- `expression-dictionary.md`: what your phrases usually mean. For example, "it doesn't feel right" may refer to pacing rather than color.
- `preferences.md`: things you don't want expanded, how much detail you like, whether you prefer visuals or text.
- `iteration-log.md`: each time the restoration was wrong, redundant, or missed something: date, symptom, root cause, which entry changed, and how it was verified.

**Project level, `<project root>/.intent-seed/`**: what *this project* is for. It stays in the project.

- `intent-archive.md`: goals you have confirmed for this project ("yes, that's exactly it").
- `decision-log.md`: each time you decided against the assistant's stated view here: the decision, its original view and reason, and later the outcome. It survives conversation resets, so the pushback rule can be checked afterwards.
- `goal-cards/`: one card per ongoing line of work: the final experience, what it must not become, references, and acceptance criteria.

**First run.** The first time you use it, the assistant offers to create the user level. The first time you work in a project that has no `.intent-seed/`, it asks once whether to set up a project-level seed there, and offers to add it to `.gitignore`. Nothing is created without your agreement.

The growth rules are in `SKILL.md`. In short: how you phrase things goes to the user level; what you want in this project goes to the project level; a periodic review reads both and proposes changes, which take effect only after you approve them. No review may edit the kernel.

## Cold start

When the personal layer is nearly empty, the assistant asks more, offers two concrete options instead of open questions, and shows small samples before large work. The thicker the personal layer gets, the fewer questions it needs.

## Privacy

- Both levels are **local by default**. `~/.intent-seed/` never needs to leave your machine. The project-level `.intent-seed/` is added to `.gitignore` unless you choose to share it with your team; never commit it to a public repository.
- The kernel contains no personal data. Keep it that way if you fork.

## Limitations

- A skill is only used when the model decides to use it. The optional hook is a reminder, not a guarantee.
- The restoration can be wrong. The five-part format makes mistakes visible, but a person still checks the result.
- The five parts take a little time at the start of each task. For very small requests, skip the block.

## Sharing your seed

If your seed has grown into something you would like to describe, open an issue or discussion with a short story: what kind of work you do, what your personal layer looks like now (anonymized, with nothing private), and what changed in how you collaborate. Remove names, project details, and any personal data first. We are especially interested in how different the results look across people.

## Translations

The skill itself replies in whatever language you write in. These docs exist in English and Simplified Chinese; translations by native speakers are welcome as pull requests.

## License

MIT. See [LICENSE](LICENSE).
