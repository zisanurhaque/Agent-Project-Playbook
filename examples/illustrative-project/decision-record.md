# Decision Record: Expiry Window Computed in the Query (illustrative and fictional)

> **Illustrative and fictional.** This decision was never made, because Pantry does not exist. The file shows the format of a decision record.

- **Status:** Accepted
- **Date:** the date the decision was made, recorded when it is made
- **Deciders:** the two people who use the application

## Context

The "use soon" view needs to know which items expire within the next seven days, and how many days remain for each. That number changes every day without any user action.

Two ways to provide it were considered.

## Options considered

### Option A: Compute the window in the query

The window is calculated when the view loads, from the stored expiry date and the current date.

- The value is always correct for the moment it is displayed.
- The query is slightly more complex, and the comparison has to be written carefully so that "today" and "in seven days" fall on the correct side of the boundary.
- Nothing needs to be kept in sync.

### Option B: Store a `days_remaining` column on each item

The number is calculated once, when an item is created, and stored.

- Reading the value is trivial.
- The value is wrong from the day after it is written unless something recalculates it. That means a scheduled job, and a scheduled job that silently stops running means a view that silently lies.
- Every write path, including any future import or edit, has to remember to recompute it.

## Decision

Option A. The window is computed when the view loads.

## Why

The two options differ in where the fragility lives. Option B puts it in a background process that nobody watches, and a stale value is indistinguishable from a correct one: an item that expired three days ago would still show the number it had on the day it was added. Option A puts the fragility in one query, which can be tested directly, and which fails loudly rather than quietly if it is wrong.

The cost of Option A is a marginally more complex query. The cost of Option B is a class of bug that only appears after the application has been running for a few days, which is the hardest kind to notice in development.

## Cost accepted

- The query does more work per load. With a household's worth of items this is not measurable in practice, but it is a real tradeoff and would need revisiting if the dataset grew by orders of magnitude.
- Date arithmetic now depends on the server's clock and timezone. An item expiring "today" is correct only relative to whatever the server believes today is. This was noted as a known limitation rather than solved.

## Alternatives not taken, and why

- **A configurable window** was deferred, not rejected. It adds a setting, a settings interface, and a migration, for a single-household application where nobody has asked for it yet.

## What would change this decision

If the item count grew large enough that the query became slow, or if items were needed in a context where the server clock was unreliable, Option B with an explicit recalculation step would become worth its cost.
