# Prompts

Eight prompts for working with an AI coding agent on a real repository. Each one is self-contained: read its short preamble, copy the fenced block into a fresh session, and replace any placeholders.

## How to use these prompts

1. Pick a prompt from the index below.
2. Read **Required inputs** and gather what the prompt needs before you start.
3. Read **Optional inputs** and add anything that is true but not discoverable from the files, such as a convention that is followed by habit.
4. Copy the fenced prompt block, fill in the placeholders, and delete the angle brackets.
5. After the run, work through **How to verify the result**. That section exists because the prompt's own report is not evidence.

If a task spans more than one session, use a [workflow](../workflows/) instead of a single prompt.

## Shared working agreement

Every prompt carries a short block of the same rules. They are repeated because each prompt has to stand alone when copied, and they are kept short so they do not crowd out the task-specific instructions. The rules are:

- Read the repository's instruction files and follow the most specific one that applies.
- Inspect the relevant files rather than trusting a description of them.
- Leave unrelated files and behavior alone.
- Label statements as facts, assumptions, or proposals.
- Ask a focused question only when missing information blocks a decision; otherwise state the assumption and continue.
- Use the verification the project already has, and report what was run, skipped, or unavailable.
- Never invent evidence or history.
- Do not take destructive or publishing actions without explicit authorization.

Do not paste a longer version of these rules on top of a prompt. Add your own project context instead; see [Providing context](../README.md#providing-context-to-a-prompt).

## Placeholders

Placeholders appear inside angle brackets, for example `<repository path>`. Each one is explained in the prompt outside the copyable block. Replace the text inside the brackets and remove the brackets themselves.

Optional inputs also use angle brackets. In each copyable block they appear as a short list under the line "Additional context. Delete any line you are not supplying". Supply the ones you have, and delete the lines you do not; the prompt is written to work without them.

If you leave a placeholder in place, the agent will invent a plausible value for it. That is the single most common way to get a confident, useless result.

## Prompt index

| Prompt | Use when | Produces | Write access | Validation |
| --- | --- | --- | --- | --- |
| [01 Project discovery](01-project-discovery.md) | You need a factual baseline of a repository before changing it | A discovery report with architecture, commands, conventions, and unknowns | None | Draft |
| [02 Project planning](02-project-planning.md) | A goal needs to become a scoped, verifiable plan | A plan with scope, completion criteria, dependencies, and a verification plan | None | Draft |
| [03 Feature delivery](03-feature-delivery.md) | You have a bounded feature to implement | Code changes, verification results, and a change summary | Code and tests | Draft |
| [04 Debugging](04-debugging.md) | Something is broken and the cause is not yet established | A reproduction, a root-cause explanation, a focused fix, and verification | Code and tests | Draft |
| [05 Change review](05-change-review.md) | A diff needs review before it is accepted | Findings with severity, file references, and verification gaps | None | Draft |
| [06 Project documentation](06-project-documentation.md) | Project records need to be created or maintained | `PROJECT.md`, `WORKLOG.md`, `HANDOFF.md`, `CASE-STUDY.md`, and an evidence directory | Documentation | Draft |
| [07 Session handoff](07-session-handoff.md) | Work continues in a new session, or with another person | A short continuation brief with status, decisions, blockers, and next steps | Documentation | Draft |
| [08 Case-study generation](08-case-study-generation.md) | A project needs to be described honestly for an outside reader | A case study that separates roles, intentions, results, and unknowns | Documentation | Draft |

## Validation status

Each prompt ends with a validation status:

- **Draft** - written and reviewed for structure, but not yet exercised on a real task.
- **Used on a real task** - executed on an actual project, with the context and observed results recorded.
- **Repeatedly evaluated** - used across multiple documented tasks, with evidence from each.

All eight prompts and all three workflows are **Draft** as of 2026-10-09. That is not a placeholder value; it reflects the current absence of recorded real-world runs. Structural review, careful reading, and the fictional example in `examples/` do not count as evidence. [`CONTRIBUTING.md`](../CONTRIBUTING.md) describes what does.

## Portability

The prompts use portable wording. They refer to "the repository's instruction files" rather than to one specific file name, and they say "the project's existing checks" rather than naming a command. Where a behavior genuinely differs between tools, the prompt says so instead of assuming.

They also avoid assuming capabilities. A prompt asks the agent to report a check as unavailable rather than to improvise one, so a prompt still works in a session that cannot run tests, cannot reach the network, or cannot write files.

## Adding a prompt

See [CONTRIBUTING.md](../CONTRIBUTING.md). Two rules apply before adding a ninth prompt: it must have a distinct purpose that no existing prompt covers, and it must not duplicate a large instruction block that already lives elsewhere.
