# Workflow: Document and Present a Project

For turning a project's records into documentation someone else can use, and a case study that survives scrutiny.

This workflow runs at the end of a period of work, or before a project is shown to anyone. It is short by design: three prompts, in one order, for one reason. Each step depends on the record the previous step produced.

## Starting conditions

- The project exists and you can describe what it does.
- Some record of the work exists: notes, commit history, session transcripts, or a previous `WORKLOG.md`.
- You intend to present the work to someone: a reader, a reviewer, a client, or your future self.

If there is no record at all, this workflow will produce thin output. Consider running [Improve an existing project](improve-an-existing-project.md) first to do the work in a way that generates records.

## Prompt order

| Step | Prompt | Why here |
| --- | --- | --- |
| 1 | [Project documentation](../prompts/06-project-documentation.md) | Creates or updates `PROJECT.md`, `WORKLOG.md`, and `HANDOFF.md`, and captures raw evidence. Everything downstream depends on this. |
| 2 | [Session handoff](../prompts/07-session-handoff.md) | Condenses the current state into a short brief. Only needed if someone else is picking up the work; the case study does not depend on it. |
| 3 | [Case-study generation](../prompts/08-case-study-generation.md) | Writes the outside-facing narrative, drawing only on the records produced in steps 1 and 2. |

The order matters. A case study written before the records exist will be assembled from memory, and memory exaggerates.

## What carries into the next step

| From | To | What to carry |
| --- | --- | --- |
| Project documentation | Session handoff | `docs/PROJECT.md` and the newest `docs/WORKLOG.md` entry. |
| Project documentation | Case study | The decision records, the evidence files, and the "what failed" entries. Failures are what make a case study credible. |
| Session handoff | Case study | The decisions in force and the remaining verification, so the case study's limitations section is accurate. |

Pass the file paths explicitly, and state that those records are the only permitted sources for the case study.

## Artifacts at the end

- `docs/PROJECT.md` describing what exists, with verified and unverified commands marked.
- `docs/WORKLOG.md` with dated entries, including what did not work.
- `docs/HANDOFF.md` if the work is continuing.
- `docs/CASE-STUDY.md` with sections for demonstrated results, undemonstrated intentions, and unknowns.
- `docs/evidence/` containing the raw artifacts that back the claims.

## Judging whether the workflow is complete

- Every claim in the case study traces to a record or an evidence file, or is listed as unsourced.
- The case study distinguishes intended benefit from demonstrated result in a way a sceptical reader would accept.
- Attribution names specific human decisions and specific delegated work, rather than crediting a vague team or a tool.
- Nothing confidential or sensitive appears in any of the records.
- The records describe the project as it is now, not as it was before the last change.

## Lightweight path

For a personal project being described in a paragraph or two:

1. Write `docs/PROJECT.md` and one `WORKLOG.md` entry yourself, including the run commands and what did not work.
2. Skip the handoff.
3. Use [Case-study generation](../prompts/08-case-study-generation.md) with those two files as the only sources, and keep the length short.

The verification step for the case study is the one part that should never be skipped. Take three claims and find them in the records.

## What not to do

- Do not write the case study first and the records afterwards. That is how a case study becomes a story about a project that did not quite happen.
- Do not treat the case study as marketing. Its value is that the claims hold up; a reader who checks one will check more.
- Do not omit failures and unknowns. A case study with no limitations reads as an advertisement and is trusted less, not more.
- Do not let `PROJECT.md` grow into a narrative. History belongs in `WORKLOG.md`; `PROJECT.md` describes the present.

## Validation status

**Draft.** The sequence is described and reviewed for structure, but it has not been run end to end on a real project. The prompts it references are also Draft. See [Validation status](../README.md#validation-status).
