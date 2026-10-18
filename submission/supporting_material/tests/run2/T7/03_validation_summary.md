# Validation Summary

Project: pandas `DataFrame.filter` extension (issue pandas-dev/pandas#61317). Date 2026-10-18. Non-interactive run.

## ESCALATION NEEDED

**Verdict: not implementation-ready. Escalation needed.**

Triggers (skill Section 7):
- More than half of the requirements are BLOCKED: 24 of 28 (FR, NFR and constraints) are BLOCKED, 4 are NEEDS CLARIFICATION, 0 are READY.
- A required decision belongs to people who have not answered: the pandas core team (no named decider) and @jorisvandenbossche (pinged in C18, no reply in the thread).

**What is blocked:** everything that depends on the root conflict CF-01 (extend `DataFrame.filter` versus keep it label-based and extend `query`), plus CF-02 to CF-07.

**What was tried:** all conflicts were recorded without choosing a side; one read-only DeepWiki lookup (EV-01) produced comparison options only; no stakeholder could be asked in a non-interactive run.

**Decisions needed (in order):**
1. Q-001 and Q-002: which API gets the new behaviour, and who decides.
2. Q-003 and Q-004: callable semantics and the contradictory example in the issue body.
3. Q-007 and Q-008: string conditions, and the fate and timeline of `query`.
4. Q-009 and Q-010: new `cond` argument versus reusing `items`, and replacements and warnings for `like`, `regex`, `axis=1`.
5. Q-005 and Q-006: validation of callable results and bool-only versus truthiness.

## Tool use

DeepWiki MCP was called directly over HTTP (initialize, tools/list, one `ask_wiki_question` call). It succeeded on the first attempt (HTTP 200, relevant result). No retry or fallback was needed. Result logged as EV-01 (`ev_01.txt`); it created no requirement. The lookup was skipped for questions about the maintainers' own decisions, because an outside project cannot answer them.

## Check results

| Check | Result | Evidence |
|---|---|---|
| Completeness | PASS | All Section 3 categories are addressed (actors, FR, NFR, constraints, assumptions, dependencies, risks, acceptance criteria). Empty areas are stated with a reason (regulators and payers; NFR performance/security/privacy/reliability/portability/compliance; budget, deadline, regulation). Every requirement has a source, status, priority ("unprioritised") and an AC or an explicit "none yet, see Q-nnn". Note: this check is structural; whether the requirement set is semantically complete cannot be judged until scope is settled (Q-023). |
| Consistency | FAIL | FR-022 is mutually exclusive with FR-001 to FR-006, FR-009, FR-011, FR-013, FR-018, FR-020 (CF-01). FR-006 and FR-007 contradict (CF-02). FR-015 conflicts with the "keep `query`" claims and FR-017 (CF-04). FR-018 and FR-020 compete (CF-06). The term "filter" names two behaviours (label-based now, value-based proposed). C0's example contradicts itself (CF-03). |
| Traceability | FAIL | Every requirement has a source quote. These have no acceptance criterion yet: FR-008 (Q-005), FR-010 (Q-006), FR-014 (Q-011), FR-015 (Q-008), FR-016 (Q-008), FR-020 (Q-009), NFR-002 (Q-015), NFR-003 (Q-016), NFR-004 (Q-017), CON-01 (Q-014), CON-02 (Q-010). |
| Feasibility | FAIL | Mutually exclusive demands (FR-022 versus the extend-`filter` set; CF-01). Tension between CON-01 (backward compatibility) and FR-011, FR-013, FR-015 (deprecations). FR-009 needs `pd.col` (DEP-01, availability unknown, Q-012). FR-013 needs `DataFrame.select` (DEP-02, Q-011). |
| Testability | FAIL | Unmeasured terms: NFR-002 ("intuitive"), NFR-003 ("maintenance-wise simpler", "odd edge cases"), CON-01 ("backward compatibility"), FR-016 ("discourage"), FR-015 ("slow"). No observable outcome: FR-008, FR-010. Imprecise: FR-007 ("similar to `apply(..., by_row=True)`"), FR-002 and FR-003 edge behaviour (Q-018). |
| Provenance honesty | PASS | No INFERRED or EXTERNAL item is written as CONFIRMED. FR-012, ACT-05, ACT-07, ACT-08, RSK-07 to RSK-12 are INFERRED with rationale. All discussion ideas are "CONFIRMED (as proposal)" and none is READY. EV-01 is EXTERNAL and created no requirement or status change. Opinions without data (C2, C13, C15) are not treated as facts (RSK-12). |

## Counts by status (28 requirements: 22 FR, 4 NFR, 2 CON)

| Status | Count | IDs |
|---|---|---|
| CONFIRMED (no open sub-part; 12 "as proposal", 2 as stated goal/need) | 14 | FR-001, 004, 006, 009, 011, 013, 015, 016, 017, 018, 021, 022; NFR-001; CON-02 |
| CONFIRMED (term or partial) / UNRESOLVED (meaning or edge rules) split | 11 | FR-002, 003, 005, 007, 014, 019, 020; NFR-002, NFR-003, NFR-004; CON-01 |
| INFERRED | 1 | FR-012 |
| EXTERNAL | 0 | (EV-01 informs options only) |
| UNRESOLVED | 2 | FR-008, FR-010 |

Other registers: 9 actors, 8 assumptions, 24 open questions (Q-001 to Q-024), 8 conflicts (CF-01 to CF-08), 1 external evidence entry (EV-01), 7 dependencies, 12 risks, 17 acceptance criteria.

## Readiness table

| Readiness | Count | IDs |
|---|---|---|
| READY | 0 | none |
| NEEDS CLARIFICATION | 4 | NFR-001, NFR-002, NFR-003, CON-01 |
| BLOCKED | 24 | FR-001 to FR-022 (all), NFR-004, CON-02 |
| Total | 28 | |

## Blocking items

- Conflicts: CF-01 (root), CF-02, CF-03, CF-04, CF-05, CF-06, CF-07, CF-08.
- Stakeholder decisions: Q-001, Q-002, Q-003, Q-004, Q-005, Q-006, Q-007, Q-008, Q-009, Q-010, Q-013.
- Unavailable stakeholder: @jorisvandenbossche (Q-020); core-team decision authority unidentified (Q-002).
- Unavailable dependencies: `pd.col` (Q-012), `DataFrame.select` (Q-011).

## Subset that is ready

None. No requirement qualifies as READY, so no subset can be handed to design or implementation.

## Plain-language verdict

This specification records what the pandas thread actually says, but the thread has not reached a decision. The central question, whether the new filtering belongs in `filter` or in `query`, is open, and almost every other requirement depends on it. The spec is **not implementation-ready**; escalation to the pandas core team is needed. The recommended next step is to put Q-001 and Q-002 to the maintainers (and re-read the live issue, since this snapshot may be out of date, ASM-05), then re-run steps 3 to 7 of the workflow.

No implementation work was done.
