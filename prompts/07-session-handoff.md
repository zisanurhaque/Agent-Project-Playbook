# 07 - Session Handoff

## Purpose

Produce a short continuation brief that lets the next session start from facts. The brief records where the work stands, which decisions are settled, which files matter, what is blocked, and what is claimed but unverified.

A handoff reduces the cost of restarting. It does not remove the need to look at the code. Anyone acting on a handoff should still inspect the files it names, because a brief is a claim about the state of the tree and claims go stale.

## When to use it

- You are ending a session with work unfinished.
- Another person, or another agent session, will continue the work.
- You are returning to a project after a break and want your own summary to be trustworthy.

This prompt produces one file. Use [Project documentation](06-project-documentation.md) in `session-close` mode when the whole record set - `PROJECT.md`, `WORKLOG.md`, `HANDOFF.md`, `CASE-STUDY.md`, and evidence - needs attention. Do not run both for the same session; the handoff is a subset of what that mode produces.

## Required inputs and access

- **Repository path** (`<repository path>`).
- **Session record** (`<session record>`): what the agent should base the brief on. This can be the session transcript, the current diff, or a description of what happened. Without it, the brief is written from memory and will contain errors.
- **Destination** (`<destination>`): where to write the brief, usually `docs/HANDOFF.md`. If you only want it in the conversation, say so.

Read access is enough if the brief is written to the conversation. Write access is needed to save it to a file.

## Optional inputs

- **Next objective** (`<next objective>`): what the next session is expected to do, so the brief can be aimed at it and list only the relevant files.
- **Blocked items** (`<blockers>`): anything you already know is stuck, so the agent can record it accurately instead of guessing.
- **Audience** (`<audience>`): whether the next reader is the same person, a different person, or a fresh agent with no context.

## The prompt

Copy the block below into a fresh session.

```text
You are working in the repository at <repository path>.

Additional context. Delete any line you are not supplying:
- Who will read this: <audience>
- Known blockers: <blockers>

Session record: <session record>
Next objective: <next objective>

Your task is to write a continuation brief for whoever works on this project next. Write it to <destination>, replacing any existing contents, or output it in the conversation if no destination was given.

Working rules:
- Read the repository's instruction files first and follow the most specific one that applies.
- Establish the state of the working tree by inspecting it, not by trusting the session record. Run read-only commands such as status, diff, and recent log, and report what they actually show.
- Base every statement on the session record, a file you read, or a command you ran. Mark anything else as unverified.
- Do not invent progress, decisions, or results. If the session record is unclear about something, write that it is unclear.
- Do not modify application code. The only file you may write is the brief itself.
- Keep the brief short. Target one screen. If it is longer, cut detail rather than dropping sections.

Write the brief with these sections:

1. Status
   Where the work stands, in a few sentences. What is done, what is partially done (and how far), and what has not been started.

2. State of the working tree
   Based on what you observed: which files are modified, untracked, or staged; whether anything was committed this session; whether the tree builds or tests pass as far as you could check. If you could not check, say so.

3. Decisions in force
   Decisions that a newcomer would otherwise revisit or reverse. One line each, with the reason and, where relevant, the alternative that was rejected. Mark decisions made without confirmation as provisional.

4. Files to read first
   The small set of paths that give the most context, in reading order, with a note on why each matters. Do not list the whole repository.

5. Blockers and open questions
   What is stuck, why, and what would unblock it. Questions that only the requester can answer.

6. Next steps
   An ordered list of concrete actions. Each step should name the files it touches and how to tell it worked.

7. Remaining verification
   What is claimed but not checked, the exact command that would check it, and what a failure would mean. Include checks that could not be run and why.

8. Caveats
   Where this brief is least reliable: what you inferred rather than observed, and what the next reader should confirm before relying on it.

Close with one line stating that this brief is a summary, that the code is the source of truth, and that the reader should inspect the named files before acting.
```

## Expected outputs

A brief with the eight sections, written to the destination and kept short. The "State of the working tree" section is the one most often wrong, so it should be based on commands actually run.

## How to verify the result

1. Compare "State of the working tree" with `git status` and `git diff`. This is the section that misleads people most often.
2. Check that "Next steps" are actionable: each should name files and a way to tell it worked.
3. Confirm that "Remaining verification" names unverified work. A brief claiming everything is verified after an incomplete session is a warning.
4. Verify that the brief names only a handful of files in "Files to read first". A long list defeats the purpose.
5. If the next reader is a fresh agent, confirm the brief contains enough context to work without the original transcript.

## Limitations and common failure modes

- **Staleness.** A brief is accurate at the moment it is written. If work continues afterwards, it silently becomes wrong.
- **Optimistic status.** Sessions get summarised as further along than they are, especially when the previous session ended hopefully. Cross-check the tree state.
- **Missing failures.** What did not work is the most useful information and the most commonly omitted. Ask for it explicitly if it is absent.
- **Inference presented as observation.** "Tests should still pass" is not a test result. Look for language that hedges, and treat those items as unverified.
- **Length.** Briefs grow as sections are added. If it does not fit on a screen, the next reader will skim it and miss the blockers.
- **Substitution for inspection.** Never act on a brief alone for anything destructive, such as reverting or deleting work. Inspect first.

## Validation status

**Draft.** Written and reviewed for structure, not yet exercised on a real task. See [Validation status](../README.md#validation-status) for what would change this.
