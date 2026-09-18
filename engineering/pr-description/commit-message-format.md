# Commit Message Format (Conventional Commits)

## Subject line

```
<type>(<scope>): <summary>
```

- **type** — one of:
  - `feat` — a new capability for the user
  - `fix` — a bug fix
  - `docs` — documentation only
  - `style` — formatting, whitespace, missing semicolons; no code behavior change
  - `refactor` — restructures code without changing behavior
  - `perf` — improves performance without changing behavior
  - `test` — adds or corrects tests only
  - `chore` — tooling, dependency bumps, build config, anything not shipped to users
  - `ci` — CI/CD pipeline changes
  - `build` — build system or external dependency changes
  - `revert` — reverts a previous commit
- **scope** — optional, the module/area touched (e.g. `auth`, `billing`, `api`).
  Omit the parentheses entirely if there's no meaningful scope, don't write `()`.
- **summary** — imperative mood ("add", not "added" or "adds"), lowercase first
  word, no trailing period, ideally ≤50 characters (hard cap ~72).

**Examples:**

Input: Added retry logic to the payment webhook handler so transient network
failures don't drop events.
Output: `feat(payments): retry webhook handler on transient failures`

Input: Fixed a bug where the date picker showed the wrong month on the last day
of the month in UTC-negative timezones.
Output: `fix(date-picker): correct month rollover in negative UTC offsets`

Input: Removed the unused `LegacyExporter` class and its tests.
Output: `chore: remove unused LegacyExporter`

## Body

Optional, but expected for anything non-trivial. Explain **why** the change was
made — the subject line already says what changed. Wrap at ~72 characters. Skip
the body entirely for genuinely self-explanatory changes (a typo fix, a version
bump) rather than padding it out.

## Footer

- `Refs: <TICKET-ID>` (or `Closes: <TICKET-ID>` if this commit closes the ticket) —
  include whenever a ticket ID is known.
- `BREAKING CHANGE: <description>` — required whenever the commit changes a public
  API, config format, or behavior a caller could be relying on. Describe what
  breaks and what the caller needs to do about it, not just that something broke.

## Multiple logical changes

If a diff bundles two or more unrelated changes (e.g. a bug fix and an unrelated
dependency bump), say so rather than forcing one commit message to cover both —
recommend splitting into separate commits. A commit message that needs "and" to
describe its subject line is usually a sign the commit should be split.
