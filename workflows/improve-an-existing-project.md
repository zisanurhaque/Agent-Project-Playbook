# Workflow: Improve an Existing Project

For changing a codebase that already works, without breaking the parts nobody asked you to touch.

The defining constraint is that the code contains decisions made before you arrived, and most of them are deliberate. This workflow starts by establishing what is actually there, then changes it in bounded steps.

## Starting conditions

- The project exists and runs, or you know which parts do not.
- You have an improvement in mind: a feature, a defect, a refactor, or an answer to a question about the code.
- You have read access, and write access if the change will be implemented.

You do not need to know the codebase. That is what step 1 is for.

## Prompt order

| Step | Prompt | Why here |
| --- | --- | --- |
| 1 | [Project discovery](../prompts/01-project-discovery.md) | Establish a factual baseline before proposing anything. Skip only if you already work in this codebase. |
| 2 | [Project planning](../prompts/02-project-planning.md) | Turn the goal into scope, completion criteria, and a verification plan, grounded in what step 1 found. |
| 3a | [Debugging](../prompts/04-debugging.md) | When the goal is a defect. Reproduce and establish a cause before changing code. |
| 3b | [Feature delivery](../prompts/03-feature-delivery.md) | When the goal is a bounded change. Implement it inside the existing conventions. |
| 4 | [Change review](../prompts/05-change-review.md) | Review the result for regressions, correctness, and verification gaps. |
| 5 | [Project documentation](../prompts/06-project-documentation.md) | Update the records in `update` or `session-close` mode. |
| 6 | [Session handoff](../prompts/07-session-handoff.md) | Produce a brief for whoever continues. |

Choose 3a or 3b, not both, for a single increment. If the defect turns out to require a feature-sized change, finish the diagnosis with 3a, then re-enter at step 2 with the new information.

Steps 3 to 4 repeat for each increment. Re-planning after a discovery report is normal, because discovery usually finds something the plan did not anticipate.

## What carries into the next step

| From | To | What to carry |
| --- | --- | --- |
| Discovery | Planning | Architecture summary, conventions, run commands, and the unknowns and risks sections. The risks section is where the plan's constraints come from. |
| Planning | Debugging or feature delivery | Scope, acceptance criteria, the verification command, and the assumptions list. |
| Debugging | Change review | The diff, the root-cause explanation, and the reproduction. A reviewer who cannot reproduce the bug cannot judge the fix. |
| Feature delivery | Change review | The diff, the changed-file list, and the claimed verification. |
| Change review | Planning | Findings that change the plan's assumptions. |
| All steps | Project documentation | What changed, what was verified, what failed, and any decision the next person would otherwise revisit. |

## Artifacts at the end

- A discovery report, if step 1 was used.
- A plan with a definition of done and a verification plan.
- The change itself, with tests where the project has a test suite.
- A review with findings and their disposition.
- Updated `docs/PROJECT.md`, a new `docs/WORKLOG.md` entry, and a refreshed `docs/HANDOFF.md`.

## Judging whether the workflow is complete

- The original goal is met, or the part that is not met is stated explicitly with a reason.
- Existing checks pass, and any that fail are known failures you can name rather than new ones.
- Every acceptance criterion is verified or listed as unverified.
- The review's blocking findings are resolved or recorded as accepted risks.
- `docs/HANDOFF.md` matches the actual state of the working tree.

## Lightweight path

For a small, well-understood change in a codebase you already know:

1. Skip discovery and the planning prompt. Write the acceptance criterion and the verification command yourself, in the feature-delivery prompt.
2. Run [Feature delivery](../prompts/03-feature-delivery.md) or [Debugging](../prompts/04-debugging.md).
3. Read the diff. Run the tests. That is the review for a change you understand.
4. Append one `WORKLOG.md` entry if the change is worth remembering.

Skip this path for anything that touches shared data, authentication, or code you have not read. Those changes are where "I know this codebase" is least reliable.

## What not to do

- Do not skip discovery and then plan anyway. A plan written without reading the code is a guess with sections.
- Do not run discovery and treat its report as current after the change lands. Update `PROJECT.md` instead of leaving two versions of the truth.
- Do not let a fix turn into a refactor. Record the refactor as a separate item and review it on its own.
- Do not implement more than one increment before reviewing. Review cost grows faster than diff size.

## Validation status

**Draft.** The sequence is described and reviewed for structure, but it has not been run end to end on a real project. The prompts it references are also Draft. See [Validation status](../README.md#validation-status).
