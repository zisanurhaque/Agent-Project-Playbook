Set up and maintain a lightweight documentation system for this project so that its context, decisions, development progress, and evidence survive across AI sessions and can support an honest portfolio case study.

Treat documentation as part of completing meaningful work. Keep it concise, factual, and useful.

## 1. Inspect before creating

Read existing repository instructions, documentation, project structure, and available Git history.

Preserve existing conventions and useful content. Update existing documents where appropriate instead of creating duplicate sources of truth. Do not overwrite or contradict existing AGENTS.md instructions.

For an existing project, distinguish:

- Current behavior verified through inspection or execution.
- Historical changes supported by Git history or other records.
- My explanations of goals and decisions.
- Unknown information that cannot be recovered.

Do not reconstruct motivations or development history as facts without evidence.

## 2. Establish these files

### AGENTS.md

Add concise instructions requiring agents to:

- Read docs/PROJECT.md and docs/HANDOFF.md before starting work.
- Consult relevant WORKLOG entries when a task depends on earlier decisions.
- Inspect the actual code rather than relying solely on documentation.
- Update the appropriate documents after meaningful work.
- Preserve historical worklog entries and mark superseded decisions clearly.
- Keep claims grounded in evidence.
- Follow the repository’s existing development and verification requirements.

Use relative links to the documents.

### docs/PROJECT.md

Maintain a concise description of the current project:

- Product purpose and problem being addressed.
- Intended users and important workflows.
- Goals, scope, and explicit exclusions.
- Constraints and user-confirmed preferences.
- Current implemented features.
- Planned features, clearly separated from implemented features.
- High-level architecture and important code locations.
- Setup, run, and verification commands verified against the repository.
- Known limitations and unresolved questions.

Label assumptions and proposals. Do not treat an agent suggestion as my approved decision.

### docs/WORKLOG.md

Maintain chronological entries for meaningful features, fixes, experiments, and decisions.

Each entry should include, where relevant:

- Date and short title.
- Problem or requested outcome.
- Decision and its rationale.
- Who proposed or confirmed the decision.
- Alternatives actually considered.
- Implementation summary and relevant code paths.
- Problems encountered and approaches actually tried.
- Verification performed and its result.
- Evidence links and existing commit references.
- Remaining limitations or follow-up work.

Keep entries brief. Omit fields that have nothing useful to record. Do not log every small edit or repeat full code diffs.

### docs/HANDOFF.md

Keep this short and current:

- Last updated date.
- Current milestone and project status.
- Most recent completed work.
- Work in progress.
- Blockers and unresolved issues.
- Relevant files for the next task.
- Next recommended actions.
- Verification still required.

Replace outdated status information. Preserve development history in WORKLOG.md.

### docs/CASE-STUDY.md

Create a living, evidence-based draft with:

- Project summary and status.
- Problem and intended users.
- My role and the AI agent’s role.
- Constraints.
- Important decisions and tradeoffs.
- Significant challenges and how they were addressed.
- Implemented solution.
- Verification and evidence.
- Demonstrated outcomes.
- Limitations and lessons.
- Future work.

Initially mark it as “Draft — evidence still being collected.”

Distinguish intended benefits from demonstrated results. Do not claim users, revenue, improved engagement, time savings, performance improvements, or production readiness without supporting evidence.

Describe my contribution only where it is supported by my instructions or confirmations. Distinguish agent-run verification from checks I personally performed.

### docs/evidence/

Use this directory for relevant screenshots, demo recordings, and concise verification records.

Add a short README explaining:

- Naming conventions.
- How evidence links relate to worklog entries.
- What each saved item demonstrates.
- Whether it shows the current version or an earlier state.

Capture evidence when tools and access permit. If capture is unavailable, record exactly what remains to be captured. Never create placeholder evidence that appears real.

Do not store credentials, sensitive personal information, unnecessary raw logs, or large generated build artifacts here.

## 3. Follow this workflow during development

Before a meaningful task:

1. Read the project context and handoff.
2. Inspect the relevant implementation.
3. Identify the requested outcome and how completion will be checked.
4. Record consequential decisions when they are made.

During the task:

1. Record significant obstacles, failed approaches, and changed decisions while their context is available.
2. Separate observed facts from assumptions.
3. Continue authorized work without requesting approval solely to maintain documentation.

After the task:

1. Run appropriate verification and report actual results.
2. Update PROJECT.md if behavior, architecture, scope, or commands changed.
3. Append a concise WORKLOG entry.
4. Refresh HANDOFF.md.
5. Save or link relevant evidence when available.
6. Update CASE-STUDY.md only when the task adds a meaningful decision, challenge, or result.

For trivial edits, update only the documents affected. Avoid documentation churn.

## 4. Keep the record trustworthy

- Never invent challenges, alternatives, tests, feedback, metrics, or outcomes.
- A successful build does not prove every workflow works.
- A screenshot demonstrates appearance, not reliability or usability.
- Report failed, skipped, and unavailable checks clearly.
- Link claims to relevant files, worklog entries, evidence, or existing commits.
- Do not fabricate commit hashes or create commits solely for this documentation setup.
- Exclude private information from the public case study.
- If my rationale is missing and matters, ask one focused question; continue independent work where possible.
- Keep documents in plain Markdown and version control alongside the project.

## 5. Prepare the case study when requested

When I request a case study, review the documents and relevant implementation, then produce a concise portfolio-ready narrative:

Problem → My role and AI involvement → Constraints → Key decisions → Challenges → Solution → Verification → Outcomes → Limitations and lessons.

Lead with the problem and resulting behavior. Select the strongest supported examples instead of listing every feature.

If results have not been measured, describe demonstrated functionality and identify the missing evidence. Do not turn aspirations into achievements.

## 6. Start now

Inspect the project and establish this system using the information available.

Then briefly report:

- Files created or updated.
- Your current understanding of the project.
- Important missing information.
- The next development step.

If no code or project idea exists yet, establish a minimal skeleton and mark unknown sections clearly. Do not invent an application.

Apply this workflow throughout subsequent tasks in this repository.
