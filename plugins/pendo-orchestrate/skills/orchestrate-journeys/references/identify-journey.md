# Identify subscription and target journey

Run before any mutation on an **existing** journey, and before create when `subId` / `appId` are unknown.

---

## Subscription and application

Use `subId` and `appId` the user gave, or call `listAllApplications`. Do not guess ids from journey names.

---

## Existing journey by name or id

**If the user gave a journey id** — call `getOrchestrateJourney`. Confirm `name` and `appId` match what they
meant.

**If the user gave a name (no id):**

1. Call `listOrchestrateJourneys` (`limit` up to 500; paginate with `offset` if needed).
2. Match by name in the results — there is no server-side name filter.
3. **Zero matches** — for **edit** intent, tell the user and ask for a different name or the journey id. For
   **create** intent, zero matches is expected — return to `references/intake.md`.
4. **One match** — call `getOrchestrateJourney` with that `id`.
5. **Two or more matches** — **STOP.** List each candidate (`id`, `name`, `status`, `appId`). Ask which
   journey to edit. Do not call any set/write tool until the user picks one.

Never mutate using a display name alone — you need a single `journeyId` first.
