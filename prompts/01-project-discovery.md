# 01 - Project Discovery

## Purpose

Build a factual understanding of an existing repository before anyone changes it. This prompt produces a picture of what the project does, how it is put together, and what is still unknown, so that later work is grounded in the code rather than in assumptions.

## When to use it

- Your first session in an unfamiliar repository.
- Before planning a change in a codebase you only partly know.
- When you suspect the documentation no longer matches the code.
- When a project is being handed to you and you need to establish where things stand.

Do not use it when you already know the codebase and only need to make a small change. Running discovery on a project you understand wastes time and produces a report nobody reads.

## Required inputs and access

- **Repository path** (`<repository path>`): the directory to inspect, or the working directory if the agent is already inside it.
- **Read access** to the repository, including tests, configuration, and any documentation directories.
- **Permission to run read-only commands** such as listing files, reading files, and searching. If the agent cannot run commands, say so; the report quality drops but the task is still possible.

## Optional inputs

- **Focus areas** (`<focus areas>`): subsystems or questions you care about most. Discovery on a large repository will otherwise spread thin.
- **Known suspicions** (`<known suspicions>`): things you believe are broken or undocumented, so the agent can confirm or refute them specifically.
- **A time or scope budget** (`<budget>`): for example, "the `api/` and `db/` directories only". Large repositories need this or the report becomes shallow everywhere.

## The prompt

Copy the block below into a fresh session.

```text
You are working in the repository at <repository path>.

Additional context. Delete any line you are not supplying:
- Focus areas: <focus areas>
- Known suspicions: <known suspicions>
- Scope budget: <budget>

Your task is to build a factual understanding of this project and report it.

This is a read-only task. Do not modify, create, rename, or delete any file. Do not run any command that changes state. Do not install dependencies. Read-only inspection commands are expected and encouraged.

Working rules:
- Read the repository's own instruction files first (for example AGENTS.md, CLAUDE.md, README.md, CONTRIBUTING.md, and any docs directory). Follow the most specific instruction that applies to the files you are looking at.
- Inspect the files you describe. Do not infer behaviour from file names, folder names, or the README alone.
- Label every statement as a fact you verified in a file, an assumption, or a proposal. Facts must cite a file path.
- Never invent history, versions, or intent. If you cannot determine why something is the way it is, say that it is undetermined.
- If information is missing, say so rather than filling the gap with plausible detail.

Produce a report with these sections, in this order:

1. Purpose
   What this project appears to do and who it appears to be for. Cite the files that support the conclusion, and note any documentation that disagrees with the code.

2. How to run it
   Install, build, test, lint, and start commands, taken from configuration files. Mark each as verified (you ran it, or it is defined in a configuration file you read) or unverified.

3. Entry points
   Where execution starts, with file paths and line references. If there are several entry points, explain which is primary.

4. Architecture
   The main modules or layers, what each is responsible for, how they depend on each other, and the files that define those boundaries. Note any boundary that is crossed inconsistently.

5. Data
   Where data is stored, how it moves through the system, and the schemas, models, or migrations involved. Note any place where the code and the schema disagree.

6. Conventions
   The naming, formatting, testing, and error-handling patterns the code actually follows, with one or two examples each. Distinguish conventions that are enforced by tooling from ones that are followed by habit.

7. Dependencies
   Runtime and development dependencies. Call out anything pinned, vendored, deprecated, or unused.

8. Current state
   What looks complete, what looks half-finished, and what looks unused. Base this on evidence such as TODOs, dead code, disabled tests, or features that are implemented but never called.

9. Unknowns and risks
   What you could not determine, what you suspect but did not verify, and which parts of the system look fragile and why.

10. Suggested next steps
    What to investigate or fix next, in priority order, with the reasoning behind each item.

Additionally report:
- Every instruction file you found and any instruction that constrained how you worked.
- Any place where the documentation contradicts the code.
- Any file, directory, or configuration you expected to find and did not.

Do not propose or make code changes. If you find something that looks wrong, record it in the report and leave it untouched.
```

## Expected outputs

A single report in the conversation, following the ten sections above. If you want it kept, ask for it to be written to a file such as `docs/discovery.md` and treat that as a separate, explicit request.

A good report is specific and uneven: it goes deep where the agent actually read code, and says "undetermined" where it did not. A report that is uniformly confident across every section usually means the agent summarized the README and guessed at the rest.

## How to verify the result

1. Pick three factual claims and open the files they cite. If the claim is not supported by the file, the rest of the report is unreliable.
2. Run the commands the report marks as verified. A command that fails was not verified, whatever the report says.
3. Check that section 9 contains real entries. A discovery report with no unknowns did not look hard enough.
4. Confirm the working tree is unchanged: `git status` should show nothing new from the agent.
5. Check that assumptions are labeled as assumptions. Unlabeled guesses presented as facts are the main failure mode of this prompt.

## Limitations and common failure modes

- **Size.** On a large repository, the agent will not read everything. Without a scope budget it tends to sample a few files and generalize, which produces a confident but shallow report.
- **Generated and vendored code.** `node_modules/`, build output, and generated clients inflate the report. Exclude them in the scope budget if the agent does not.
- **Documentation-only inference.** If the agent reads only the README, the report describes intentions rather than behavior. Check the citations.
- **Dynamic behavior.** Configuration loaded at runtime, feature flags, and environment-dependent paths are often missed by static inspection. Treat the "How to run it" section as a starting point, not a guarantee.
- **Silent scope drift.** A read-only prompt does not technically prevent edits. Verify with `git status` rather than trusting the report.

## Validation status

**Draft.** Written and reviewed for structure, not yet exercised on a real task. See [Validation status](../README.md#validation-status) for what would change this.
