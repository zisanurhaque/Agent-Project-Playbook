# 06 - Project Documentation

## Purpose

Set up and maintain a lightweight documentation system so that a project's context, decisions, progress, and evidence survive across sessions and can support an honest case study later.

Documentation is treated as part of finishing meaningful work, not as a separate chore. It should be concise, factual, and useful, and it should be cheap enough that people actually keep it current.

This prompt maintains:

| Path | Role | Update style |
| --- | --- | --- |
| `AGENTS.md` | Instructions telling future agents how to use the records | Updated when the convention changes |
| `docs/PROJECT.md` | What the project is and how it works now | Rewritten in place |
| `docs/WORKLOG.md` | Chronological record of meaningful work | Append-only |
| `docs/HANDOFF.md` | Short, current brief for whoever continues | Replaced, not appended |
| `docs/CASE-STUDY.md` | Living, evidence-based draft for outside readers | Updated when there is a new decision, challenge, or result |
| `docs/evidence/` | Verification records and visual evidence, with a README | Added to, never edited |

## When to use it

Use it in one of three modes:

- `bootstrap` - the project has no records, or has some but not a complete set.
- `update` - records exist and no longer match the code.
- `session-close` - meaningful work just finished and the records need to catch up.

It is also the first step of [Document and present a project](../workflows/document-and-present-a-project.md).

Do not run it after every edit. For a trivial change, update only the documents the change actually affects. Documentation churn is its own kind of inaccuracy, because a record nobody trusts is worse than no record.

## Required inputs and access

- **Repository path** (`<repository path>`).
- **Mode** (`<mode>`): `bootstrap`, `update`, or `session-close`.
- **Records** (`<records>`): what the entries should be based on. This can be a session transcript, the current diff, commit history, test output, or notes you wrote. Without it, entries are reconstructed from memory, which is the failure this system exists to prevent.
- **Access**: read access to the repository and its Git history. Write access to `AGENTS.md` and `docs/` if the files are to be created or updated.

## Optional inputs

- **Project context** (`<project context>`): purpose, intended users, and constraints the agent cannot derive from the code.
- **Your explanations** (`<explanations>`): why you made particular decisions. The agent cannot recover motivations from code, and must not invent them.
- **Sensitivity rules** (`<sensitivity rules>`): what must never be written down, such as credentials, personal data, internal hostnames, or customer names.
- **Evidence to capture** (`<evidence to capture>`): screenshots, recordings, or command output you want saved, and how they should be named.

## The prompt

Copy the block below into a fresh session.

```text
You are working in the repository at <repository path>.

Additional context. Delete any line you are not supplying:
- Project context: <project context>
- Sensitivity rules: <sensitivity rules>
- Evidence to capture: <evidence to capture>

Mode: <mode>
Records to base this on: <records>
Information only I can supply: <explanations>

Your task is to set up or maintain this project's documentation system, so that context, decisions, progress, and evidence survive across sessions and can support an honest case study later.

Treat documentation as part of completing meaningful work. Keep it concise, factual, and useful.

Working rules:
- Read the repository's existing instructions, documentation, project structure, and Git history before creating anything.
- Preserve existing conventions and useful content. Update existing documents rather than creating duplicate sources of truth. Do not overwrite or contradict existing AGENTS.md instructions; integrate with them.
- Never invent challenges, alternatives, tests, feedback, metrics, or outcomes.
- Report failed, skipped, and unavailable checks clearly.
- Ask one focused question if my rationale is missing and it matters. Continue independent work where possible.
- Keep everything in plain Markdown, version controlled alongside the project.
- Do not create commits for the sake of this documentation work, and do not fabricate commit references.
- Do not store credentials, sensitive personal information, unnecessary raw logs, or large generated build artifacts.

## 1. Inspect before creating

For an existing project, distinguish these four categories and keep them separate throughout the documents:

- Current behaviour verified through inspection or execution.
- Historical changes supported by Git history or other records.
- My explanations of goals and decisions.
- Unknown information that cannot be recovered.

Do not reconstruct motivations or development history as facts without evidence. If my rationale is missing and it matters, ask one focused question.

## 2. Establish these files

### AGENTS.md

Add or extend concise instructions requiring agents to:

- Read `docs/PROJECT.md` and `docs/HANDOFF.md` before starting work.
- Consult relevant `WORKLOG.md` entries when a task depends on earlier decisions.
- Inspect the actual code rather than relying on documentation alone.
- Update the appropriate documents after meaningful work.
- Preserve historical worklog entries, and mark superseded decisions clearly rather than deleting them.
- Keep claims grounded in evidence.
- Follow the repository's existing development and verification requirements.

Use relative links to the documents. If AGENTS.md already contains instructions, preserve them and add to them.

### docs/PROJECT.md

A concise description of the project as it is now:

- Product purpose and the problem being addressed.
- Intended users and important workflows.
- Goals, scope, and explicit exclusions.
- Constraints and user-confirmed preferences.
- Current implemented features.
- Planned features, clearly separated from implemented features.
- High-level architecture and the important code locations.
- Setup, run, and verification commands, verified against the repository.
- Known limitations and unresolved questions.

Label assumptions and proposals as such. Do not treat an agent suggestion as my approved decision.

### docs/WORKLOG.md

Chronological entries for meaningful features, fixes, experiments, and decisions.

Each entry contains, where relevant:

- Date and a short title.
- Problem or requested outcome.
- Decision and its rationale.
- Who proposed or confirmed the decision.
- Alternatives actually considered.
- Implementation summary and relevant code paths.
- Problems encountered and approaches actually tried.
- Verification performed and its result.
- Evidence links and existing commit references.
- Remaining limitations or follow-up work.

Keep entries brief. Omit fields that have nothing useful to record. Do not log every small edit, and do not paste full diffs. Append new entries; do not rewrite old ones. If an earlier entry is wrong, add a new one that corrects it.

### docs/HANDOFF.md

Short and current:

- Last updated date.
- Current milestone and project status.
- Most recently completed work.
- Work in progress.
- Blockers and unresolved issues.
- Relevant files for the next task.
- Next recommended actions.
- Verification still required.

Replace outdated status information rather than accumulating it. Development history belongs in `WORKLOG.md`.

### docs/CASE-STUDY.md

A living, evidence-based draft containing:

- Project summary and status.
- Problem and intended users.
- My role and the AI agent's role.
- Constraints.
- Important decisions and tradeoffs.
- Significant challenges and how they were addressed.
- Implemented solution.
- Verification and evidence.
- Demonstrated outcomes.
- Limitations and lessons.
- Future work.

Mark it initially as "Draft - evidence still being collected."

Distinguish intended benefits from demonstrated results. Do not claim users, revenue, improved engagement, time savings, performance improvements, or production readiness without supporting evidence.

Describe my contribution only where it is supported by my instructions or confirmations. Distinguish checks the agent ran from checks I personally performed.

### docs/evidence/

Verification records, screenshots, and demo recordings that support claims elsewhere.

Add a short README in this directory explaining:

- Naming conventions.
- How evidence links relate to worklog entries.
- What each saved item demonstrates.
- Whether it shows the current version or an earlier state.

Capture evidence when tools and access permit. If capture is unavailable, record exactly what remains to be captured. Never create placeholder evidence that appears real.

## 3. Follow this workflow during development

Before a meaningful task:

1. Read the project context and handoff.
2. Inspect the relevant implementation.
3. Identify the requested outcome and how completion will be checked.
4. Record consequential decisions when they are made.

During the task:

1. Record significant obstacles, failed approaches, and changed decisions while their context is still available.
2. Separate observed facts from assumptions.
3. Continue authorized work without requesting approval solely in order to maintain documentation.

After the task:

1. Run appropriate verification and report the actual results.
2. Update `PROJECT.md` if behaviour, architecture, scope, or commands changed.
3. Append a concise `WORKLOG.md` entry.
4. Refresh `HANDOFF.md`.
5. Save or link relevant evidence when available.
6. Update `CASE-STUDY.md` only when the task added a meaningful decision, challenge, or result.

For trivial edits, update only the documents affected. Avoid documentation churn.

## 4. Keep the record trustworthy

- Never invent challenges, alternatives, tests, feedback, metrics, or outcomes.
- A successful build does not prove that every workflow works.
- A screenshot demonstrates appearance, not reliability or usability.
- Report failed, skipped, and unavailable checks clearly.
- Link claims to relevant files, worklog entries, evidence, or existing commits.
- Do not fabricate commit hashes, and do not create commits solely for this documentation setup.
- Exclude private information from the case study, since it is the document most likely to be published.

## 5. Prepare the case study when requested

When I request a case study, review the documents and the relevant implementation, then produce a concise, portfolio-ready narrative in this order:

Problem, then my role and AI involvement, then constraints, key decisions, challenges, solution, verification, outcomes, and finally limitations and lessons.

Lead with the problem and the resulting behaviour. Select the strongest supported examples instead of listing every feature.

If results have not been measured, describe demonstrated functionality and identify the missing evidence. Do not turn aspirations into achievements.

## 6. Report when finished

Report briefly:

- Files created or updated.
- Your current understanding of the project.
- Important missing information.
- The next development step.

If no code or project idea exists yet, establish a minimal skeleton and mark the unknown sections clearly. Do not invent an application.
```

## Expected outputs

- `AGENTS.md` reflecting the documentation workflow, or an addition to the existing one.
- `docs/PROJECT.md`, `docs/WORKLOG.md`, `docs/HANDOFF.md`, and `docs/CASE-STUDY.md`.
- `docs/evidence/README.md`, plus any evidence that could actually be captured.
- The short closing report, including the files changed and what is still missing.

## How to verify the result

1. Open `docs/PROJECT.md` and run one of the commands it lists. Commands that were not verified should be marked as unverified; if they are marked verified and fail, the file is not trustworthy.
2. Check `docs/HANDOFF.md` against the working tree. A handoff that misreports the tree state will mislead the next person.
3. Read the newest `WORKLOG.md` entry and look for what failed or was tried and abandoned. If everything succeeded, the record is probably incomplete.
4. Confirm that `docs/CASE-STUDY.md` contains no metrics, user counts, or outcomes that are absent from the evidence directory.
5. Confirm that existing `AGENTS.md` instructions were preserved rather than replaced. Use `git diff` to check.
6. Search the new files for credentials, personal data, and private hostnames before committing.
7. Confirm that older worklog entries were not rewritten.

## Limitations and common failure modes

- **Reconstruction from memory.** Without the session records, the agent writes what sounds plausible. Always supply the transcript, diff, or command output.
- **Sycophantic logs.** Agents tend to record successes and omit abandoned approaches, which removes the most useful part of the log.
- **Duplicate sources of truth.** A second file describing the same thing as `PROJECT.md` will drift from it. Extend the existing file instead.
- **Unverified commands.** Setup commands copied from a README that was already wrong. Run them, and mark the ones you did not.
- **Evidence that is not evidence.** A written summary of a test run is not a test run. Keep the raw output.
- **Growth without pruning.** `WORKLOG.md` accumulates by design; `PROJECT.md` and `HANDOFF.md` must be rewritten or they become unusable.
- **Case study drift.** This is the document most likely to be read by strangers and the most tempting to improve. Every claim in it must be traceable.

## Validation status

**Draft.** Written and reviewed for structure, not yet exercised on a real task.

This prompt is adapted from the repository author's own project-documentation prompt, which was provided as the source for this file. Its safeguards are preserved: inspecting before creating, keeping the four categories of information separate, never inventing challenges or outcomes, distinguishing agent-run checks from checks the author performed, and refusing to let intentions read as results. The organization around the shared prompt format, the mode selection, and the verification section is new.

See [Validation status](../README.md#validation-status) for what would change this label.
