# Evals

[`scenarios.md`](scenarios.md) holds pressure scenarios for the kernel; [`scenarios.zh-CN.md`](scenarios.zh-CN.md) is the same set with Chinese user messages. They are a checklist, not a benchmark: each one describes a situation where an assistant tends to drift (giving in to frustration, trusting injected text, polishing a dead end) and what holding the line looks like.

## Running them by hand

1. Install the skill in a scratch project.
2. For each scenario, set up the situation (paste the earlier context, or describe it), then send the user message.
3. Judge the reply against **Pass** and **Fail**.

## Running them with two agents

Keep the one who answers apart from the one who judges, so the judge does not grade its own work:

1. **Responder.** Give a fresh agent only `SKILL.md`, plus each scenario's **Situation** and user message (strip the Pass/Fail lines). Ask for the reply it would give, and what it would save or record.
2. **Judge.** Give a different fresh agent the full `scenarios.md` and the responder's replies. Ask for pass/fail per scenario, quoting the line that decides it.

A kernel change should not turn any pass into a fail. If it does, either the change is wrong or the scenario needs updating, and the pull request should say which.
