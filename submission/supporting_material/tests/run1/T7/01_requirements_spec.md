# pandas DataFrame.filter Extension Requirements Specification

Version 0.1 | 2026-10-16 | Author: agent (requirements-spec skill) | Source document: `T7_github_conflicting-discussion.md` (pandas-dev/pandas issue #61317, issue body plus 22 comments). Comments are cited below as C1..C22 in order of appearance; "OP" is the issue body by @datapythonista.

Status legend: CONFIRMED = stated in the source (here: stated as a *proposal or position by a named participant*; no decision by the pandas team appears in the source). INFERRED, EXTERNAL, UNRESOLVED as in the skill. No external evidence was consulted.

## 1. Problem restatement and scope

The pandas maintainers are debating whether `DataFrame.filter`, today a label-based selector (`items`, `like`, `regex`, `axis`), should also accept value-based conditions (boolean Series, query strings, callables), and what that means for `query`, for the existing parameters and for backward compatibility. The thread ends with participants still disagreeing; no decision is recorded.

- **In scope (stated):** value-based conditions accepted by `DataFrame.filter` (OP, C1); fate of `like`/`regex`/`axis=1` (C1, C20); relationship to `DataFrame.query` (OP, C6, C20); migration/deprecation path (C12, C20).
- **Out of scope (stated):** none stated.
- **Unknown scope:** whether any of this is adopted at all (Q-001); `pd.col` implementation (DEP-01); `DataFrame.select` (FR-008, Q-008); renaming `items` (Q-013).

Note: because the source is a design debate, many "requirements" below are candidate proposals. They are recorded so the options are visible, and are BLOCKED until the maintainers decide.

## 2. Glossary

| Term | Definition |
|---|---|
| label-based filtering | Current `DataFrame.filter` behaviour: selects by index/column labels via `items`, `like`, `regex` (C4, C19) |
| value-based filtering | Selecting rows by a condition on data values (OP) |
| UDF | User-defined function (callable) passed to `filter` (C1) |
| by-row | UDF called once per row, like `apply(..., by_row=True)` (C1) |
| whole-frame callable | UDF called once with the entire DataFrame (OP, C5) |
| `pd.col` | Proposed expression builder, e.g. `pd.col("age") > 3` (C5, C21). Existence/status: undefined, see Q-009 |
| "intuitive" | Undefined, see Q-010 |
| "reasonably simple" (OP) | Undefined, see Q-010 |
| "slow deprecation", "very long deprecation timeline", "long period" | Undefined, see Q-007 |
| "truthyness" | Undefined (any value pandas can evaluate as true/false?), see Q-006 |
| "odd edge cases" (C22) | Only example given: integer column labels. See RSK-02 |

## 3. Actors

| ID | Actor | Status | Source |
|---|---|---|---|
| ACT-01 | Users of current label-based `filter` (`items`/`like`/`regex`) | INFERRED | Implied by backward-compat concern (OP) and C12 |
| ACT-02 | Users of `DataFrame.query`, including those with large existing code bases | CONFIRMED | C6 ("I have LOTS of code that uses that") |
| ACT-03 | Users doing method chaining who want a condition-taking `filter` | CONFIRMED | OP, C15 |
| ACT-04 | Users migrating from PySpark/Polars/ibis | CONFIRMED | C5, C8 |
| ACT-05 | pandas core team / maintainers (decision makers; no named decision owner) | CONFIRMED | C3 ("cc @pandas-dev/pandas-core"); decision process UNRESOLVED, Q-011 |
| ACT-06 | Participants whose input is pending: @jorisvandenbossche (pinged, no reply in source) | CONFIRMED | C18 |
| ACT-07 | Downstream projects (type stubs, linters, wrappers) | INFERRED | C22 mentions linters; rationale: API change affects tooling. Needs confirmation |

Regulators: none identified, because the source is an open-source library API discussion with no stated regulatory context.

## 4. Functional requirements

All readiness values below follow Section 6 of the skill. "Pn" in the Source column means proposer; each is a proposal, not a decision.

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | `DataFrame.filter` shall accept a boolean Series as the condition, selecting rows equivalent to `df[cond]`. | CONFIRMED (as proposal) | OP: "`df.filter(df["age"] > 18)  # same as df[df["age"] > 18`"; C1 "Series (will align on index)". Opposed in C14, C16 | unprioritised | AC-001 (draft) | BLOCKED (CF-01) |
| FR-002 | `DataFrame.filter` shall accept a non-Series list-like condition of the same length as the DataFrame. | CONFIRMED (as proposal) | C1: "Non-Series list-likes (must be same length as df)" | unprioritised | none yet, see Q-001, Q-006 | BLOCKED (CF-01) |
| FR-003 | `DataFrame.filter` shall accept a string condition behaving as `DataFrame.query` does for the same string. | CONFIRMED (as proposal) | OP `df.filter("age > 18")`; C1; C20 "`.filter` will behave exactly the same on string input". Opposed in C21 | unprioritised | AC-003 (draft) | BLOCKED (CF-01, CF-03) |
| FR-004 | `DataFrame.filter` shall accept a callable condition. Semantics (whole-frame vs by-row) are not decided. | CONFIRMED (as proposal) for acceptance; UNRESOLVED for semantics | OP (whole-frame example); C1 "UDFs to be discussed"; C3 leans by-row; C5 prefers whole-frame | unprioritised | none yet, see Q-002 | BLOCKED (CF-02) |
| FR-005 | `DataFrame.filter` shall accept a `pd.col(...)` expression, e.g. `pd.col("age") > 3`. | CONFIRMED (as proposal) | C5, C15, C21 ("can we just support `df.filter(pd.col("age") > 3)`?") | unprioritised | none yet, see Q-009 | BLOCKED (CF-01, DEP-01 unresolved) |
| FR-006 | Existing calls to `DataFrame.filter` using `items`, `like`, `regex` and `axis` shall continue to return the same results as before this change. | CONFIRMED | OP: "still work with the current parameters ... keeping backward compatibility"; C12 | unprioritised | AC-006 | READY |
| FR-007 | `like` and `regex` of `DataFrame.filter` shall be deprecated. | CONFIRMED (as proposal) | C1 "Deprecate `like`, `regex`; offer no alternatives"; C13 suggests index-mask alternative; C12 asks what to recommend. Opposed in effect by C14, C19 | unprioritised | none yet, see Q-005 | BLOCKED (CF-04) |
| FR-008 | `axis=1` of `DataFrame.filter` shall be deprecated and a `DataFrame.select` method shall be added. | CONFIRMED (as proposal; not discussed further) | C1 | unprioritised | none yet, see Q-008 | BLOCKED (CF-01) |
| FR-009 | `DataFrame.filter` shall accept a new argument `cond`, mutually exclusive with a non-empty subset of `{items, like, regex}`; using the latter shall be deprecated. | CONFIRMED (as proposal) | C20 | unprioritised | AC-009 (draft) | BLOCKED (CF-01, CF-04) |
| FR-010 | `DataFrame.query` shall be deprecated. Alternatives in the source: deprecate after a long period (C3), keep indefinitely as an alias for `filter` on strings (C20), do not deprecate and extend `query` instead (C6). | UNRESOLVED | OP, C3, C6, C7, C20 | unprioritised | none yet, see Q-004 | BLOCKED (CF-05) |
| FR-011 | `DataFrame.filter` shall define and enforce validation of the condition's result (type of values, index alignment, list-likes). | UNRESOLVED | C1 (truthy preference), C3 ("Must it be a Series with the same index as `df`, or are we going to allow alignment?") | unprioritised | none yet, see Q-006 | BLOCKED (Q-006 undecided by any authorised stakeholder; CF-01) |

## 5. Non-functional requirements

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | Changes shall preserve backward compatibility for existing `filter` users during a deprecation period of [UNRESOLVED, duration]. | CONFIRMED (goal), UNRESOLVED (period) | OP: "keeping backward compatibility"; C3 "1 or 2 years" (for query), C7 "very long deprecation timeline" | unprioritised | none yet, see Q-007 | NEEDS CLARIFICATION |
| NFR-002 | Deprecation of any API shall start no earlier than [UNRESOLVED, SUGGESTED 1 to 2 years after the replacement is introduced and docs discourage the old API, unconfirmed]. | UNRESOLVED | C3 | unprioritised | none yet, see Q-007 | BLOCKED (CF-05) |
| NFR-003 | The new `filter` API shall be intuitive to users [UNRESOLVED, no measure]. | UNRESOLVED | OP, C5, C3 (intuitiveness disputed) | unprioritised | none yet, see Q-010 | NEEDS CLARIFICATION |
| NFR-004 | The implementation shall have low maintenance burden [UNRESOLVED, no measure]. | UNRESOLVED | C21, C22 | unprioritised | none yet, see Q-010 | NEEDS CLARIFICATION |
| NFR-005 | The `filter` API shall be consistent with `filter` in PySpark, Polars and ibis. | CONFIRMED (as stated rationale; disputed) | C8; disputed by C9, C19 | unprioritised | none yet, see Q-001 | BLOCKED (CF-01) |

## 6. Constraints

| ID | Constraint | Status | Source |
|---|---|---|---|
| CON-01 | Backward compatibility must be kept (described as the main design challenge). | CONFIRMED | OP |
| CON-02 | The new behaviour must coexist with current parameters (`items`, `like`, `regex`, `axis`). | CONFIRMED | OP |
| CON-03 | Technology, budget, deadline, legal: none identified, because the source states none. Deadline/release target: UNRESOLVED, Q-007 | n/a | n/a |

## 7. Dependencies

| ID | Dependency | Status | Source |
|---|---|---|---|
| DEP-01 | `pd.col` expression API (existence and timing unknown) | UNRESOLVED | C5 ("it was discussed before about adding"), C21 (dev-call direction) |
| DEP-02 | `DataFrame.select` (new, for column selection) | CONFIRMED (as proposal) | C1; related #55289; C19 notes an older `select` was removed |
| DEP-03 | pandas-core decision (dev call / consensus) | UNRESOLVED | C3, C21 |
| DEP-04 | Related issues #12401, #26642, #55289 (history and prior positions) | CONFIRMED | C1, C17, C18 |
| DEP-05 | Input from @jorisvandenbossche | UNRESOLVED | C18 |

## 8. Risks

| ID | Risk | Status | Source | Likelihood / impact |
|---|---|---|---|---|
| RSK-01 | Deprecating `like`/`regex` with "no alternative" breaks or irritates existing users; no migration message agreed. | CONFIRMED | C12 | not estimable from source |
| RSK-02 | String conditions carry edge cases (integer column labels, `@` syntax confusing linters, f-string dependence). | CONFIRMED | C21, C22 | not estimable |
| RSK-03 | Duplicate APIs (`filter`, `query`, boolean indexing) confuse newcomers. | CONFIRMED | C14, C15 | not estimable |
| RSK-04 | Deprecating `query` disrupts users with large code bases. | CONFIRMED | C6 | not estimable |
| RSK-05 | By-row callables act as a transpose (single dtype per row). | CONFIRMED | C3 | not estimable |
| RSK-06 | A whole-frame callable duplicates `pipe` and its argument `x` is non-obvious. | CONFIRMED | C1, C3 | not estimable |
| RSK-07 | Changing a method that currently filters labels to filter values may be hard to transition (no clean path). | CONFIRMED | C16 | not estimable |

## 9. Acceptance criteria

All ACs below are DRAFTS derived from equivalences stated in the source. They apply only if the related requirement is adopted.

| ID | Requirement | Pass/fail test |
|---|---|---|
| AC-001 | FR-001 | For a DataFrame `df` and boolean Series `s` with identical index, `df.filter(s)` equals `df[s]` (same rows, order, dtypes). Behaviour on non-identical index is not specified (Q-006). |
| AC-003 | FR-003 | For any string `q` valid for `df.query(q)`, `df.filter(q)` equals `df.query(q)`. |
| AC-006 | FR-006 | For the existing documented `filter` calls with `items`, `like`, `regex`, `axis` (as in the pre-change test suite), results are identical to pre-change results. Whether a deprecation warning is emitted is decided by FR-007/FR-009, not this AC. |
| AC-009 | FR-009 | Passing `cond` together with any of `items`, `like`, `regex` raises an error; passing only `cond` or only the legacy arguments does not. (Error type unspecified.) |

Requirements with no AC yet: FR-002, FR-004, FR-005, FR-007, FR-008, FR-010, FR-011, NFR-001 to NFR-005 (see the Q reference in each row).

## 10. Traceability matrix

| Source | Requirement | Acceptance criterion |
|---|---|---|
| OP | FR-001, FR-003, FR-004, FR-006, NFR-001, CON-01, CON-02 | AC-001, AC-003, AC-006 |
| C1 | FR-001, FR-002, FR-004, FR-007, FR-008, FR-011 | AC-001; others none yet |
| C3 | FR-004, FR-010, FR-011, NFR-002, RSK-05, RSK-06 | none yet |
| C5, C15 | FR-005, NFR-003, ACT-03, ACT-04 | none yet |
| C6 | FR-010, ACT-02, RSK-04 | none yet |
| C8 | NFR-005 | none yet |
| C12, C13 | FR-006, FR-007, RSK-01 | AC-006 |
| C14 | RSK-03 (opposition to FR-001..FR-005) | n/a |
| C16 | RSK-07 | n/a |
| C20 | FR-003, FR-009, FR-010 | AC-003, AC-009 |
| C21, C22 | FR-005, NFR-004, RSK-02 | none yet |
| EV-nn | none (no external evidence consulted) | n/a |

## 11. Change log

| Date | Version | Change |
|---|---|---|
| 2026-10-16 | 0.1 | Initial draft from the issue thread. Non-interactive run: all questions left open. |
