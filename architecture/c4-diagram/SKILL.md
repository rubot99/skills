---
name: c4-diagram
description: Generate C4-model architecture diagrams (System Context, Container, or Component level) as Mermaid diagrams, following a fixed notation standard so every diagram uses the same shapes, colors, and labeling instead of an ad hoc style each time. Use when the user asks for a "C4 diagram", "architecture diagram", "system context diagram", "container diagram", "component diagram", wants to "draw the architecture", "show how these systems connect/talk to each other", "map out the services", or is documenting a system's structure for an ADR, README, or design doc. Also use if the user pastes a system description or list of services and asks for a picture of how they fit together, even if they don't name C4 explicitly.
---

# C4 Diagram

You are producing C4-model architecture diagrams as Mermaid source. The point
of this skill is consistency: every diagram this produces should look like it
came from the same team, not like each one was improvised fresh.

## Notation standard

Always read `c4-notation.md` (in this skill's directory) fresh before drawing
a diagram — do not rely on a memorized copy of the conventions. It defines
the shapes, colors, labeling rules, and ID conventions to use. `examples.md`
has one full worked example at each level; skim the one matching your target
level before writing your own.

## Choosing the level

C4 has three levels worth using day to day — System Context, Container, and
Component. (There's a fourth, Code, but it's rarely worth generating and
duplicates what the code itself already shows — don't offer it unless the
user specifically asks for a class/code-level diagram.)

- **System Context** — the system as one box, its users, and the other
  systems it talks to. No internal detail. This is the right default when a
  request is ambiguous ("show me the architecture", "how does this fit
  together") — it's the level nearly everyone actually wants first.
- **Container** — zooms into the system to show its major
  deployable/runnable pieces (services, apps, databases, queues) and how
  they talk to each other. Use when the user is clearly asking about
  internal structure, or explicitly says "container diagram".
- **Component** — zooms into a single container to show the
  modules/classes/layers inside it. Only draw this when the user names a
  specific container and asks what's inside it — jumping straight to
  Component for a whole-system request produces a diagram nobody asked for
  and buries the useful information.

If the request is genuinely ambiguous about level, default to System
Context and say what you produced — don't stall on a clarifying question for
something this cheap to redo.

## Drawing the diagram

1. Identify the elements (people, systems, containers, or components,
   depending on the level) and the relationships between them from what the
   user has told you or from the code/docs you can see. **Which level to
   draw is cheap to guess and redo — which systems actually exist is not.**
   If the user hasn't named concrete systems/dependencies and you can't find
   them in visible code or docs, don't invent plausible-sounding ones (a
   fabricated "Okta" or "Stripe" makes a diagram that looks authoritative
   but describes a system that doesn't exist — actively worse than no
   diagram for something meant to document reality, e.g. onboarding a new
   engineer). Ask what the real dependencies are instead. The one exception:
   if the user explicitly wants a rough sketch/strawman to iterate on,
   placeholder elements are fine — but label them as placeholders
   (e.g. `"Identity Provider (TBD)"`) rather than naming a specific real
   vendor you're guessing at.
2. Write native Mermaid C4 syntax (`C4Context`, `C4Container`, or
   `C4Component`) following every rule in `c4-notation.md` — element types,
   colors, relationship labels, and the ordering discipline it describes
   (Mermaid's C4 support has no auto-layout, so declaration order is your
   only layout control).
3. Return the diagram as a fenced ` ```mermaid ` code block.
4. If you're writing the diagram into a file rather than just returning it in
   chat, also export a PNG next to it — follow `export.md`. Native Mermaid C4
   diagrams currently render as raw text on GitHub (GitHub's bundled Mermaid
   doesn't include the C4 extension), so the PNG is what makes the diagram
   actually visible there; VS Code's Mermaid extension renders the native
   source directly and doesn't need the PNG. If `mmdc` isn't installed, tell
   the user the one-line setup command from `export.md` rather than silently
   skipping the export.

## Self-check before returning

Before handing back the diagram, check it against `c4-notation.md`:
- Is this the right level for what was asked (not over- or under-zoomed)?
- Does every element use the correct shape/color for its type, and is
  internal vs. external clearly distinguishable?
- Does every relationship have a verb-phrase label (and a tech/protocol
  annotation where it's meaningful)?
- Is a legend included?

If something's off, fix it before returning rather than handing back a
diagram that breaks the standard.
