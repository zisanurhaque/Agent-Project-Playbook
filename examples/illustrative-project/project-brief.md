# Project Brief: "Use Soon" View (illustrative and fictional)

> **Illustrative and fictional.** Pantry does not exist, and neither does this request. No part of this file describes real work.

## Context

Pantry tracks food items in a household. Each item has a name, a location (fridge, freezer, cupboard), and an expiry date. The application is used by two people, runs on a home server, and stores its data in a local database.

The problem it was built to solve is waste: food gets bought, stored, and forgotten until it is no longer usable. Writing the expiry date down is not the hard part. Noticing that the date is close is the hard part, because nothing prompts you to look.

## Intended user

A person who cooks at home and keeps a stocked freezer. They add items when they put shopping away, and they look at the application when they are deciding what to cook. They do not want to maintain the data carefully; they want it to tell them what to use.

## Request

Add a view that lists items expiring within the next seven days, ordered by expiry date, and make it the first thing shown when the application opens.

## Acceptance criteria

1. Opening the application shows the "use soon" list before anything else.
2. The list contains every item whose expiry date falls within the next seven days, and no others.
3. Items are ordered by expiry date, earliest first.
4. An item expiring today appears in the list.
5. Each entry shows the item name, location, expiry date, and how many days remain.
6. If nothing is expiring within seven days, the view states that clearly rather than showing an empty list.
7. Items whose expiry date has already passed still appear, marked as expired.

## Out of scope

- Notifications, email, or push.
- Editing items from the new view.
- Any change to how items are added.
- Configurable windows. Seven days is fixed for now; a setting was considered and deferred.

## Verification

The project has a test suite for its data layer and a small set of end-to-end checks. The new query is expected to be covered by tests; the view itself is expected to be checked by opening it with a prepared database.

Whatever cannot be checked this way must be recorded as unverified rather than assumed to work.
