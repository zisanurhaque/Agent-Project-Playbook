# Examples

This directory contains one deliberately fictional example. It shows what the artifacts produced by the prompts look like when they are filled in, so you can judge whether the structure works for your project before you use it.

## What is here

[`illustrative-project/`](illustrative-project/) is a small invented project, described in four documents:

| File | Demonstrates |
| --- | --- |
| [project-brief.md](illustrative-project/project-brief.md) | The shape of a scoped request with a clear user and a bounded change |
| [decision-record.md](illustrative-project/decision-record.md) | How a decision, its alternatives, and its costs are recorded |
| [handoff.md](illustrative-project/handoff.md) | A short continuation brief that separates what is verified from what is claimed |
| [case-study-excerpt.md](illustrative-project/case-study-excerpt.md) | An outside-facing narrative that keeps intentions, results, and unknowns apart |

## What the example is not

- It is not a real project. The application, the people, the dates, and every event in it are invented.
- It is not evidence. It contains no test results, no measurements, no user feedback, and no deployment outcomes, because inventing those is exactly the failure the prompts are written to prevent. Where a real document would carry a figure, the example either states that nothing was measured or describes what would have to be measured.
- It is not proof that the prompts work. It demonstrates document structure only.

The example is written the way a real record should be written, with the unknowns left visible. That is the part worth copying.

## Adding a genuine example

A real example is far more useful than a fictional one, and [CONTRIBUTING.md](../CONTRIBUTING.md) explains what it must include. Before you add one:

1. Get permission from anyone whose work or data appears in it.
2. Remove secrets, credentials, customer names, personal data, internal hostnames, and private repository URLs. Check evidence files specifically; logs are where these hide.
3. Include only artifacts you actually produced: the prompt as used, the agent's output, the checks you ran, and what you reviewed.
4. Record what went wrong as well as what went well. An example in which everything worked is not believable and is not useful.
5. Describe your own role and the agent's role separately.
6. Do not upgrade any prompt's validation status on the strength of an example alone. An example is a demonstration; a validation record is a documented run of the prompt on a task. See [CONTRIBUTING.md](../CONTRIBUTING.md) for the difference.

Put real examples in their own directory named after the project, and add a row to the table above.
