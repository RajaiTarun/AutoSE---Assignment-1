# Validation Summary

Date 2026-10-18 | Version 0.2 | Verdict: NOT implementation-ready. ESCALATION NEEDED.

## Escalation (Section 7)

- **What is blocked:** FR-001, FR-008, FR-009, NFR-001, CON-02 and CON-03 are BLOCKED. All of them wait on a decision from Varthik or a legal owner.
- **Legal obligations this agent cannot verify:** CON-04 requires compliance with the DPDP Act. No one with legal authority has confirmed which obligations apply (Q-008). Whether the hosted-model opt-in is lawful under the Act and the stated data-in-India rule is not something this agent can judge (RSK-01).
- **Decisions needed:**
  - From Varthik, plus the legal or compliance owner: how to reconcile "only local models, no data leaves the premises" with "admin may opt in to hosted models such as GPT" (CF-02, Q-007).
  - From Varthik: whether a private cloud counts as "the premises" (CF-03, Q-009).
  - From Varthik: the meaning of "govern" (Q-006).
- **What was tried:** the stakeholder answered round 1, and these conflicts surfaced from his own answers. A second round has not been asked, because the coordinator asked for the artefacts to be finished.

## Checks

| Check | Result | Evidence |
|---|---|---|
| Completeness | FAIL | All Section 3 categories are addressed. Eight items have no acceptance criterion yet: FR-001 (Q-006), FR-007 (Q-016), FR-009 (Q-007), FR-011 (Q-014), NFR-002 (Q-019), NFR-003 (Q-011), CON-02 (Q-009), CON-04 (Q-008). Several ACs contain TBD placeholders. |
| Consistency | FAIL | CF-02: FR-008 and NFR-001 contradict FR-009. CF-03: CON-02 against NFR-001. The term "premises" is used two ways. |
| Traceability | FAIL | Every requirement traces back to a source (S0, A1-Q00x, A1-Extra or EV-01). The forward link to an acceptance criterion is missing for the same eight items listed under Completeness. |
| Feasibility | FAIL | FR-008, NFR-001 and CON-03 cannot all hold together with FR-009 (CF-02). CON-01 needs hardware that is not stated (DEP-02, Q-010). |
| Testability | FAIL | Not yet testable from the statement alone: FR-001, FR-007, NFR-002, NFR-003 and CON-04, which have no criterion. Others are testable only after their placeholders are filled: FR-002, FR-003, FR-004, FR-005, FR-006, FR-010, NFR-004, NFR-005, CON-01. |
| Provenance honesty | PASS | INFERRED items (FR-003, the actors, the dependencies, the risks) and the EXTERNAL item FR-011 are labelled and not shown as CONFIRMED. Terms use the split form. One flag: the phrase "no per-action prompts" in FR-002 comes from my option text, which Varthik selected. The answers were relayed by the coordinator and simulated. |

**Tool use:** EV-01 consulted (DeepWiki). It is partially unverified and is logged in `02`. No external evidence was consulted for the DPDP Act, because DeepWiki cannot answer a legal question. No tool failures occurred.

## Readiness table

| Readiness | Count | IDs |
|---|---|---|
| READY | 0 | none |
| NEEDS CLARIFICATION | 14 | FR-002, FR-003, FR-004, FR-005, FR-006, FR-007, FR-010, FR-011, NFR-002, NFR-003, NFR-004, NFR-005, CON-01, CON-04 |
| BLOCKED | 6 | FR-001, FR-008, FR-009, NFR-001, CON-02, CON-03 |
| Total | 20 | 11 FR + 5 NFR + 4 CON |

**Counts by status** (split-form items count under CONFIRMED):

| Status | Count |
|---|---|
| CONFIRMED | 16 |
| INFERRED | 1 |
| EXTERNAL | 1 |
| UNRESOLVED | 2 |

Judgment calls on the tie-break:
- NFR-002 and NFR-003 are NEEDS CLARIFICATION and not BLOCKED, because the answers already supply a working meaning.
- FR-001 is BLOCKED, because the meaning of "govern" decides what is built.

Less than half of the requirements are BLOCKED (6 of 20), so the "more than half BLOCKED" escalation does not fire. The escalation above comes from the legal and conflict triggers instead.

## Blocking items

- CF-02 (Q-007) blocks FR-008, FR-009, NFR-001 and CON-03.
- CF-03 (Q-009) blocks CON-02 and NFR-001.
- Q-006 blocks FR-001.
- Open, not blocking: Q-008, Q-014, Q-010, Q-012 and others in the register.

## Verdict

Not implementation-ready. No requirement is READY, so no subset can be handed on. The spec is a correct and honest result for an input this vague: Varthik's answers settled the autonomy, scope and deployment questions. They also exposed a real contradiction in his own answers about hosted models and data leaving the premises. The recommended next step is a second round with Q-007, Q-009, Q-008, Q-014 and Q-006, and bringing in a legal or compliance owner for the DPDP Act.
