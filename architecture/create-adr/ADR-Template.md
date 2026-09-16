# ADR-NNNN: <Short, descriptive title>

| Field | Value |
|---|---|
| **Status** | Proposed / Accepted / Rejected / Deprecated / Superseded by ADR-XXXX |
| **Date** | YYYY-MM-DD |
| **Author(s)** | |
| **Deciders** | (who approved this — e.g. Staff Eng, Security, Compliance) |
| **Domain / System** | (e.g. Payments Core, Ledger, Card Issuing, Onboarding) |

---

## 1. Context

What is the problem or forcing function? Include relevant background: current
architecture, constraints, why this decision is needed now (new regulation,
scaling limit, incident, vendor deprecation, etc.).

## 2. Decision

State the decision in one or two sentences, as if reading it out of context.
> We will use X to do Y.

## 3. Options Considered

| Option | Summary | Pros | Cons |
|---|---|---|---|
| A (chosen) | | | |
| B | | | |
| C | | | |

Briefly note why the rejected options were rejected — future readers should
not have to re-litigate this.

## 4. Regulatory & Compliance Impact

- **Applicable regimes**: (PCI DSS, PSD2/SCA, FCA/PRA, SOC 2, GDPR/data residency, AML/KYC, etc.) — or "None identified"
- **Audit trail impact**: Does this change what's logged, retained, or reportable?
- **Data classification touched**: (cardholder data, PII, transaction data, credentials, none)
- **Compliance sign-off required?**: Yes/No — who, and has it happened?

## 5. Security & Data Sensitivity

- Does this introduce new data flows, storage, or third parties handling sensitive data?
- Encryption at rest / in transit implications
- New attack surface (new external endpoint, new dependency, new trust boundary)?
- Secrets/key management impact

## 6. Financial Correctness & Risk

- Idempotency implications (does this affect at-least-once/exactly-once processing paths?)
- Reconciliation impact — does this change how money movement is verified end-to-end?
- Failure mode: what happens to in-flight transactions if this component fails?
- Money-safety review needed? (double-entry ledger, settlement, payout paths)

## 7. Consequences

**Positive**
-

**Negative / trade-offs**
-

**Neutral / follow-on work**
-

## 8. Reversibility & Blast Radius

| | |
|---|---|
| **Reversibility** | Easy / Moderate / Hard / One-way door |
| **Blast radius** | Single service / Domain / Cross-domain / Org-wide |
| **Rollback plan** | |

## 9. Operational Impact

- Monitoring/alerting changes needed
- On-call / runbook updates needed
- Migration plan and expected downtime (if any)
- Feature flag / phased rollout strategy

## 10. Links

- Related ADRs:
- Tickets / RFC / design doc:
- Incident(s) that motivated this (if applicable):

---
*Superseded ADRs should be marked with a `Status: Superseded by ADR-XXXX` header rather than deleted — ADRs are an immutable historical record.*
