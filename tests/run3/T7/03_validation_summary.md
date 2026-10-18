# Validation Summary

Date: 2026-10-18. Spec: `01_requirements_spec.md` v0.1.

## ESCALATION NEEDED

**Verdict: NOT implementation-ready. Escalation needed.** This was a non-interactive run, so the spec was finished with questions open.

Escalation triggers fired (Section 7 of the skill):
1. **More than half of the requirements are BLOCKED**: 13 of 20 (65%).
2. **A required decision belongs to a named person who has not answered**: @jorisvandenbossche was pinged in C18 and has no reply in the thread (Q-017); the core team was cc'd in C03 and no decision is recorded (Q-001).

Not fired: safety/legal/medical/financial compliance (none in input); instructions in the input aimed at the agent (none found); tool failure (none).

**What is blocked.** Everything that adds value-based filtering to `filter`, the callable semantics, string support, the deprecations (`like`, `regex`, `axis`, `query`) and the constraints on compatibility and timeline.

**What was tried.** All conflicts were logged without picking a winner (CF-01 to CF-08). One DeepWiki call (EV-01) gave comparable-library evidence, which can only inform questions (Q-010, Q-012); it cannot settle a design conflict.

**Decisions needed (from the pandas core team, or whoever Q-001 names):**
1. Q-002: does value-based filtering belong in `filter`, in `query`, or nowhere?
2. Q-003: what does a callable receive (whole frame, row, or not accepted)?
3. Q-005 and Q-006: fate of `query`; are string conditions supported?
4. Q-007 and Q-014: API shape (`items` reuse vs new `cond`), and what "backward compatibility" means.
5. Q-001 and Q-017: who decides, and the missing input from @jorisvandenbossche.

## Check results

| Check | Result | Evidence |
|---|---|---|
| Completeness | FAIL | Every Section 3 category is addressed (performance, reliability, privacy, portability, compliance recorded as "none identified, because ..."). Every requirement has ID, statement, status, source, priority and a readiness mark. But 13 requirements have no real acceptance criterion because the information is missing: FR-004 (Q-003, Q-004), FR-005 (Q-008), FR-006/FR-007 (Q-009), FR-008/FR-009 (Q-005, Q-016), FR-011 (Q-010), FR-013 (Q-012), NFR-001/NFR-002/NFR-003 (Q-015), CON-01 (Q-014), CON-02 (Q-016). Treated as FAIL per "cannot be judged because information is missing". |
| Consistency | FAIL | CF-01 to CF-08. Same term "filter" means label-based (current) and value-based (proposed). CON-01 vs FR-005/FR-006/FR-010 (CF-07). FR-010 vs C01's `items` reuse (CF-04). OP's "same as" vs its callable example (CF-06). |
| Traceability | FAIL (forward half only) | Backward: PASS, every requirement has a quoted or EV source (Section 10). Forward: 13 requirements have no acceptance criterion yet (list above). No orphan requirement and no unsourced item. |
| Feasibility | FAIL | CON-01 (keep compatibility, current parameters work) is in tension with FR-005, FR-006, FR-010 deprecations until Q-014 is answered. NFR-003 ("consistent with other libraries") does not obviously support FR-003/FR-004 given EV-01 (unverified) (CF-08). FR-014 needs DEP-01. |
| Testability | FAIL | Unmeasured terms: "intuitive" (NFR-001), "simple" (NFR-002), "consistent" (NFR-003), "slow/long" (CON-02), "backward compatibility" (CON-01). Underspecified behaviour: FR-004, FR-011, FR-012 (mismatch), FR-002 (mismatch). Testable from the input: FR-001, FR-002 (positive), FR-003, FR-010, FR-012 (reorder), FR-014, NFR-004. |
| Provenance honesty | PASS | Proposals are labelled `CONFIRMED (as proposal)`, never READY. FR-011 is UNRESOLVED (a "lean" is not a decision). FR-013 is EXTERNAL and marked SUGGESTED. ACT/DEP/RSK items that go beyond the text (ACT-04, DEP-01, DEP-03, DEP-05, RSK-06, RSK-07, RSK-08) are INFERRED. C18's guess about @jorisvandenbossche is recorded as an unverified claim, not as his position. EV-01 is EXTERNAL and flagged unverified. No INFERRED/EXTERNAL item is written as CONFIRMED. |

## Readiness table

Readiness applies to FR, NFR and CON (20 records).

| Readiness | Count | IDs |
|---|---|---|
| READY | 0 | none (all FR/NFR/CON are proposals, buzzwords or contested) |
| NEEDS CLARIFICATION | 7 | FR-011, FR-012, FR-013, NFR-001, NFR-002, NFR-003, NFR-004 |
| BLOCKED | 13 | FR-001, FR-002, FR-003, FR-004, FR-005, FR-006, FR-007, FR-008, FR-009, FR-010, FR-014, CON-01, CON-02 |

Notes on judgment calls. FR-011 and FR-012 and FR-013 are rule details that would apply wherever value-based filtering ends up, so they are NEEDS CLARIFICATION (a choice or number is missing), but at implementation time they inherit the host decision of CF-03. NFR-001 (a buzzword with a placeholder) was marked NEEDS CLARIFICATION under the "when unsure" default.

Counts by status of the 20 requirement records: CONFIRMED (as proposal) or partly so: 18 (of which several are split with UNRESOLVED parts); UNRESOLVED: 1 (FR-011); EXTERNAL: 1 (FR-013); INFERRED: 0. Open questions: 20. Conflicts: 8 (5 actual, 3 potential).

## Blocking items

- Conflicts: CF-01 (callable semantics), CF-02 (fate of `query`), CF-03 (where value-based filtering lives), CF-04 (API shape and deprecation scope), CF-05 (string support), CF-06 (OP inconsistency), CF-07 (backward compatibility vs deprecation), CF-08 (tool vs input; no BLOCKED dependents beyond NFR-003/FR-011 clarification).
- Open questions that decide what is built: Q-001, Q-002, Q-003, Q-005, Q-006, Q-007, Q-009, Q-013, Q-014, Q-017.
- Dependencies: DEP-01 (`pd.col`), DEP-02 (`select`), DEP-03 (a decision).

## Tool use

- DeepWiki MCP called directly (no MCP configured): `initialize` (200), `notifications/initialized` (202), `tools/list` (200; the three expected tools present), then one `ask_wiki_question` on `pola-rs/polars` (200, answered within the timeout). No retries needed. Raw: `ev_00_discover.txt`, `ev_00_headers.txt`, `ev_01.txt`.
- Result flagged unverified (empty citation markers). Only one repository was queried; PySpark and ibis were not.
- No injected instructions were found in the tool output.

## Plain-language verdict

The specification is not implementation-ready and has no READY items. Of 20 requirement records, 13 are BLOCKED and 7 need clarification. The thread records proposals but no decision, and its participants disagree on the core design (where filtering lives, callable meaning, strings, `query`'s future, deprecations). No subset is ready to implement. Next step: obtain the decisions listed in the escalation section (start with Q-001, Q-002, Q-003), then re-run steps 3 to 7. No implementation work was done.
