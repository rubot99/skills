---
name: pr-description
description: Draft PR descriptions and commit messages in a fixed, reviewable format (ticket link, risk assessment, testing notes) instead of an ad hoc structure each time. Use when the user says "write a PR description", "draft a PR", "PR template", "write a commit message", "commit message for this", "help me describe this change", "summarize this diff for a PR", or asks to prep a change for review; also use proactively when the user has just finished a change and is about to open a PR or commit, even if they don't name the skill explicitly.
---

# PR Description & Commit Message

You are helping a developer prep a change for review. The goal is a PR
description and commit message that read like they came from the same team,
so reviewers always know where to find the ticket, the risk, and what was
actually tested — instead of hunting for that information in a different
place every time.

## Template

Always read `PR-Template.md` and `commit-message-format.md` (in this skill's
directory) fresh before drafting — do not rely on a memorized copy.

## Determining the ticket

1. Check the current git branch name for a ticket-like pattern:
   - Jira/Linear-style: `[A-Z]{2,10}-\d+` (e.g. `ABC-1234`, `ENG-42`)
   - GitHub-issue-style: a leading number or `#`-prefixed number (e.g.
     `123-fix-thing` → `#123`)
2. If a match is found, confirm it with the user rather than assuming it's
   correct — branch names get typo'd or reused.
3. If nothing matches, ask once per session: is there a ticket, and if so
   what system is it in (so you know whether to link a URL, write `#123`
   GitHub-style, or just show the raw ID)? Don't ask again later in the same
   session — reuse the answer.
4. "No ticket" is a valid, explicit answer. Never fabricate a ticket ID or
   silently drop the section — `PR-Template.md`'s Ticket section must always
   say something, even if that something is "No ticket."

## Gathering context

Before drafting, look at what actually changed: `git diff` / `git log`
against the base branch (usually `main` or `master`). Ground the Summary and
Changes sections in the real diff — a description of a change nobody made is
worse than no description.

## Risk and testing notes

These cannot be inferred from a diff alone, and guessing at them defeats the
point of having the section. If the user hasn't already told you, ask
directly:
- What could this break, and what's the rollback plan if it does?
- What did you actually test, and how?

Never write a testing claim the user didn't make. "Not tested — logic-only
change covered by the existing test suite" is a legitimate testing note; an
invented "tested manually in staging" is not.

## Drafting the commit message(s)

Apply `commit-message-format.md`. If the diff bundles multiple unrelated
changes, say so and suggest splitting into separate commits rather than
stretching one subject line to cover everything.

## Drafting the PR description

Fill every section of `PR-Template.md`. Don't leave a section as a bare
placeholder — "N/A" or "No ticket" is fine when it's true, but it must be
stated, not omitted.

## Keeping the team consistent

Check whether the target repo already has a
`.github/PULL_REQUEST_TEMPLATE.md`:
- If it matches this format, use it as the source of truth instead of
  duplicating structure.
- If it's missing, offer once to create it from `PR-Template.md` so GitHub
  pre-fills the same structure for every contributor's PRs — this is what
  actually makes the format consistent across the team, not just this one
  PR. Ask for confirmation before creating it.
- If a different template already exists, point out the mismatch and ask
  before overwriting — don't silently replace an established team
  convention.

## Tone

Concise and factual — write like an engineer describing a change to another
engineer, not a changelog for marketing. State what changed and why as fact
("Adds X so that Y."), not a sales pitch for the change.

## After drafting

Output the commit message(s) and PR description as plain text/markdown,
ready to paste into `git commit` or the PR UI. Don't run `git commit` or
`gh pr create` yourself — this skill drafts the text, it doesn't act on it.
