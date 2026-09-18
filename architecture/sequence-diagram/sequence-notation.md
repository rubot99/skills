# Sequence Diagram Notation Standard

Mermaid's `sequenceDiagram` gives you several visually distinct arrow types
and structuring blocks. Using them precisely — rather than defaulting to one
arrow for everything — is what makes a diagram convey information instead of
just decoration. This is the fixed mapping this team uses.

## Participants

- Use `actor Name` for a human (an end user, an admin, an on-call engineer).
- Use `participant Name` for anything else — a service, an API, a database,
  a queue, a third-party system.
- Alias long or technical names so the diagram stays readable:
  `participant api as API Application`. Keep the alias stable across
  diagrams describing the same system, matching the IDs used in this
  team's C4 diagrams where applicable (see the `c4-diagram` skill) so a
  reader can connect the two.
- List participants left-to-right in the order a reader would naturally
  encounter them in the flow — normally the initiator (often a person)
  first, then each system in the order it first gets contacted. Don't
  alphabetize or reorder for its own sake; the left-to-right order *is*
  meaningful chronological framing for the reader.

## Arrow types

Mermaid distinguishes solid vs. dashed line and arrowhead style — use each
one for exactly what it visually implies, not interchangeably:

| Arrow | Syntax | Meaning | When to use |
|---|---|---|---|
| Solid, filled arrowhead | `A->>B: message` | Synchronous request/call | The caller is blocked waiting for a response |
| Dashed, filled arrowhead | `A-->>B: message` | Synchronous response/return value | The reply to a prior `->>` call — show it whenever the returned value or status matters to the story |
| Solid, open arrowhead | `A-)B: message` | Asynchronous message (fire-and-forget) | An event published, a message enqueued, a notification sent — the sender doesn't wait |
| Dashed, open arrowhead | `A--)B: message` | Asynchronous callback/response | A response to an async message that arrives later, out of band (rare — most async flows don't need this) |
| Solid line, ends in X | `A-xB: message` | Failed/lost request | The call fails to reach or complete at the target (timeout, rejected connection, dropped message) |
| Dashed line, ends in X | `A--xB: message` | Failed response | A response that itself fails (e.g. the reply is lost, or reports a hard failure like a 500) |

Trivial acknowledgement-only returns (e.g. a bare `200 OK` with no data or
status that changes the story) can be omitted to reduce clutter — but any
return whose value, error code, or absence changes what happens next must be
shown explicitly.

## Activation bars

Use `activate`/`deactivate` (or the `+`/`-` shorthand suffix, e.g.
`A->>+B: message` ... `B-->>-A: reply`) to show when a participant is
actively doing work, especially across a chain of nested calls. Rule of
thumb: activate a participant on receiving a synchronous request it's
processing, deactivate it right at (or just before) its return — but reserve
this for the tiers that actually do the work being narrated (services, APIs,
databases). Skip it for a simple relay/UI layer (a web/mobile frontend that
just forwards a click to an API and renders the result) even though it
technically also "receives a synchronous request" — activating every
participant in the chain buries the bars that actually matter under ones
that don't add information. The `+`/`-`
shorthand pairs cleanly with `->>`/`-->>` arrows; on a failure path that
ends in a `-x`/`--x` arrow instead, use an explicit `deactivate Name` line
right after it rather than trying to suffix the shorthand onto the cross
arrow — every activation opened with `+` still needs to be closed on every
path through the diagram, including failure branches. Skip
activation for participants that only ever send fire-and-forget messages —
there's no "in progress" state worth showing.

## Structuring blocks

Use these to represent control flow explicitly rather than describing it
only in prose above the diagram:

- `alt condition / else condition / end` — branching paths (e.g. validation
  succeeds vs. fails). This is the primary way to show an error path — use
  it whenever a step can meaningfully succeed or fail differently.
- `opt condition / end` — a step that only sometimes happens, with no
  alternative branch to show (e.g. an optional cache check).
- `loop condition / end` — a step repeated multiple times (e.g. retrying,
  paginating, processing a batch).
- `par action / and action / end` — actions that genuinely happen
  concurrently, not just in quick succession.
- `critical action / option fallback / end` — a critical section with an
  explicit fallback if it fails (e.g. "acquire lock" with a fallback of
  "queue and retry").
- `break condition / end` — a condition that ends the flow early (e.g. an
  auth failure that stops everything after it).

Don't nest more than two levels of these blocks — if a flow needs three
levels of `alt`/`loop` nesting to describe, it's a sign the diagram is
trying to cover too much at once; split it into two diagrams instead.

## Notes

Use `Note over A,B: text` for something that spans or concerns multiple
participants (a shared state change, an elapsed-time note, a business rule
that isn't obvious from the arrows alone), and `Note right of A` /
`Note left of A` for something about a single participant. Keep notes to one
line where possible — a note that needs a paragraph belongs in the
surrounding prose, not on the diagram.

## Labeling

Every message label is a concrete, specific verb phrase describing what's
actually sent or returned — `"POST /orders {items, total}"`,
`"Validate JWT"`, `"200 OK, order id"` — never a vague placeholder like
`"process request"` or `"handle response"`. Include a payload or status
detail only when it's essential to following the flow; don't paste full
JSON bodies into a label.

## Numbering

Turn on `autonumber` at the top of the diagram by default. It costs nothing
and lets prose reference specific steps by number ("step 4 fails because the
token has expired"), which is far more precise than "the call to the auth
service".
