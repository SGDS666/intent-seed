# Contributing

Thanks for helping the seed grow. A few rules keep it small and safe.

## Kernel changes need scenarios

The kernel (K1–K9 in `skill/intent-seed/SKILL.md`) is what keeps an assistant from drifting. A change to it must:

1. say which failure it fixes, with a concrete example (anonymized);
2. add or update a scenario in [`evals/scenarios.md`](evals/scenarios.md) that fails before the change and passes after;
3. keep every existing scenario passing, or explain in the pull request why a scenario was wrong.

Run the scenarios as described in [`evals/README.md`](evals/README.md) and paste the pass/fail table into the pull request.

Keep the kernel short. A new rule should replace text, not only add it.

## No personal content

Do not commit anyone's memories, decision logs, goal cards, conversation excerpts, names, or project details, including your own. Examples must be fictional. The repository's `.gitignore` excludes `.intent-seed/`; keep it that way.

## Host-specific advice

The skill deliberately does not prescribe how each assistant stores memory, because those features change between versions. Notes about a particular host belong in an issue or discussion, not in the kernel.

## Translations

The skill replies in the user's language by itself. Documentation translations by native speakers are welcome: add `README.<language-code>.md` and link it from the language line at the top of each README. Keep the meaning, not the word order.

## Sharing how your seed grew

Use an issue or discussion (see "Sharing your seed" in the README). Anonymize first.
