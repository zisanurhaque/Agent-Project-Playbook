# 04 - Debugging

## Purpose

Establish what is actually wrong before changing anything. The prompt separates the observed symptom from the explanation for it, requires a reproduction or a clear statement that reproduction failed, and asks for a focused fix with evidence that it works.

## When to use it

- Something is broken and the cause is not established.
- A test fails intermittently and the pattern is unclear.
- A change produced a regression and it is not obvious which part caused it.
- A bug report is vague and needs to become a reproducible case.

Not for changes where the cause is already known and the work is simply to implement a fix. Use [Feature delivery](03-feature-delivery.md) in that case.

## Required inputs and access

- **Repository path** (`<repository path>`).
- **Symptom** (`<observed symptom>`): what happens, where it happens, and what you expect instead. If you have an error message, include it verbatim.
- **Reproduction steps** (`<reproduction steps>`): how you trigger it, if you know. "Unknown" is an acceptable and useful input.
- **Verification command** (`<verification command>`): the command that should pass once the defect is fixed, such as a failing test or a build.

Write access is required for the fix. If you want diagnosis only, say so and the prompt stops after the root-cause section.

## Optional inputs

- **When it started** (`<when it started>`): a commit, version, or date, if known. This narrows the search enormously.
- **Environment** (`<environment>`): operating system, runtime version, configuration, or data shape where the failure appears.
- **Already ruled out** (`<ruled out>`): hypotheses you have eliminated, so the agent does not repeat them.
- **Blast radius** (`<allowed scope>`): which files the agent may change. Useful when the codebase is shared or sensitive.

## The prompt

Copy the block below into a fresh session.

```text
You are working in the repository at <repository path>.

Additional context. Delete any line you are not supplying:
- When it started: <when it started>
- Environment: <environment>
- Already ruled out: <ruled out>
- Allowed scope for changes: <allowed scope>

Observed symptom: <observed symptom>

Reproduction steps: <reproduction steps>

Verification command: <verification command>

Working rules:
- Read the repository's instruction files first and follow the most specific one that applies.
- Inspect the relevant code yourself. Do not rely on this description for facts about the code, and cite file paths for every claim.
- Separate observation from explanation at all times. A log line, a failing assertion, and a stack trace are observations. "This must be a race condition" is a hypothesis, and must be labelled as one.
- Do not guess-and-patch. Do not change code until you can explain why the current code produces the observed behaviour.
- Preserve unrelated code and behaviour. Do not reformat or reorganise files while fixing this.
- Do not run destructive commands, and do not push or publish anything.
- Ask a focused question only if missing information blocks progress. Otherwise state the assumption and continue.

Work in this order:

1. Reproduce or characterise.
   Attempt to reproduce the symptom using the stated steps and the verification command. Record the exact command and the exact output.
   If you cannot reproduce it, say so explicitly, and then characterise it as far as the evidence allows: when it happens, what it depends on, and what you ruled out. Do not proceed to a fix on the basis of an unreproduced bug without stating clearly that the cause is unconfirmed.

2. Describe the symptom precisely.
   State what happens, where it diverges from expected behaviour, and the smallest input that triggers it. Trace the execution path from the entry point to the point of failure, with file and line references.

3. Form hypotheses and test them.
   List the candidate causes. For each, state what evidence would confirm or refute it. Test the cheapest hypotheses first. Report what each test showed, including the ones that were wrong.

4. Identify the root cause.
   Explain why the code produces the observed behaviour, referencing specific lines. Distinguish the root cause from contributing conditions. If you cannot establish a cause, stop here and report what you know; do not apply a speculative fix.

5. Fix the cause, not the symptom.
   Make the smallest change that addresses the root cause and fits the existing code style. Do not add defensive code for problems the fix already removes. If a broader refactor is the right answer, describe it and ask before doing it.

6. Verify.
   Re-run the verification command and any other check that covers the changed code. Confirm the original symptom is gone. Run the wider test suite if one exists, and report any test that newly fails. State clearly whether the fix was verified and how.

7. Prevent recurrence if the project has a test suite.
   Add a regression test that fails without the fix and passes with it. If you cannot write one, say why.

8. Report.
   Use this structure:
   - Symptom: what was observed, with the exact evidence.
   - Reproduction: the command and output, or an explicit statement that it could not be reproduced.
   - Root cause: the explanation, with file and line references.
   - Hypotheses considered: what you tested and what it showed.
   - Fix: what changed and why this addresses the cause.
   - Verified: checks run and their results.
   - Not verified: what remains uncertain, including any part of the code path you did not exercise.
   - Related issues: similar defects you noticed but deliberately did not fix.
   - Follow-ups: anything the fix makes necessary.
```

## Expected outputs

A diagnosis, a focused fix, and the eight-part report. If the defect cannot be reproduced or the cause cannot be established, the correct output is a characterisation report with no code change. That is a useful result, not a failure.

## How to verify the result

1. Reproduce the original symptom yourself before looking at the fix, so you know the baseline is real.
2. Check that the root cause explains the symptom without extra assumptions. A fix that works for unexplained reasons will break again.
3. Run the verification command and the wider suite. Watch for tests that now fail or were disabled.
4. Inspect the diff. A debugging fix should be small. If the diff is large, ask which part is the fix and which part is tidying.
5. If a regression test was added, confirm it fails when the fix is reverted. Without that check, the test may pass for the wrong reason.
6. Verify the reported observations against the actual output. Quoted evidence should match what the command really printed.

## Limitations and common failure modes

- **Guess-and-patch.** The most common failure: the agent changes something plausible, the symptom disappears, and nobody learns what was wrong. Requiring the root-cause step before the fix is what prevents this.
- **Unreproducible defects.** Intermittent, environment-specific, or data-specific bugs may not reproduce in a session. The prompt then produces characterisation, which is less satisfying but honest.
- **Symptom suppression.** Adding a guard, a retry, or a more permissive check can hide the defect rather than fix it. Check that the fix addresses the cause and not the report.
- **Widening the net.** Debugging often turns up adjacent problems. The prompt asks for them to be reported, not fixed.
- **Flaky verification.** If the verification command itself is unreliable, a passing run proves little. Establish whether the check is deterministic before trusting it.
- **Environment drift.** A fix verified in the agent's environment may not hold where the defect was reported. Note the environment in the report.

## Validation status

**Draft.** Written and reviewed for structure, not yet exercised on a real task. See [Validation status](../README.md#validation-status) for what would change this.
