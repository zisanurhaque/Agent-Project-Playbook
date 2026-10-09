# 02 - Project Planning

## Purpose

Turn a goal into a scoped plan that someone can execute and check. The plan states what will change, what will not, how completion will be judged, and how the result will be verified.

This prompt produces a plan, not implementation. Planning and building in the same step is how scope grows without anyone noticing.

## When to use it

- A goal is clear enough to describe but not yet broken into work.
- A change touches several files or subsystems and needs an order.
- You want completion criteria agreed before an agent starts writing code.
- You need to know what a task depends on before committing to it.

Skip it for small, well-understood changes. If the work is "rename this field and update its callers", a plan is overhead.

## Required inputs and access

- **Goal** (`<goal>`): what should be true when the work is done, in one or two sentences.
- **Repository path** (`<repository path>`): the working tree the plan describes. The agent needs to see the code; a plan written without reading it is fiction.
- **Verification expectation** (`<verification expectation>`): how you intend to check the result, such as an existing test suite, a manual check, or a build command. If you do not know yet, say so; the plan should propose one.

## Optional inputs

- **Constraints** (`<constraints>`): deadlines, compatibility requirements, forbidden dependencies, performance budgets, or conventions that are not written down.
- **Out of scope** (`<out of scope>`): what you already know you do not want touched. This is the most valuable optional input, because unstated scope is where plans go wrong.
- **Prior context** (`<prior context>`): output from [Project discovery](01-project-discovery.md) or a previous session. Paste the relevant sections rather than telling the agent to go find them.

## The prompt

Copy the block below into a fresh session.

```text
You are working in the repository at <repository path>.

Additional context. Delete any line you are not supplying:
- Constraints: <constraints>
- Out of scope: <out of scope>
- Prior context: <prior context>

Goal: <goal>

Verification expectation: <verification expectation>

Your task is to produce a plan for achieving this goal. Do not implement anything. Do not modify files. The deliverable is a plan document.

Working rules:
- Read the repository's instruction files first and follow the most specific one that applies.
- Inspect the code the plan will affect. Do not plan against an imagined codebase. Cite file paths for every claim about existing behaviour.
- Label statements as verified facts, assumptions, or proposals. Assumptions must be listed explicitly, because they are the main source of planning error.
- Ask a focused question only if a missing decision blocks the plan. Otherwise state the assumption you are making and continue.
- Do not propose a plan that requires destructive operations or publishing to an external system without flagging that it does.

Scale the plan to the work. A one-file change gets a short checklist and a verification step. A multi-subsystem change gets phases with checkpoints. Do not pad a small task into a large plan.

Produce a plan with these sections:

1. Goal and definition of done
   Restate the goal in your own words, then list completion criteria. Each criterion must be observable: someone other than the author can check it and get a yes or no. If a criterion cannot be made observable, say so and explain why.

2. Current behaviour
   What happens today, with file references. Include anything that looks broken or accidental, since the plan may need to preserve it.

3. User-visible workflows
   The workflows that change, step by step, from the user's point of view. If the change is not user-visible, say so and describe the developer-facing equivalent.

4. Scope
   In scope: the specific files, modules, or behaviours you expect to change.
   Out of scope: things a reader might reasonably expect to be included but which are not.
   Call out consciously deferred items rather than silently omitting them.

5. Design
   The approach you propose, and why it fits the existing architecture. Name the patterns already in use that the change should follow. If a simpler alternative exists, describe it and say why you did not choose it.

6. Steps
   An ordered list of steps. Each step should be independently reviewable where possible, and should state what it changes and how you will know it worked. Group steps into phases if the work is large, with a checkpoint between phases.

7. Dependencies and sequencing
   What must happen before what, and which steps can be done in parallel. Include external dependencies such as a decision, an approval, or a third-party change.

8. Risks
   What could go wrong, how likely it is, and what would reveal it. Include risks that the change will not fit the existing architecture, and risks of regressions in existing behaviour.

9. Verification plan
   How each completion criterion will be checked. Use the project's existing checks where they exist; name the exact command. Where no automated check exists, describe the manual check. State which criteria will remain unverified, and why.

10. Open questions
    Decisions that only the requester can make, each phrased as a specific question with the options you see. If there are none, say so plainly.

Also state explicitly:
- The assumptions you made, and what would change if each is wrong.
- Anything you would need to know before starting implementation.
```

## Expected outputs

A plan document covering the ten sections. The most important parts are the definition of done, the scope boundaries, and the verification plan. Those three sections are what make the plan checkable.

## How to verify the result

1. Test the definition of done: could a different person read each criterion and decide yes or no? Vague criteria like "the feature works well" fail this test.
2. Check the file references in "Current behaviour". Open two of them. If the code does not say what the plan claims, the plan's design is built on sand.
3. Confirm that out-of-scope items are real decisions rather than an empty section.
4. Check that every step has a way to tell it worked. A step nobody can verify will be declared complete by default.
5. Read the assumptions list. If it is empty on a nontrivial task, the agent did not surface them; ask again.
6. Look for a step that does two unrelated things. Those are the steps where review gets skipped.

## Limitations and common failure modes

- **Plans that grow during execution.** A plan is a snapshot. Once implementation starts, re-check scope rather than assuming the plan still applies.
- **Unknown unknowns.** Planning cannot reveal what nobody knows. The risks section is a partial substitute, not a solution.
- **Over-planning.** Agents asked to plan will often produce a thorough-sounding document for a task that needed half a page. If the plan is longer than the change, shorten it.
- **Confident file references.** Agents sometimes cite plausible paths that do not exist. Verify them.
- **Deferred decisions.** An open question left in the plan will become an unrecorded decision during implementation. Answer the questions before starting, or confirm the chosen assumption in writing.
- **Verification by assertion.** "Tests will be added" is not a verification plan until it names what the tests assert.

## Validation status

**Draft.** Written and reviewed for structure, not yet exercised on a real task. See [Validation status](../README.md#validation-status) for what would change this.
