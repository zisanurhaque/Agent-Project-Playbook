# Contributing

This repository is a collection of prompts that support a specific process: understand the work, keep scope honest, verify what you claim, and document decisions instead of reconstructing them later. Contributions that strengthen that process are welcome. Contributions that add volume or marketing are not.

## Before you start

1. Read [README.md](README.md) and [prompts/README.md](prompts/README.md).
2. Check whether an existing prompt already covers your need. Most requests are better as an edit to an existing prompt than as a new file.
3. Open an issue describing the problem your change solves, if you are not sure. A described failure case is more useful than a proposed file.

## Adding or changing a prompt

A prompt document uses this structure, and new prompts should follow it:

1. Purpose.
2. When to use it.
3. Required inputs and access.
4. Optional inputs.
5. The complete copyable prompt in a fenced block.
6. Expected outputs.
7. How to verify the result.
8. Limitations or common failure modes.
9. Validation status.

Rules for the prompt itself:

- It must work when copied into a fresh session with no prior context.
- It must contain a short working-agreement block covering instruction files, scope discipline, facts versus assumptions, destructive and publishing actions, and honest reporting.
- Placeholders use angle brackets and must be explained outside the copyable block.
- It must not promise correct code, guarantee an outcome, or imply universal compatibility.

If you change a prompt's wording in a way that affects observable behavior, say so in the pull request description, because it may invalidate an existing validation record.

## Preventing duplication

Before adding a prompt, answer these questions in your pull request:

- Which existing prompt is closest to this one?
- Why is an edit to that prompt insufficient?
- What does this prompt do that no other prompt does?

Two prompts that share most of their instructions should be merged. Repeating a large instruction block across prompts is a maintenance problem: a fix in one place must be made in every place it was copied.

## Evidence for a tested example

Validation status labels are defined in the [validation status section of the README](README.md). To move a prompt from `Draft` to `Used on a real task`, include a record containing:

- **Task**: what was actually being built, fixed, or reviewed, in enough detail that the reader understands the context.
- **Prompt as used**: the exact version of the prompt, including the placeholders you filled in.
- **Environment**: the agent or tool and model, and the repository's shape, in general terms.
- **Observed result**: what the agent produced and what you did with it. Include the parts that were wrong.
- **Verification**: which checks you ran, their results, and which checks you skipped or could not run.
- **Assessment**: what the prompt did well, what it missed, and what you would change.

To reach `Repeatedly evaluated`, include records from multiple distinct tasks. Point to the records from the prompt's validation section using relative links.

Structural checks, editorial review, reading the prompt carefully, and the fictional example in `examples/` are not evidence. They cannot upgrade a status.

## Reporting a failure case

Failures are useful and are not a reason to be embarrassed. A good failure report includes the task, the prompt version, what the agent did, and the check that revealed the problem. If a prompt reliably produces a bad result, open an issue; a prompt that causes harm should be corrected or removed rather than kept for completeness.

## Removing secrets and private information

Before contributing anything derived from real work:

- Remove credentials, tokens, API keys, connection strings, and environment files.
- Remove customer names, personal data, internal hostnames, private repository URLs, and proprietary code that you are not permitted to share.
- Replace project specifics with a description of their shape, unless the specifics are necessary to make the example useful and you have permission to share them.
- Do not paste raw logs without reading them. Logs routinely contain tokens, identifiers, and request payloads.
- If you are unsure whether something is shareable, leave it out.

## Observed results versus opinions

Keep the two separate in everything you contribute.

- **Observed**: what you ran, what it printed, what changed in the files. Another person can check it.
- **Opinion**: whether the output was good, whether the approach was elegant, whether it saved time.

Opinions belong in the discussion, not in the evidence. "The agent missed the retry handling and I added it in review" is an observation. "The agent was much better than my usual workflow" is an opinion, and it needs measurement before it can be stated as a result.

## Style

- Lowercase kebab-case filenames.
- Relative links; no absolute URLs for content in this repository.
- Restrained formatting: headings, short lists, and tables where they help. No decorative badges, no emoji headers.
- Fenced code blocks for anything copyable, with a language hint.

## Changelog

Add an entry to [CHANGELOG.md](CHANGELOG.md) for meaningful changes: a new prompt, a changed prompt contract, a new workflow, or a documentation restructuring. Do not log every edit.
