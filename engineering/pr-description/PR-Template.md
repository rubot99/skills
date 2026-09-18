# <type>(<scope>): <short summary>

## Summary

What changed and why, in 2-4 sentences. State it as fact ("Adds X so that Y."), not
a changelog restatement of the diff — a reviewer should understand the motivation
without opening the code.

## Ticket

Link or ID of the ticket this PR closes or relates to (e.g. `Closes ABC-1234`,
`Refs #123`). If there is no ticket, write "No ticket" explicitly — never leave this
section blank.

## Changes

- Notable change one
- Notable change two

Bullet the changes that matter to a reviewer (new behavior, removed behavior,
notable refactors). This is not a restatement of every line in the diff —
mechanical/renaming changes usually don't need their own bullet.

## Risk

| | |
|---|---|
| **Risk level** | Low / Medium / High |
| **What could break** | |
| **Rollback plan** | (revert PR / feature flag / config toggle / not applicable) |

State the risk level as a judgment call, not a formality — "Low" should mean a
reviewer can skim, "High" should mean a reviewer should slow down and check the
rollback plan is real.

## Testing Notes

- What was actually tested, and how (unit tests added, manual QA steps run,
  staging verification, etc.)
- Known gaps — what wasn't tested, and why that's an acceptable gap for this change

Never state that something was tested if it wasn't. "Not tested — logic-only change
covered by existing test suite" is a valid and honest testing note.

## Screenshots

(Only include this section for UI-visible changes. Delete it otherwise.)
