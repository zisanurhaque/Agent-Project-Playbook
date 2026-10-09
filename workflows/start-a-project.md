# Workflow: Start a Project

For going from a rough idea to a working, documented first version with an AI coding agent.

This workflow assumes there is no existing code to understand. That is the difference between it and [Improve an existing project](improve-an-existing-project.md), where discovery comes first because the codebase already contains decisions you have to respect.

## Starting conditions

- You have a goal you can describe in a sentence or two, and a rough sense of who it is for.
- You have a location for the project, empty or nearly empty.
- You have the ability to run the project's tooling, or you know you will not be able to and have said so.

You do not need a complete specification. You do need a definition of done that someone other than you could check.

## Prompt order

| Step | Prompt | Why here |
| --- | --- | --- |
| 1 | [Project planning](../prompts/02-project-planning.md) | Nothing exists to discover, so planning comes first. It produces the scope, the definition of done, and the verification plan. |
| 2 | [Feature delivery](../prompts/03-feature-delivery.md) | Implement the first bounded step from the plan, and nothing beyond it. |
| 3 | [Change review](../prompts/05-change-review.md) | Review the result before building on it. Mistakes in the foundation are the most expensive ones. |
| 4 | [Project documentation](../prompts/06-project-documentation.md) | Record what now exists, in `bootstrap` mode. This becomes the baseline that discovery would otherwise have to reconstruct. |
| 5 | [Session handoff](../prompts/07-session-handoff.md) | Produce a brief so the next session starts from facts. |

Steps 2 and 3 repeat for each bounded increment. Do not plan the whole project and then implement it in one pass; the plan's assumptions will be wrong by the time you reach the third feature.

## What carries into the next step

| From | To | What to carry |
| --- | --- | --- |
| Planning | Feature delivery | The scope statement, the step you are implementing, its acceptance criteria, and the verification command from the plan. |
| Feature delivery | Change review | The diff, the report's "Changed" list, and the author's claimed verification. |
| Change review | Feature delivery | Findings that must be addressed before the next increment. |
| Any step | Project documentation | What was built, what was verified, what failed, and the run commands. |
| Project documentation | Session handoff | `docs/PROJECT.md` and the newest `docs/WORKLOG.md` entry. |

Carry the text forward explicitly. Do not tell an agent to go and find the previous output; paste the relevant sections into the next session.

## Artifacts at the end

At the end of a completed run you should have:

- A plan with a definition of done and a verification plan.
- The planned scope implemented, or a clear statement of what was deferred.
- At least one review of the change, with findings and their resolution.
- `docs/PROJECT.md` describing what exists, with run commands marked verified or unverified.
- `docs/WORKLOG.md` with at least one entry, including what failed.
- `docs/HANDOFF.md` describing the present state.

## Judging whether the workflow is complete

- Every acceptance criterion in the plan is either confirmed by a check you ran, or explicitly listed as unverified.
- Someone unfamiliar with the project could start it using `docs/PROJECT.md` alone.
- The working tree matches what `docs/HANDOFF.md` claims about it.
- The review found no unresolved blocking issues, or the unresolved ones are written down.

If a criterion is unverified, the workflow is not complete; it is paused on a known gap. That is a legitimate state, but it should be recorded rather than glossed over.

## Lightweight path

For a small project, such as a script, a single component, or a spike:

1. Write a few sentences of plan yourself: goal, definition of done, and one verification step. You do not need the planning prompt for this.
2. Run [Feature delivery](../prompts/03-feature-delivery.md) once for the whole thing.
3. Read the diff yourself instead of running a formal review. Run the verification command.
4. Write `docs/PROJECT.md` with the run commands and a short `WORKLOG.md` entry.

Skip the handoff if you are continuing immediately and the work is uncomplicated. Do not skip the documentation step; the run commands are the part that is expensive to rediscover.

## What not to do

- Do not run discovery on an empty repository. There is nothing to find, and the report will be padded.
- Do not plan the entire project to the last detail before writing code. Plan the first increment, implement it, then re-plan.
- Do not let step 2 grow. Feature delivery works because the scope is fixed before it starts.

## Validation status

**Draft.** The sequence is described and reviewed for structure, but it has not been run end to end on a real project. The prompts it references are also Draft. See [Validation status](../README.md#validation-status).
