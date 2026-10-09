# Agent Project Playbook

Practical workflows for building, verifying, documenting, and maintaining software with AI coding agents.

This repository is a small, dependency-free collection of prompts and the workflows that connect them. Each prompt is a self-contained instruction block you paste into an agent session so the work stays anchored to the repository's real state: its instruction files, its code, its checks, and the decisions it has already made.

The prompts support a process. They do not guarantee correct code, and they do not replace reading a diff, running the tests, or making your own engineering judgment.

## The problem this addresses

AI coding agents are fast at producing changes and unreliable at remembering why those changes were made. Working with them fails in a few predictable ways:

- The agent guesses at conventions instead of reading the instruction files that already exist.
- Context from the previous session is lost, so the next session re-derives decisions that were already settled.
- Work is reported as finished without evidence that anything was actually verified.
- Documentation and case studies get written from memory, which drifts toward flattering, unverifiable claims.

These prompts target those failure modes directly: make context explicit, keep verification honest, and separate what is known from what is assumed.

## Who it is for

- Developers using an AI coding agent on a repository they care about.
- Anyone handing a project between sessions, agents, or teammates without losing the reasoning behind it.
- Anyone who has to document or present AI-assisted work accurately, including the parts that were not verified.

## What this repository is not

- It is not a tool, package, or framework. There is nothing to install.
- It is not a benchmark. There are no success rates, comparisons, or effectiveness numbers here, because nobody has measured them.
- It is not universal. The prompts use portable wording; where a behavior is specific to one agent or tool, the prompt says so.

## Quick start

1. Pick the prompt that matches your need from the table below.
2. Read its **Required inputs** section and gather what it asks for.
3. Copy the fenced prompt block into a fresh agent session.
4. Run the checks in its **How to verify the result** section before trusting the output.

For a task that spans more than one session, start from one of the [workflows](#workflows) instead.

## Find the right prompt

| If you need to... | Use | Type of work |
| --- | --- | --- |
| Understand an existing repository before touching it | [Project discovery](prompts/01-project-discovery.md) | Read-only |
| Turn a goal into a scoped, verifiable plan | [Project planning](prompts/02-project-planning.md) | Read-only |
| Implement a bounded feature in existing conventions | [Feature delivery](prompts/03-feature-delivery.md) | Changes code |
| Reproduce and fix a defect | [Debugging](prompts/04-debugging.md) | Changes code |
| Review a diff before merging | [Change review](prompts/05-change-review.md) | Read-only |
| Set up or update project records | [Project documentation](prompts/06-project-documentation.md) | Writes docs |
| Continue work in a new session | [Session handoff](prompts/07-session-handoff.md) | Writes docs |
| Describe a project honestly for others | [Case-study generation](prompts/08-case-study-generation.md) | Writes docs |

The full index, including what each prompt expects and produces, is in [prompts/README.md](prompts/README.md).

## Workflows

Workflows sequence several prompts and describe what carries from one step to the next.

- [Start a project](workflows/start-a-project.md) - go from a rough idea to working, documented code.
- [Improve an existing project](workflows/improve-an-existing-project.md) - understand what exists, change it deliberately, and verify the change.
- [Document and present a project](workflows/document-and-present-a-project.md) - turn project records into documentation and an honest case study.

Each workflow includes a lightweight path for small changes. Not every task needs every prompt; most small changes need one or two.

## Providing context to a prompt

Prompts are designed to be copied into a fresh session, so they must be told what to work on. Before you paste a prompt, have the following ready:

- **The working directory or repository** the agent should operate in.
- **The goal** in one or two sentences, including what is explicitly out of scope.
- **Access** the task needs, such as read access to the repository or the ability to run an existing test command.
- **Verification commands**, if the project has them, such as the test, lint, or build command you expect to be run.
- **Any constraints** that are not discoverable from the files, such as an established convention that is followed by habit rather than written down.

Avoid pasting the same long block of rules on top of a prompt. Each prompt already carries a short working-agreement block that covers inspecting instructions, preserving unrelated work, separating facts from assumptions, avoiding destructive actions, and reporting checks honestly. Add your own context instead of repeating that.

### Placeholders

Some prompts contain placeholders in angle brackets, such as `<repository path>` or `<acceptance criteria>`. Replace everything inside the brackets before sending, and delete the brackets themselves. Each placeholder is explained in the prompt, outside the copyable block, so you always know what it is asking for.

Optional inputs use the same convention. They appear in each copyable block under the line "Additional context. Delete any line you are not supplying". Keep the ones you can answer and delete the rest.

Do not leave a placeholder unresolved. An agent given `<describe the change>` will invent a change, and you will get a plausible-looking result for a task nobody asked for.

## Evaluating a prompt's output

The prompt tells the agent how to report. Your job is to check the report:

- **Claims about the codebase.** If the agent says a function validates input, open the function. A prompt that demands file references makes this cheap.
- **Claimed verification.** Re-run the commands the agent says it ran. If it lists a skipped or unavailable check, treat the corresponding behavior as unverified.
- **Assumption labels.** Confirm that assumptions are marked as assumptions rather than presented as facts.
- **Unrelated changes.** Run `git status` and `git diff --stat`. Anything outside the stated scope is a defect in the run, not a bonus.
- **Placeholders.** Search the output for the angle brackets you were supposed to replace. If any remain, the agent was working from an incomplete task description.
- **Confidence.** Treat fluent, unqualified summaries with more suspicion than ones that name what was checked, what was skipped, and what remains unknown.

## Validation status

Every prompt and workflow in this repository carries one of three labels:

- **Draft** - written and reviewed for structure, but not yet exercised on a real task.
- **Used on a real task** - executed on an actual project, with the context and observed results recorded.
- **Repeatedly evaluated** - used across multiple documented tasks, with evidence from each.

Structural checks, editorial review, and the fictional example in `examples/` are not evidence and cannot upgrade a prompt beyond **Draft**. All prompts and workflows in this repository are currently **Draft** as of 2026-10-09.

What this means for you: the prompts are organized to be useful and readable, and that is all that is being claimed. Their effect on real work is unproven. [CONTRIBUTING.md](CONTRIBUTING.md) explains what counts as evidence for an upgrade.

## Repository layout

```
.
|-- README.md                 This file
|-- AGENTS.md                 Rules for AI agents working in this repository
|-- CONTRIBUTING.md           How to add or change prompts, and what counts as evidence
|-- CHANGELOG.md              Meaningful repository changes
|-- prompts/                  The eight prompts, plus an index
|-- workflows/                Three sequences that connect prompts
|-- examples/                 A fictional project used to show documentation structure
```

## Contributing

Contributions are welcome, and the bar is evidence rather than enthusiasm. See [CONTRIBUTING.md](CONTRIBUTING.md) for how to propose a prompt, how to avoid duplicating an existing one, what a tested example must include, and how to report a prompt that failed.

Two rules matter more than the rest:

- Remove secrets and private information before contributing anything derived from real work.
- Do not upgrade a prompt's validation status without recorded evidence of a real run.

## Attribution: human and AI contributions

This repository is explicitly about AI-assisted work, so it states its own contributions plainly.

- The structure, product direction, and editorial decisions came from the repository owner.
- The prompt text and documentation were drafted with an AI coding agent, then reviewed and edited by a human.
- No prompt here has been validated as more effective than any alternative. There are no performance claims.
- No example in this repository describes a real project. The example is fictional and labeled as such. See [examples/README.md](examples/README.md).

If you contribute a tested example, describe your own role and your agent's role honestly, including what you reviewed and what you did not.

## License

No license has been selected for this repository yet. Until one is chosen, the default is that all rights are reserved, and content here should not be reused or redistributed.

Selecting a license is an owner decision that should be made before treating this repository as publicly released or open source. This README deliberately does not describe the repository as open source or licensed, because neither is currently true.
