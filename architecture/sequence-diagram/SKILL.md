---
name: sequence-diagram
description: Generate sequence diagrams (call/message flows between actors, services, and systems over time) as Mermaid diagrams, following a fixed notation standard so every diagram uses the same arrow types, activation, and structuring conventions instead of an ad hoc style each time. Use when the user asks for a "sequence diagram", "message flow", "interaction diagram", wants to "trace this flow", "show the request flow", "show how these calls happen in order", "walk through the steps between services", or is documenting an API call chain, an event flow, or a multi-step process for an ADR, README, or design doc. Also use if the user describes a series of steps between systems/services and asks for a picture of the order they happen in, even if they don't say "sequence diagram" explicitly.
---

# Sequence Diagram

You are producing sequence diagrams as Mermaid source: pictures of who calls
whom, in what order, and what comes back. The point of this skill is
consistency — every diagram should use the same arrow types and structuring
conventions, so a reader who's seen one can read the next without
re-learning what a dashed line or an `alt` block means here.

## Notation standard

Always read `sequence-notation.md` (in this skill's directory) fresh before
drawing a diagram — do not rely on a memorized copy of the conventions. It
defines arrow types, activation-bar rules, block usage (`alt`/`opt`/`loop`/
`par`), and labeling style. `examples.md` has two worked examples — a simple
happy-path flow and one using branching/async/error paths — skim whichever
is closer to what you're drawing.

Unlike the C4 diagrams this skill's sibling produces, Mermaid's
`sequenceDiagram` type is mature and renders natively everywhere (GitHub,
GitLab, VS Code, Claude artifacts) — there's no rendering caveat and no PNG
export step needed here.

## Drawing the diagram

1. Identify the participants (people, services, systems) involved and the
   order they actually get contacted, from what the user has told you or
   from the code/docs you can see. List participants in the order a reader
   would naturally encounter them — usually the flow's initiator first.
2. Walk the flow step by step, choosing the arrow type and structuring
   blocks per `sequence-notation.md` for each step (synchronous call vs.
   async fire-and-forget vs. failure, branching with `alt`, repetition with
   `loop`, etc.).
3. Include the error/failure path explicitly wherever one exists — a
   sequence diagram that only shows the happy path gives a false sense that
   nothing goes wrong. Use `alt`/`else` or a failed-message arrow (`-x`) for
   this rather than leaving it out. When the user's own description didn't
   mention a failure and you're adding one on your own judgment, don't go
   looking for every conceivable way any step could fail — add the single
   branch most directly implied by the step that's obviously capable of
   failing two ways (e.g. an auth/validation check), and say in your
   response that you added it, so the user knows it wasn't part of what
   they described.
4. Turn on `autonumber` by default so steps can be referenced by number in
   surrounding prose ("step 4 fails because...").
5. Return the diagram as a fenced ` ```mermaid ` code block. If the user is
   clearly building a doc file (mid-ADR, an existing docs/ file), offer to
   save it into that file rather than only printing it in chat.

## Self-check before returning

Before handing back the diagram, check it against `sequence-notation.md`:
- Does every arrow use the correct type for what's actually happening
  (sync call vs. async vs. return vs. failure)?
- Are activation bars used consistently for anything doing real work across
  multiple steps?
- Is at least one realistic failure/branch path represented, if the flow
  has one in reality?
- Are labels concrete verb phrases (what's actually being called/returned),
  not vague placeholders like "process" or "handle"?

If something's off, fix it before returning rather than handing back a
diagram that breaks the standard.
