# Validation Summary

Project: Sovereign Agentic Harness (Varthik's idea). Date: 2026-10-18. Spec version 0.1.

## ESCALATION NEEDED

Verdict: **not implementation-ready; escalation needed.** The run was non-interactive, so this is placed here instead of stopping to ask.

- **What is blocked:** 7 of 8 requirements and constraints are BLOCKED (more than half): FR-001, FR-002, NFR-001, NFR-002, NFR-003, NFR-004, CON-02. The remaining one (CON-01) is NEEDS CLARIFICATION. Nothing is READY.
- **What was tried:** all stated terms extracted as CONFIRMED (term) / UNRESOLVED (meaning); one DeepWiki lookup (EV-01) to generate candidate options; 16 questions and 2 conflicts logged. No further progress is possible without stakeholder answers, and none could be asked.
- **Decisions needed from a human (Varthik, or whoever holds authority):**
  1. Q-001: what the "agentic harness" is.
  2. Q-002 and Q-013: what "fully autonomous" means and whether enterprise controls may limit it.
  3. Q-003, Q-007 and Q-012: what "sovereign" and "privacy-preserving" mean, and which wins over "model-agnostic" (CF-01).
  4. Q-009: who the users are and who has decision authority.
- **Also possible escalation:** the brief may involve legal or compliance obligations (sovereignty, privacy; RSK-06, Q-016) that the agent cannot verify. Whether this trigger applies depends on Q-016.
- A required decision belongs to a named person (Varthik) who has not answered (recorded, not stalled).

## Check results

| Check | Result | Evidence |
|---|---|---|
| Completeness | PASS (structure) | All eight Section 3 categories are addressed: actors (ACT-01 to 05), FR, NFR, constraints, assumptions (ASM-01 to 05), dependencies (DEP-01 to 04), risks (RSK-01 to 06), acceptance criteria (AC-001 to 008). Empty sub-areas (budget, deadline, stack, named law, other functional detail) are explicitly "none identified, because not stated". Every requirement has ID, statement, status, source, priority ("unprioritised"), AC reference and readiness. The ACs are stubs, so content completeness is covered under Testability. |
| Consistency | FAIL | CF-01 (model-agnostic vs sovereign / privacy-preserving / local-first) and CF-02 (fully autonomous vs enterprise-grade). Terminology risk: "local-first" and "self-hosted" may overlap or mean different things (Q-004). Terms are used the same way elsewhere in the spec. |
| Traceability | PASS | Every requirement traces to a quoted phrase on L4 and forward to an AC stub (section 10). Inferred items carry a rationale. Caveat: the forward links reach stubs, not testable criteria. |
| Feasibility | FAIL | Cannot be judged for any requirement because meanings are undefined (Q-001 to Q-008). Known tensions: CF-01, CF-02. A requirement needing an unstated resource: CON-01 / DEP-02 (who provides the infrastructure, Q-005). |
| Testability | FAIL | All 8 requirements and constraints contain undefined terms and no thresholds; AC-001 to AC-008 are unfilled stubs (Q-001 to Q-008, Q-015). |
| Provenance honesty | PASS | No INFERRED or EXTERNAL item is written as CONFIRMED. Eight requirements and constraints are CONFIRMED as wording only. ACT-02 to 05, DEP, RSK, ASM are INFERRED. EV-01 is EXTERNAL and created no requirement. |

## Readiness table

| Readiness | Count | IDs |
|---|---|---|
| READY | 0 | none |
| NEEDS CLARIFICATION | 1 | CON-01 |
| BLOCKED | 7 | FR-001, FR-002, NFR-001, NFR-002, NFR-003, NFR-004, CON-02 |

## Blocking items

| Blocker | Blocks |
|---|---|
| CF-01 (Q-012) | NFR-001, NFR-002, NFR-003, CON-02 |
| CF-02 (Q-013) | FR-002, NFR-004 |
| Q-001 (scope of harness) | FR-001 (and meaning of every other item) |
| Q-002 (autonomy meaning) | FR-002 |
| Q-003 (sovereign meaning) | NFR-003 |
| Q-004 (local-first meaning) | CON-02 |
| Q-008 (enterprise-grade meaning) | NFR-004 |
| Q-005, Q-015 (self-hosted environment; success measures) | CON-01 stays NEEDS CLARIFICATION; all ACs |

## External evidence and tools

- DeepWiki MCP consulted once (EV-01, OpenHands). Discovery (`initialize`, `notifications/initialized`, `tools/list`) confirmed all three expected tools. No errors, timeouts or retries. Result relevant with caveats (see 02). Raw files: `ev_00_initialize.txt`, `ev_00_initialized.txt`, `ev_00_tools_list.txt`, `ev_01.txt`.
- Tool output contained no instructions. Nothing from it was promoted to CONFIRMED.

## Verdict

**Not implementation-ready; escalation needed.** The spec records what the stakeholder said (eight buzzwords and two slogans) and everything missing. No subset is ready. The recommended next step is for Varthik to answer Q-001, Q-002, Q-003, Q-009 and Q-012, then rerun workflow steps 3 to 7. The spec must not be passed to design or implementation yet.

## Checklist

- Every item has a status, a source (or rationale) and an ID: yes.
- No INFERRED or EXTERNAL item appears as CONFIRMED: yes.
- Every buzzword is recorded as an open question: yes (Q-001 to Q-008, Q-010, Q-011).
- Every open question lists what it blocks: yes (Q-010 and Q-014 state "none" / constraint category).
- Verdict matches the readiness table: yes.
- Tool call logged as EV-01 with its effect stated (spec section 12): yes.
- No implementation work was done: yes.
