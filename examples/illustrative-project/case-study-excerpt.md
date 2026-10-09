# Case Study Excerpt: Pantry "Use Soon" View (illustrative and fictional)

> **Illustrative and fictional.** Pantry does not exist and this work never happened. This excerpt is written from the three fictional documents in this directory and from nothing else. It contains no test results, no measurements, and no user feedback, because the fictional records contain none. It exists to show the shape of an honest write-up, including the places where it has to stop.

## One-line summary

A small self-hosted application for tracking household food, and a change that surfaces what is about to expire.

## Context and problem

Pantry stores food items with a location and an expiry date. Its intended users are two people in one household who cook at home and keep a stocked freezer.

The problem was not recording expiry dates, which is easy, but noticing them in time. Data that is entered and never looked at does not prevent waste, because nothing prompts you to check. See [project-brief.md](project-brief.md).

## Constraints

- Two users, one household. Usability for a second administrator is not a concern.
- A local database on a home server. No external services.
- The feature had to be bounded: a view and a query, not a notification system.
- The project has a data-layer test suite and a small set of end-to-end checks. Nothing beyond those was available to verify against.

## Approach

The change adds a query that returns items expiring within seven days, including items that have already expired, and makes the view built on it the default screen. The acceptance criteria are in [project-brief.md](project-brief.md): the boundary at seven days, the ordering, the treatment of today, the treatment of expired items, and an explicit empty state.

One design question dominated the work: whether the "days remaining" value should be computed when the view loads, or stored on each item. It was computed rather than stored, and the reasoning is recorded in [decision-record.md](decision-record.md).

## Key decisions and tradeoffs

**Compute the expiry window at read time.** The alternative, a stored `days_remaining` column, would have been faster to read and would have gone stale silently. A stored value that says "three days" is equally convincing whether it is three days old or thirty. Put the fragility in one testable query instead of an unwatched background job.

The cost: the query is more complex, it does more work per load, and date arithmetic now depends on the server's timezone. The timezone dependency was noted as a known limitation rather than solved. The tradeoff was accepted because a household's item list is small, and because it was not possible to measure the difference either way.

**Fixed seven-day window.** Configurable windows were deferred rather than rejected. Adding a setting means a settings surface and a migration, which the feature did not earn.

**Expired items are shown.** Filtering them out would have made the list tidier and less useful, since the items most worth knowing about are the ones already past their date.

## What was built

A query that returns items inside the expiry window, ordered by date, and a view that renders them as the application's default screen. The session record names these changes but not the exact file paths, which is a gap; see [handoff.md](handoff.md).

## Evidence of results

**None recorded.** The fictional records contain no test output, no measurements, and no observation of the running application. The handoff states that the end-to-end behaviour was never checked by anyone other than the author, and that no build or test result was captured.

This section is therefore empty, and that is the correct outcome. The temptation at this point is to write "the view displays correctly", which is a claim with nothing behind it. What can be said is limited: the query logic exists, the decision behind it is documented, and the checks that would establish whether it works are listed in [handoff.md](handoff.md).

## Intended but not demonstrated

- **Intended:** that the view reduces food waste by prompting use of expiring items.
  **Not demonstrated:** how much waste, if any, changes. Measuring this would require tracking what is thrown away over weeks, against a baseline, by people who are also changing their habits. Nothing in the records attempts it.
- **Intended:** that the seven-day window is the useful default.
  **Not demonstrated:** whether a different window would surface items better. The choice was a judgement, not a result.
- **Intended:** that computing the window at read time is fast enough.
  **Not demonstrated:** no measurement was taken in either design. The claim that the difference does not matter is reasoning about a small dataset, not a measurement.

## Contribution and attribution

Within the fiction: the requirement, the acceptance criteria, and both design decisions came from the people who use the application. The implementation, including the query boundary handling, was drafted with an AI coding agent and adjusted by hand. The case study itself was drafted from the records and edited by hand.

The records do not say how much of the final code was changed after the agent's draft, so no proportion is claimed here. `WORKLOG.md` in a real project would carry that; the fictional records do not.

## Limitations and unknowns

- The feature was never reviewed by anyone other than its author.
- The timezone dependency of "today" is unresolved. What happens when the server clock and the users' clocks disagree is unmeasured.
- Nothing was verified about behaviour with a large item list.
- The changed-file list in the handoff is incomplete, so the surface of the change is not fully known.

## What would be done differently

The records contain no retrospective, so this section is not evidence-based. The gap that stands out is procedural rather than technical: the work was finished before any check was run or captured, which is why the evidence section is empty. Capturing the verification at the time it happens is cheaper than reconstructing it later, and here it was not reconstructed at all.

---

*This excerpt follows [Case-study generation](../../prompts/08-case-study-generation.md). Its most important property is that it stops where the records stop.*
