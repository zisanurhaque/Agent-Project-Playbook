# Handoff: Pantry "Use Soon" View (illustrative and fictional)

> **Illustrative and fictional.** This brief was never written for a real session. It is an example of the format, including the parts most handoffs leave out.

## 1. Status

The "use soon" view has been implemented and the query behind it has been written. The view is wired in as the default screen. The feature has not been through a review, and the end-to-end behaviour has not been checked by anyone other than the person who wrote it.

## 2. State of the working tree

- Files changed: the query module for expiring items, the view component, and the routing that makes the view the default. The session record names these changes but not the exact paths.
- Committed: undetermined. No `git status` output was captured in the session record, so this must be checked before continuing.
- Build and test results: not captured. Nothing is claimed here.

## 3. Decisions in force

- **The expiry window is computed when the view loads**, not stored on each item. Reason: a stored value goes stale silently, and a stale value looks the same as a correct one. See [decision-record.md](decision-record.md).
- **The window is fixed at seven days.** Configurability was deferred, not rejected. Do not add a setting without asking.
- **Expired items still appear**, marked as expired. They are not filtered out; the list is meant to prompt action, and hiding what is already wasted removes the prompt.
- Both decisions were made by the two people who use the application. Neither is provisional.

## 4. Files to read first

1. The expiring-items query. The boundary handling for "today" and "seven days" is the part most likely to be wrong.
2. The view component. Check how the empty state is rendered.
3. The route definition that makes this view the default.
4. [project-brief.md](project-brief.md), for the acceptance criteria.

## 5. Blockers and open questions

- No blocker.
- Open question, for the users rather than the next developer: should an item expiring today be visually distinguished from one expiring in a few days? The brief requires only that it appears.

## 6. Next steps

1. Run `git status` and record what it shows, so the tree state is known.
2. Add tests for the query covering four cases: an item expiring today, an item on the seventh day, an item on the eighth day, and an already-expired item. The brief's criteria 2, 3, 4, and 7 depend on these boundaries.
3. Open the view with a prepared database and check criteria 1, 5, and 6 by observation.
4. Ask for a review of the change before it is relied on.

## 7. Remaining verification

| Claim | How to check | What failure would mean |
| --- | --- | --- |
| Boundary between six, seven, and eight days is correct | Query tests with three prepared items | Off-by-one errors at the window edge; the view shows the wrong items |
| Expired items appear | Open the view with one expired item present | The most urgent items are hidden, which defeats the feature's purpose |
| Empty state is clear | Open the view with no items expiring | The view looks broken rather than informative |
| Timezone behaviour is understood | Run with a server clock set to a different timezone | "Today" is determined by the server, not the user; a known limitation, but its effect is unmeasured |

## 8. Caveats

The status in section 1 is based on the session record, not on a command run. The changed-file list is incomplete because the record does not name exact paths. Nothing in this brief has been checked against the working tree, which is the first thing the next person should do.

---

Everything above is a summary. The code is the source of truth. Inspect the files named in section 4 before acting on any of it.

*Note on this example: it deliberately leaves several fields unverified rather than claiming success. A brief that reports everything as passing, without naming a command, is the failure this format exists to prevent.*
