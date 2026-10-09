# 08 - Case-Study Generation

## Purpose

Turn project records into a case study that an outside reader can trust. The hard part is not writing well; it is refusing to write things the records do not support. This prompt separates the contributor's role from the tooling's role, intended outcomes from demonstrated ones, and verified history from what remains unknown.

## When to use it

- A project needs to be described for a portfolio, a proposal, a talk, or a handover document.
- You want a written record that will still be defensible in a year.
- Someone has asked what an AI agent actually contributed, versus what you did.

The prompt needs records to work from: `WORKLOG.md`, `HANDOFF.md`, `PROJECT.md`, evidence files, commit history, or detailed notes. A case study written without records is fiction with a professional tone. If there are no records, use [Project documentation](06-project-documentation.md) first.

## Required inputs and access

- **Repository path** (`<repository path>`), if the project code is available.
- **Records** (`<records>`): the paths or pasted content the case study must be built from. Be explicit that these are the only permitted sources.
- **Audience** (`<audience>`): who will read it, for example a hiring manager, a technical peer, or a client.
- **Contribution description** (`<contribution description>`): what you did, in your own words, so the agent can attribute work correctly rather than guessing.

Read access is sufficient. Write access is needed only if the study is saved to a file.

## Optional inputs

- **Length and format** (`<length and format>`): for example, "one page of prose" or "a two-slide summary".
- **What to emphasise** (`<emphasis>`): a decision, a technique, or a problem that deserves detail.
- **What to omit** (`<omissions>`): confidential topics, unfinished work you do not want illustrated, or claims you are not ready to defend.
- **Prior drafts**: if a previous version exists, the agent can check it for unsupported claims instead of starting over.

## The prompt

Copy the block below into a fresh session.

```text
You are writing a case study for the project at <repository path>.

Additional context. Delete any line you are not supplying:
- Length and format: <length and format>
- What to emphasise: <emphasis>
- What to omit: <omissions>

Sources you may use: <records>
Audience: <audience>
The contributor's own account of their contribution: <contribution description>

Working rules:
- Read the repository's instruction files first and follow the most specific one that applies.
- Build the case study only from the sources listed above. Do not add context, results, or history from general knowledge. If a section cannot be supported by the sources, write that the evidence is missing rather than filling it.
- Never invent metrics, testimonials, feedback, analytics, screenshots, deployment outcomes, user counts, timelines, or test results. If a number is not in the sources, it does not appear.
- Separate three things throughout: what was intended, what was demonstrated, and what is unknown. Never let an intention read as a result.
- Separate the contributor's work from any tooling, agent, or template contribution. Be specific about which decisions and reviews were human.
- Do not use marketing language: no "seamless", "powerful", "cutting-edge", "10x", or similar. Describe what the system does instead.
- Do not include third-party names, customer data, or confidential details. If a source contains them, describe the shape of the thing instead.
- Do not exaggerate the maturity of the work. If it is a prototype, say prototype.

Produce a case study with these sections:

1. Title and one-line summary
   A plain description of what the project is. No superlatives.

2. Context and problem
   The situation the project addresses, who it is for, and why the existing approach was insufficient. Base this on the records; if the problem statement is not recorded, say so.

3. Constraints
   The conditions that shaped the work: time, tools, compatibility, data availability, team size. Include constraints that limited what could be demonstrated.

4. Approach
   What was done, in order, at a level a technical peer would find useful. Include the alternatives that were considered and why they were rejected, if the records contain them.

5. Key decisions and tradeoffs
   Each decision, what it cost, and what it bought. Name the alternative that was given up. Distinguish decisions that were revisited from ones that held.

6. What was built
   The concrete artifacts: components, modules, documents, or systems, with enough specificity that a reader could find them. No feature list padding.

7. Evidence of results
   What the records actually show, with a pointer to each piece of evidence: command output, test results, measurements, or observed behaviour. For each claim, name the evidence file or log entry that supports it.
   If a claim has no evidence, move it to section 8. Do not soften it into vague language to make it defensible.

8. What is intended but not demonstrated
   Benefits you expect or hope for, explicitly labelled as expectations. Also list what was never measured, and what measuring it would require.

9. Contribution and attribution
   The contributor's role, stated plainly, and what was delegated to tooling or agents, including what the contributor reviewed and what they did not.

10. Limitations and unknowns
    What the project does not handle, what is unverified, and what the reader should not conclude from this document.

11. What would be done differently
    Concrete changes, grounded in the records. If the records contain no retrospective material, say that this section is not evidence-based.

After the draft, list separately:
- Every claim in the draft that you could not trace to a source.
- Every place where the sources were thin and you had to write around the gap.
- Any sentence you are unsure belongs in a document of this kind.
Do not remove these notes; they are for the human editor.
```

## Expected outputs

A case study with the eleven sections, plus an explicit list of unsourced claims and thin spots. The list of unsourced claims is a normal and expected part of the output; a draft with none usually means the agent was not checking.

## How to verify the result

1. Take three factual claims and find them in the sources. If they are not there, the draft was embellished.
2. Check that every number in the draft appears in the records. Numbers are where fabrication is easiest and most convincing.
3. Confirm that section 8 exists and is non-empty. A case study with no undemonstrated intentions is overstating what was proven.
4. Confirm that attribution names specific human decisions rather than crediting "the team" or "the AI" generically.
5. Read for marketing adjectives and remove them.
6. Check the sources for confidential material that leaked into the prose.

## Limitations and common failure modes

- **Confident invention.** This is the failure mode the prompt exists to prevent, and it still happens. The verification step is not optional.
- **Aspirational rewriting.** Language drifts toward what the project was meant to achieve. Compare the draft against `WORKLOG.md` rather than against your memory of the project.
- **Vague hedging instead of deletion.** "Results were promising" is an unsupported claim wearing a modest coat. Either find the evidence or state that nothing was measured.
- **Attribution collapse.** An agent asked to describe a project will often describe it as its own work, or as the contributor's alone. Be explicit in the input, and check the output.
- **Thin records.** If the records are one paragraph, the case study will be short. That is the honest outcome; do not accept padding.
- **Context loss over time.** A case study written long after the work depends on records that were written quickly at the time. The earlier the records, the better this prompt works.

## Validation status

**Draft.** Written and reviewed for structure, not yet exercised on a real task. See [Validation status](../README.md#validation-status) for what would change this.
