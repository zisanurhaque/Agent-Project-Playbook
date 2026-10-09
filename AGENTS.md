# AGENTS.md

Instructions for AI agents working in this repository. Read this file before making changes.

## What this repository is

The Agent Project Playbook is a documentation-only repository. It contains Markdown prompts, workflows, and one fictional example. There is no build step, no package manifest, no website, and no dependencies. Do not add any.

## Purpose to preserve

The repository exists to make AI-assisted software work more organized, verifiable, and honestly documented. Every change should serve that purpose.

Keep the tone factual and restrained. Do not add marketing language, superlatives, badge collections, decorative emoji, invented statistics, testimonials, personal biography, credentials, or contact information. Do not describe the repository, or any prompt in it, as the best, ultimate, or complete collection.

## Rules

### Content

1. Keep every copyable prompt self-contained. A reader must be able to copy one fenced block into a fresh agent session and have it work, without needing to have read another prompt first.
2. Keep the shared working-agreement block short. Each prompt carries a compact block covering instruction files, scope discipline, facts versus assumptions, destructive actions, and honest reporting. Do not expand it into a large repeated instruction dump; adapt it minimally and keep the task-specific instructions in the task-specific sections.
3. Do not claim universal compatibility. Use portable wording, and name a tool-specific behavior only when it actually differs between tools.
4. Do not claim that a prompt guarantees correct code, replaces human review, or has been measured to work better than alternatives.
5. Keep prompts distinct. If two prompts would contain substantially the same instructions, merge or narrow them instead of duplicating.

### Examples

6. Label every example prominently as illustrative and fictional. Examples must not describe real projects, real users, real feedback, real metrics, real test results, or real deployment outcomes.
7. Never fabricate evidence. Do not invent test runs, command output, measurements, screenshots, analytics, dates of real work, or historical facts. If a fact was not observed, describe it as unknown.

### Validation status

8. Use only these labels: `Draft`, `Used on a real task`, and `Repeatedly evaluated`.
9. New prompts and workflows start as `Draft`. Structural checks, editorial review, and fictional examples are not evidence and cannot upgrade a status.
10. Upgrade a status only when the repository contains a recorded run: the task, the context, the observed result, and what was verified. Never upgrade on the basis of a summary written by an agent.

### Structure and links

11. Use lowercase kebab-case filenames and relative links. Do not use absolute URLs for content inside this repository.
12. Keep the structure small. Add a file only when it has a distinct purpose; otherwise extend an existing file.
13. When a file is added, renamed, or removed, update every index that references it: the table in `README.md`, the index in `prompts/README.md`, the workflow documents, and any cross-links between prompts.
14. Keep the nine-section format used by every prompt document. If you add a prompt, follow it.

### Scope discipline

15. Preserve existing content and unrelated work. Make focused, reviewable changes rather than broad rewrites.
16. Do not add tooling, scripts, configuration, or dependencies for documentation checks that can be performed by reading the files.
17. Do not add a license file, and do not describe the repository as licensed or open source. License selection is an owner decision.
18. Do not push, publish, or change remote repository settings unless the owner explicitly asks.

## Checks before finishing a change

- Every relative link resolves to a file that exists.
- Every prompt in `prompts/` appears in `prompts/README.md` and in the root README table.
- Copyable prompt blocks are complete and contain no unresolved placeholders other than intentional, explained input examples.
- No unsupported claim, invented evidence, or mislabeled example was introduced.
- No new dependency, build step, or overly repetitive instruction block was introduced.
- Validation statuses were not upgraded without recorded evidence.

## Where things live

| Path | Contents |
| --- | --- |
| `README.md` | Repository overview, prompt table, context and evaluation guidance |
| `prompts/README.md` | Prompt index, shared conventions, validation labels |
| `prompts/0N-*.md` | The prompt documents |
| `workflows/` | Prompt sequences for multi-step work |
| `examples/` | The fictional illustrative project |
| `CONTRIBUTING.md` | Contribution process and evidence bar |
| `CHANGELOG.md` | Meaningful repository changes |
