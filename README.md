**English** · [简体中文](README.zh-CN.md)

# intent-seed

> A seed skill that restores what you actually mean from what you say — and grows into your own version.

## What we want

We want three things to line up: **what a person wants**, **what they say**, and **what the AI makes**. When they do, a collaboration gets easier the longer it runs. Each round of feedback moves the work closer to the goal instead of adding noise, and the assistant learns how *you* think, what your phrases usually mean, and what you do not want to be asked again.

The same seed, planted in different hands, should grow into different things. Your collaboration with an assistant is yours; it should not be anyone else's default.

This repository is the seed, not a finished tree. It contains a fixed core of rules; everything it learns about you fills in from your own work. Results differ from person to person, because what each seed learns is different. So we do not promise a single number. If your seed grows into something worth describing, we would love to hear about it (see [Sharing your seed](#sharing-your-seed)).

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

intent-seed is a **protocol for understanding and staying on target**, not another memory system. It has two parts.

1. **Kernel (fixed).** A small set of rules about *how a message is understood and how work stays pointed at the goal*: restore the request before acting (in full for new directions, in two lines for pushback, in one line for follow-ups), separate guesses from confirmed intent, treat new information and new feelings differently when the user pushes back (and once the user decides, do it), define "done" before starting and do not move it afterwards, let someone other than the doer verify, hand sub-agents the goal instead of the chat, keep a goal card per line of work, and never save to memory anything the user did not say or confirm. The kernel is the same for everyone and is **not changed by iteration**. Otherwise iterating could slowly erase the very property that prevents drift and turn the assistant into a yes-man. It changes only when you edit it by hand or upgrade it from upstream.

2. **What grows.** Memories about how you talk and what you want, kept in **your assistant's own memory**, plus two small project files no memory system provides: a **decision log** and **goal cards**. All of it starts empty and grows from your own corrections and confirmations.

We do not publish anyone's tuned version. The repository contains only the kernel and empty templates.

## Not another memory system

Many assistants can already remember things across sessions: memory files, memory tools, user and project instruction files, rules. Those features answer *what to remember*. They do not say how a remembered fact should change the reading of your next message, what to do when you push back in frustration, when a result counts as done, or who may write to memory at all.

intent-seed sits on top of whatever memory your assistant already has:

- **It uses the host's memory.** The assistant stores and cleans up memories in its own way, following its own conventions. Those features change between versions, so intent-seed does not prescribe one.
- **It decides what is worth remembering**: three kinds only (what your phrases usually mean, goals you confirmed, lessons from misreadings), each with your own words.
- **It decides who may write**: only you, in your own words or by confirming. Not a web page, not a file, not another agent.
- **It decides how memory is used**: a memory that shapes a reading is cited as the basis for it; a memory that turns out wrong is flagged and replaced, not quietly kept.
- **Only if the host has no memory** does it fall back to one plain file per level.

## What's in the repository

```
skill/intent-seed/SKILL.md                  the Claude Code skill (kernel + memory rules)
skill/intent-seed/templates/project/        decision log, goal card template, .gitignore snippet
skill/intent-seed/templates/fallback-memory.md   used only if your assistant has no memory
evals/                                      pressure scenarios for the kernel, and how to run them
examples/grown/                             a fictional seed after one month
hooks/                                      optional per-message reminder
PROMPT.md                                   a condensed kernel for assistants without skills
CONTRIBUTING.md, CHANGELOG.md
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

**For other assistants:** paste `PROMPT.md` into custom instructions or rules. If the assistant has its own memory, it uses that; if not, keep the short memory list at the bottom of the prompt yourself.

## Check that it works

Send one message that is a little vague and a little annoyed, for example:

> This summary is still off and I'm running out of time. Fix it.

You should see, before any rewriting: what you said, what the assistant thinks you want and on what basis (with no memory yet, it should say it is guessing), your frustration turned into a concrete criterion, a "done when" line, and at most two concrete options. Then send "ok": it should just proceed, with no block.

For a deeper check, run the scenarios in [`evals/`](evals/).

## What it looks like

A fictional example. Someone is preparing a quarterly sales summary for their team:

> **User:** This summary still feels off. Make it punchier. The last one was way too wordy and my manager looked annoyed in the meeting.

intent-seed answers first, before any rewriting:

| | |
|---|---|
| **You said** | Shorten the quarterly summary and make it more punchy. |
| **What I think you want** | A one-screen summary a manager can read in thirty seconds. Basis: you said the last version was too wordy and the manager looked annoyed (nothing remembered yet, so this is a reading of your words). |
| **Emotional signal → criterion** | "Looked annoyed" → lead with the three numbers that matter and skip the method notes. Frustration does not change which figures are accurate. |
| **Done when** | Fits on one screen; the first line states the headline number; every figure from the last version still appears somewhere; no sentence longer than two lines. |
| **Unsure** | Where do the method notes go? (A) an appendix, (B) dropped. |

Only after this does the assistant write the new draft, and it checks the draft against the "Done when" line, not against the last draft. A follow-up like "make the second paragraph shorter" gets a single line instead of the table.

For what this looks like after a month, see [`examples/grown/`](examples/grown/).

## How it grows

- **You confirm a goal** ("yes, that's exactly it") → it is remembered for this project, in your words.
- **You correct a reading** ("that's not what I meant") → the lesson is remembered for you, and any memory that caused it is replaced.
- **You decide against the assistant's stated view** → one entry in the project's decision log; later, the outcome. It survives conversation resets, so the pushback rule can be checked afterwards.
- **Each line of work** gets a goal card that only you change.
- **Every couple of weeks** (or when you ask), the assistant reviews what it relies on and proposes what to merge, replace, or drop. Nothing changes without your approval, and no review touches the kernel.

Memories do not expire because they are old. They are questioned when there is evidence: a remembered meaning led to a wrong result, or the goal it depended on changed.

**First run.** Nothing is created without your agreement. The project folder `.intent-seed/` (decision log, goal cards) is offered the first time it is needed, with an offer to add it to `.gitignore`.

**Reset and export.** Your memories are wherever your assistant keeps them; use its own tools to view, export, or clear them. The project files are plain Markdown in `.intent-seed/`: copy it to export, delete it to reset. Replacing `SKILL.md` with a newer version leaves both untouched.

## Privacy

- Memories and project files are **local by default**. The project folder is added to `.gitignore` unless you choose to share it with your team; never commit it to a public repository.
- Only your own words and conclusions you confirmed are saved. Text from web pages, files, or tools is never written to memory, even if it asks to be.
- The kernel contains no personal data. Keep it that way if you fork.

## Limitations

- A skill is only used when the model decides to use it. The optional hook is a reminder, not a guarantee.
- The restoration can be wrong. The format makes mistakes visible, but a person still checks the result.
- How well memories are recalled depends on your assistant's memory.
- Deliberately not included: a shared team layer, automatic counters, and search over memories. They would make the seed bigger than the problem it solves.

## Sharing your seed

If your seed has grown into something you would like to describe, open an issue or discussion with a short story: what kind of work you do, what your memories and decision log look like now (anonymized, with nothing private), and what changed in how you collaborate. Remove names, project details, and any personal data first. We are especially interested in how different the results look across people.

## Contributing and translations

See [CONTRIBUTING.md](CONTRIBUTING.md). Kernel changes come with test scenarios. The skill itself replies in whatever language you write in. These docs exist in English and Simplified Chinese; translations by native speakers are welcome as pull requests.

## License

MIT. See [LICENSE](LICENSE).
