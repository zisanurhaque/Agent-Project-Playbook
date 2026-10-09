# 05 - Change Review

## Purpose

Review a change for defects before it is accepted. The review looks at correctness, regressions, security-relevant behaviour, and verification gaps, and reports findings that someone can act on. It is read-only: the reviewer does not edit, commit, or fix.

Separating review from authoring is the point. The person who wrote a change already believes it is correct, which is an unreliable perspective for finding what it breaks.

## When to use it

- A feature or fix is complete and needs a second pass before merging.
- A diff was produced by an agent and you want a review that is not the agent reviewing itself.
- A change touches authentication, authorization, input handling, or data migration.
- You want to know how much of the change is actually verified.

Not for design-level discussions of unwritten code. This prompt reviews something that exists.

## Required inputs and access

- **Repository path** (`<repository path>`): the working tree containing the change.
- **Change under review** (`<diff reference>`): how to obtain the diff, such as a pull request number, a branch name, a commit range, or uncommitted changes. Give the exact command if it is not obvious.
- **Intent** (`<intended behaviour>`): what the change is supposed to do, and which tickets or requirements it addresses.
- **Claimed verification** (`<claimed verification>`): the checks the author says were run. If unknown, say unknown.

Read access to the repository is enough. Do not grant write access for a review session; it makes read-only discipline depend on the prompt alone.

## Optional inputs

- **Focus areas** (`<focus areas>`): correctness, security, performance, accessibility, migration safety, or a specific subsystem.
- **Threat model** (`<threat model>`): who the adversaries are and what matters, so security findings are ranked sensibly rather than generically.
- **Severity convention** (`<severity convention>`): your team's labels, if they differ from the ones in the prompt.
- **How much to include** (`<strictness>`): whether to report style preferences and minor suggestions, or only defects. By default the prompt reports defects and skips preferences.

## The prompt

Copy the block below into a fresh session.

```text
You are reviewing a change in the repository at <repository path>.

Additional context. Delete any line you are not supplying:
- Focus areas: <focus areas>
- Threat model: <threat model>
- Severity convention: <severity convention>
- How much to include: <strictness>

Change under review: <diff reference>

Intended behaviour: <intended behaviour>

Claimed verification: <claimed verification>

Working rules:
- Read the repository's instruction files first and follow the most specific one that applies.
- This is a read-only review. Do not modify, create, or delete any file, do not fix anything you find, and do not commit. Report instead. If you believe a fix is trivial, describe it; do not apply it.
- Inspect the actual diff and the code it touches. Do not review the change description.
- Label statements as verified facts, assumptions, or proposals, and cite file paths and line numbers.
- Do not report style preferences, formatting, or naming as defects unless they change behaviour or break a documented convention. Say Up to me rather than filling the review with nitpicks.
- If the change is too large or the intent too unclear to review well, say so and review the highest-risk parts instead of producing a shallow review of everything.

Cover these dimensions, in this order of importance:

1. Correctness
   Does the change do what it claims? Check boundary conditions, empty and null inputs, error paths, ordering, off-by-one errors, integer and floating-point behaviour, timezone and date handling, and concurrency where relevant.

2. Regressions
   What existing behaviour could this break? Look at callers, shared state, public interfaces, serialization formats, database migrations, and anything that depends on the old behaviour. Check whether any existing test now covers the changed behaviour, and whether any test was weakened, skipped, or deleted.

3. Security
   Check input validation and trust boundaries, authentication and authorization on new paths, injection through queries, commands, templates, or file paths, unsafe deserialization, secrets in code or logs, and new dependencies. Report only concerns that are plausible in this codebase, and say what preconditions they require.

4. Data and migration safety
   If the change alters stored data or schemas, check for backward and forward compatibility, whether it is reversible, how it behaves with existing data, and whether it can run while the system is live.

5. Verification gaps
   Which parts of the change have no test or check covering them? Compare the claimed verification against what the change actually touches. Name the behaviours that are unverified.

6. Maintainability
   Only where it affects the ability to change the code safely: hidden coupling, an interface that will be hard to evolve, duplication that will need to be kept in sync, or error handling that hides failures.

For each finding, report:
- Severity: Blocking, Important, or Minor.
  Blocking: will cause incorrect behaviour, data loss, a security hole, or a broken build.
  Important: likely to cause a defect or a maintenance problem, but not immediately fatal.
  Minor: correct but fragile, unclear, or inconsistent with the codebase.
- Location: file path and line numbers.
- What: the specific problem, quoting the relevant lines.
- Why it matters: the concrete consequence, with the input or state that triggers it.
- Suggested direction: what to change, described, not applied.

End with:
- Summary: a short assessment of the change and whether it should be accepted as is.
- Unreviewed: parts of the diff you did not review and why.
- Verification I would run: the specific commands or checks that would strengthen confidence, and which findings each would confirm or refute.
- Questions for the author: where intent is unclear, ask, rather than assuming the worst.

If you find no blocking issues, say so directly. Do not invent findings to appear thorough.
```

## Expected outputs

A list of findings with severity, location, and reasoning, followed by the summary, unreviewed areas, suggested checks, and questions. An honest "no blocking issues found" is a valid outcome.

## How to verify the result

1. Open the code at two or three reported locations. A finding that misquotes the code discredits the rest.
2. Try to reproduce one reported defect mentally or in a scratch environment. Findings should be reachable, not theoretical.
3. Check the "Unreviewed" section. A complete review with no unreviewed areas on a large diff is unlikely.
4. Confirm the reviewer did not modify anything: `git status` should be unchanged apart from what existed before the review.
5. Look for findings that are really preferences. If the review is mostly naming and formatting, ask for a defects-only pass.

## Limitations and common failure modes

- **Diff-only blindness.** The reviewer sees the change but not the callers. Ask for the surrounding code to be included when the diff is small relative to its impact.
- **Missing intent.** Reviewing without knowing the intent produces style commentary. Provide the intended behaviour.
- **False confidence.** A reviewer that cannot run the code is reasoning about it, which is weaker than running it. Treat "verified by inspection" accordingly.
- **Security theatre.** Generic checklists generate low-value warnings. Prefer fewer, specific concerns with stated preconditions.
- **Nitpick volume.** Long lists of minor points bury the blocking finding. Ask for severity ordering to be respected.
- **Review fatigue.** Very large diffs get shallower reviews. Split the change or review it in pieces.

## Validation status

**Draft.** Written and reviewed for structure, not yet exercised on a real task. See [Validation status](../README.md#validation-status) for what would change this.
