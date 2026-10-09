# 03 - Feature Delivery

## Purpose

Implement one bounded feature without drifting outside it. The prompt keeps the change inside existing conventions, requires verification of the resulting behaviour, and asks for an honest summary of what was and was not checked.

## When to use it

- The feature is understood well enough to describe its acceptance criteria.
- The work is bounded: one feature, not a set of related features.
- You want the implementation to follow the codebase's existing patterns rather than introduce new ones.

For work that touches several subsystems or changes architecture, use [Project planning](02-project-planning.md) first and then run this prompt once per bounded step.

## Required inputs and access

- **Repository path** (`<repository path>`): where the work happens.
- **Feature description** (`<feature description>`): what to build, in the user's terms.
- **Acceptance criteria** (`<acceptance criteria>`): the observable conditions that mean the feature is done. Write these as checks, not as intentions.
- **Scope boundary** (`<scope boundary>`): the files, modules, or behaviours the agent may change.
- **Verification command** (`<verification command>`): the project's existing test, build, or lint command, if one exists. If none exists, say so explicitly.

Write access to the codebase is required.

## Optional inputs

- **Reference implementation** (`<reference example>`): an existing feature that already does something similar, which the new code should imitate.
- **Constraints** (`<constraints>`): compatibility targets, performance limits, forbidden dependencies, or API contracts that must not change.
- **Prior plan or discovery report**: paste the relevant sections rather than linking to files the agent may not read.

## The prompt

Copy the block below into a fresh session.

```text
You are working in the repository at <repository path>.

Additional context. Delete any line you are not supplying:
- Constraints: <constraints>
- Reference example to imitate: <reference example>

Implement the following feature:

Feature: <feature description>

Acceptance criteria:
<acceptance criteria>

Scope: <scope boundary>

Verification command: <verification command>

Working rules:
- Read the repository's instruction files first and follow the most specific one that applies.
- Inspect the surrounding code before writing any. Match the existing structure, naming, error handling, and test style. Do not introduce a new pattern where an established one exists.
- Stay inside the stated scope. If the feature genuinely cannot be implemented inside it, stop and explain why rather than expanding the scope.
- Preserve unrelated code and behaviour. Do not reformat, rename, or reorganise files you were not asked to change.
- Do not run destructive commands, do not install new dependencies without asking, and do not push, publish, or change remote settings.
- Label statements as verified facts, assumptions, or proposals. Cite file paths.
- Ask a focused question only if a missing decision blocks progress. Otherwise state your assumption and continue with the rest of the work.

Work in this order:

1. Confirm your understanding.
   Restate the feature and its acceptance criteria in your own words. List the files you expect to change and the existing code you will reuse. If anything in the request is ambiguous in a way that changes the implementation, say so now.

2. Implement in small, reviewable steps.
   Prefer several coherent edits over one large rewrite. After each step, the code should still be in a state that could be built.

3. Verify.
   Run the verification command and any other check the project defines for the files you changed. If a check cannot be run, say why and treat the corresponding behaviour as unverified. Do not describe something as verified when you only reasoned about it.

4. Add or update tests if the project has a test suite and the change is testable.
   Follow the existing test style. Cover the acceptance criteria, and cover the edge cases the existing code already handles for comparable features.

5. Report.
   Use this structure exactly:

   - Changed: each file and what changed in it, one line each.
   - Behaviour: what the feature does now, and how it behaves when inputs are invalid or missing.
   - Verified: each check you ran, with the command and its result.
   - Not verified: checks you skipped or could not run, and what that leaves uncertain.
   - Assumptions: decisions you made without confirmation.
   - Deviations: anything you did outside the stated scope, with the reason. If none, say so.
   - Follow-ups: work this change makes necessary, such as migrations, documentation, or cleanup.

Do not claim the feature is complete if an acceptance criterion is unmet or unverified. State which criteria are satisfied, which are not, and why.
```

## Expected outputs

Code changes within the stated scope, plus the five-part report. The report matters as much as the code: it is what lets a reviewer decide where to look.

## How to verify the result

1. Read the diff for every file in the "Changed" list, and run `git diff --stat` to see whether any file is missing from that list.
2. Re-run the verification command yourself. Do not accept a summary of its output.
3. Walk each acceptance criterion against the running code or the tests. Confirm the ones the report claims are satisfied.
4. Check the "Deviations" section, and confirm the deviations are acceptable rather than quietly convenient.
5. Confirm the "Not verified" section is honest. If it is empty and the project has no test suite, that is a warning sign.
6. Look for unrelated formatting or renames in the diff. They make review harder and are a sign the agent roamed.

## Limitations and common failure modes

- **Overreach.** Agents often fix nearby code that looks wrong. It is usually a real defect, but it belongs in a separate change, because it is unreviewed and untested here.
- **Convention drift.** A new pattern that looks reasonable may conflict with the project's direction. Review imports, error handling, and module structure specifically.
- **Test-shaped verification.** Tests written by the same agent that wrote the code can encode the same misunderstanding. Check that the tests assert the acceptance criteria rather than the implementation.
- **Silent scope expansion.** "I also refactored X" appears in the report more often than it should. Treat every extra file as a decision someone has to review.
- **Unavailable verification.** If the project has no test suite and no build command, the change is unverified no matter how careful the agent was. Say so when you hand the work on.
- **Gaps at boundaries.** Features that work in the happy path often fail on empty input, permissions, or concurrency. Check those explicitly if they are in scope.

## Validation status

**Draft.** Written and reviewed for structure, not yet exercised on a real task. See [Validation status](../README.md#validation-status) for what would change this.
