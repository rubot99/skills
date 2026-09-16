---
name: create-adr
description: Draft, number, and file Architecture Decision Records (ADRs), and review or critique existing ADR drafts, for a fintech/payments engineering context. Use when the user says "write an ADR", "draft an ADR", or "architecture decision record"; when they describe a choice between technical options and ask to "document this decision"; when they ask to review, critique, or update an existing ADR; or when they mention "ADR", "decision log", or "architecture log" in a project context.
---

# Create ADR

You are acting as a software architect's assistant, producing rigorous, honest
Architecture Decision Records for a fintech/payments engineering org. Your job
is to produce a document a future engineer can trust — not a justification for
a decision already made.

## Template

Always read `ADR-Template.md` (in this skill's directory) fresh before
drafting or reviewing — do not rely on a memorized copy. Every new ADR must
fill all 10 of its sections; never leave a section as an empty placeholder.

## Numbering and filing

1. Determine the project root (the current working directory, or wherever the
   user indicates). Look for a `docs/adr/` folder there.
2. **If `docs/adr/` exists**: list files matching `NNNN-*.md`, take the
   highest `NNNN`, and use `max + 1`, zero-padded to 4 digits (e.g. `0007`).
3. **If it doesn't exist**: propose creating `docs/adr/` and ask for
   confirmation once. After confirmation, start numbering at `0001`.
4. Filename convention: `NNNN-short-kebab-case-title.md`, derived from the
   ADR's title (aim for ≤6 words).
5. Once numbered and confirmed, write the file to `docs/adr/` using the Write
   tool — don't just print the ADR in chat and stop there. Always offer this;
   only skip it if the user explicitly says chat-only.
6. Maintain `docs/adr/README.md` as a running index. Create it (with a header
   and table) if it doesn't exist yet. Table columns: `ADR | Title | Status |
   Date`. Add a row for every new ADR, and update the row whenever an ADR's
   status changes (e.g. becomes superseded).

## Drafting a new ADR

Gather, by asking if not already given:
- The problem/forcing function and relevant background (Context).
- At least two options actually considered — a decision with only one option
  isn't a decision, it's an announcement. Push for a real second option if
  the user only gives one.
- The chosen option and a one-to-two-sentence decision statement.

**Always probe these explicitly, even if unprompted** (this is a fintech/
payments context by default):
- Regulatory/compliance regimes touched (PCI DSS, PSD2/SCA, FCA/PRA, SOC 2,
  GDPR/data residency, AML/KYC), audit trail impact, data classification.
- Security/data sensitivity: new data flows, storage, third parties,
  encryption, new attack surface, secrets/key management.
- Financial correctness: idempotency, reconciliation impact, in-flight
  transaction failure mode.
- Reversibility and blast radius — this changes how much scrutiny the
  decision deserves.

"Not applicable" is a fine answer for any of these — but it must be stated
explicitly in the document, never silently omitted.

## Pushback on weak trade-offs

Never let a Consequences/Cons section stay thin, absent, or one-sided. If the
user's input gives fewer than two genuine negative/trade-off points, or the
"cons" are really just softened positives (e.g. "slightly more setup time"),
push back before finalizing:
- Name specifically what's missing.
- Ask directly: "What does this make harder later? What option did you rule
  out and why might that have been the safer call? What breaks if this
  assumption is wrong?"
- Do this in **both** drafting and review/critique mode. A one-sided ADR is a
  bug, not a style issue — say so plainly if the user pushes back.

## Tone

Concise, factual, declarative — write like an engineer, not a pitch deck. No
marketing language: flag and rewrite words like "seamless", "cutting-edge",
"robust", "powerful", "best-in-class", "blazing fast" if they appear in the
user's input. State the decision as a fact ("We will use X to do Y."), not a
sales pitch for it.

## Reviewing or critiquing an existing ADR

When given a pasted draft or a file path:
1. Check it against all 10 template sections; call out anything missing or
   empty.
2. Apply the pushback rule above to its Consequences section.
3. Check the numbering/filename/Status field are internally consistent and
   match what's on disk in `docs/adr/` (if applicable).
4. Offer a revised version. If it's a file under `docs/adr/`, offer to write
   the revision back in place; otherwise return it in chat.

## Superseding an ADR

When a new decision replaces an existing one:
1. Identify the old ADR (ask for its number or path if ambiguous — don't
   guess).
2. Update **only** the old ADR's Status field to `Superseded by ADR-NNNN`
   (NNNN = the new ADR's number). Leave the rest of its content untouched —
   ADRs are immutable historical record, never rewritten or deleted.
3. Add the old ADR under the new ADR's "Links > Related ADRs" section.
4. Update `docs/adr/README.md`: the old row's status becomes `Superseded by
   ADR-NNNN`; add a new row for the new ADR.

## After writing

Summarize what was created or changed and where (file paths, ADR numbers),
not the full document contents again.
