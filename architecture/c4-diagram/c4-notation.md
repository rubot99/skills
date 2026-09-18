# C4 Notation Standard

This is the fixed notation this team's C4 diagrams follow. The goal isn't
ceremony — it's that a reader who's seen one of these diagrams can read the
next one without re-learning what a color or shape means, and that diagrams
at different zoom levels visually relate to each other.

## Levels

Use exactly one of these three per diagram — never mix levels in one
picture:

| Level | Diagram type | Shows | Skip if... |
|---|---|---|---|
| System Context | `C4Context` | The system as a single box, its users, and the external systems it depends on. No internals. | Never — this is the default, almost-always-useful level. |
| Container | `C4Container` | The system's internals: the deployable/runnable pieces (web app, API, database, queue, etc.) and how they call each other. | The user only asked "what does this system do" or "who uses this" — that's Context, not Container. |
| Component | `C4Component` | The internals of *one* container: its modules, layers, or major classes. | The user hasn't named a specific container to zoom into — Component-level detail about the whole system at once is usually noise, not signal. |

The C4 model has a fourth level, Code, that goes down to class diagrams.
Don't generate it — it duplicates what the source code already shows better,
and it goes stale immediately. Only produce it if the user explicitly and
specifically asks for a class/code diagram.

**Including the end-user actor at Container/Component level:** if the
person describing a system lists its internal pieces but doesn't mention
the end user, include a `Person` for the primary end user anyway when one
obviously exists (a diagram of a system's internals with no indication of
who it's for is an unusual, harder-to-read artifact) — but don't invent a
*named* user role or job title you weren't given; a generic label like
`"User"` is fine. Omit the actor entirely only when the system genuinely
has none (a backend batch job, an internal data pipeline).

## Element types and colors

Colors follow the standard C4 model palette (the one from c4model.com):
internal elements get progressively **lighter blue** the deeper you zoom
(Person is darkest, Component is lightest), and anything **external** —
outside the team's control — is always grey, regardless of level. That
grey-vs-blue split is the one visual cue that must never be violated: a
reader should be able to tell what your team owns vs. what it depends on at
a glance.

| Element | Mermaid macro(s) | Fill | Text | Meaning |
|---|---|---|---|---|
| Person (internal) | `Person(id, label, descr)` | `#08427B` | white | A user inside the org (customer, internal staff) |
| Person (external) | `Person_Ext(id, label, descr)` | `#686868` | white | A third party / external user |
| System (in scope) | `System(id, label, descr)` | `#1168BD` | white | The system being described |
| System (external) | `System_Ext(id, label, descr)` | `#999999` | white | A system outside the team's control (SaaS, partner API, legacy system you don't own) |
| System w/ database | `SystemDb` / `SystemDb_Ext` | same as System/System_Ext | white | Same rule, database-shaped |
| System w/ queue | `SystemQueue` / `SystemQueue_Ext` | same as System/System_Ext | white | Same rule, queue-shaped |
| Container (in scope) | `Container(id, label, tech, descr)` | `#438DD5` | white | A deployable unit your team owns (service, app, DB, queue) |
| Container (external) | `Container_Ext` | `#999999` | white | A deployable unit outside the team's control |
| Container w/ database | `ContainerDb` / `ContainerDb_Ext` | same as Container/Container_Ext | white | Same rule, database-shaped |
| Container w/ queue | `ContainerQueue` / `ContainerQueue_Ext` | same as Container/Container_Ext | white | Same rule, queue-shaped |
| Component (in scope) | `Component(id, label, tech, descr)` | `#85BBF0` | black | A module/class/layer inside one container |
| Component (external) | `Component_Ext` | `#999999` | white | A component-level dependency outside the team's control |
| Boundary | `System_Boundary` / `Container_Boundary` / `Enterprise_Boundary` | none (transparent) | — | Groups elements; dashed border, label top-left. Never fill a boundary — it should let the elements inside it show their own colors. |

Always pass the `tech` argument for Container/Component elements (e.g. "Node.js",
"PostgreSQL", "Kafka") — it's one of the most useful pieces of information on
the diagram and costs nothing to include.

## Relationships

- Default to a directional `Rel(source, target, label, tech)` — most
  relationships have a clear initiator. Reserve `BiRel` for genuinely
  symmetric relationships (e.g. two nodes replicating to each other); using
  it just to avoid picking a direction is a sign the relationship needs more
  thought, not less.
- The label is always a **verb phrase from the source's point of view**:
  "Reads task data from", "Publishes events to", "Sends email via", not a
  noun phrase like "Database connection" or a vague "Uses".
- Add the 4th argument (protocol/tech: "HTTPS/JSON", "gRPC", "SMTP", "AMQP")
  whenever it's meaningful — it's usually the difference between a diagram
  that helps an on-call engineer and one that's just decoration.
- Don't use `Rel_Up`/`Rel_Down`/`Rel_Left`/`Rel_Right` unless you've actually
  hit a layout problem the default renderer can't resolve — they're an
  escape hatch, not a default.

## Naming and IDs

- IDs are short, stable `camelCase` tokens distinct from the human-readable
  label (e.g. id `apiApp`, label `"API Application"`). Keep the same ID for
  the same real-world element across a Context → Container → Component
  diagram set, so a reader (or a diff) can track one element across zoom
  levels.
- Labels are the actual product/service name a teammate would recognize —
  not generic placeholders like "System A" outside of teaching examples.

## Layout discipline

Mermaid's C4 renderer has **no auto-layout engine** — element position is
driven by the order you declare things, not by an algorithm that untangles
the graph for you. This is a real limitation relative to tools like
C4-PlantUML, and it means declaration order is your only layout lever:

1. Declare people/actors first.
2. Declare the system(s)/container(s)/component(s) in scope next, grouped
   inside their boundary.
3. Declare external dependencies last.
4. Declare all relationships after all elements.

If a rendered diagram reads confusingly (arrows crossing awkwardly, related
things far apart), reorder the declarations — don't try to fix it with
styling alone, since `UpdateElementStyle`/`UpdateRelStyle` support is
partial and inconsistent across renderers.

## Legend

Mermaid's C4 diagrams don't support an in-diagram `Legend()` the way
C4-PlantUML does (it's one of the documented unsupported features). Compensate
by always appending a short manual legend as plain text immediately below the
` ```mermaid ` block — one line is usually enough:

> **Legend:** dark blue = your system/people, grey = external, lighter blue = deeper zoom level.

This is a required part of every diagram this skill produces, not optional
polish — without it, the blue/grey convention is opaque to a first-time
reader.
