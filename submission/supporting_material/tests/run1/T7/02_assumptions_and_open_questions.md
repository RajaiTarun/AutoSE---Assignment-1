# Assumptions, Open Questions, Conflicts, Evidence

Source: pandas-dev/pandas issue #61317 (citations C1..C22, OP, as in 01_requirements_spec.md). Run was non-interactive; no question was answered.

## Assumptions

| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | The deliverable is a requirements spec for the pandas library API (not for a separate product). | The thread concerns `DataFrame.filter` in pandas. | Wrong scope. | Stakeholder statement of intended output |
| ASM-02 | Participants' statements are positions, not decisions; no maintainer has authority to decide alone. | No decision text appears in the source. | Spec would show BLOCKED items that are already decided elsewhere (e.g. a dev call mentioned in C21). | pandas-core decision record |
| ASM-03 | FR-006 (existing calls keep working) holds under every option in the debate, including "no change". | All positions keep current behaviour at least for a period (OP, C3, C14, C20). | FR-006 wrongly READY if a position drops old behaviour immediately. | Core team confirmation |
| ASM-04 | ACT-07 (downstream tooling) is affected. | C22 mentions linters. | Missing actor. | Maintainer input |

## Open questions

| ID | Question | Why it matters | Requirements blocked | Who can answer | Suggested default (NOT applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | Should `DataFrame.filter` be extended to value-based conditions at all, or should `filter` stay label-based (with `query` extended, C6) or everything stay unchanged (C14)? | Determines whether most requirements exist. | FR-001, FR-002, FR-003, FR-005, FR-008, FR-009, NFR-005 (and indirectly all) | pandas-core team | None. Not for the agent to pick. | OPEN |
| Q-002 | If `filter` accepts a callable, is it called once with the whole DataFrame (OP, C5), or per row (C3 leans, "not strongly"), or is `pipe` sufficient (C1)? | Different behaviour, dtype and transpose consequences (RSK-05, RSK-06). | FR-004 | pandas-core team | None | OPEN |
| Q-003 | Should string conditions be accepted by `filter` (OP, C20) or only `pd.col` expressions (C21)? | Maintenance and edge-case burden (RSK-02). | FR-003 | pandas-core team | None | OPEN |
| Q-004 | What happens to `query`: deprecate soon, deprecate after a long period, keep indefinitely as alias, or extend `query` instead? | Affects ACT-02 users. | FR-010, NFR-002 | pandas-core team | None | OPEN |
| Q-005 | What is the migration path for `items`/`like`/`regex`/`axis=1`: deprecate with no alternative (C1), recommend index boolean masks (C13), or add `cond` with mutual exclusivity (C20)? What does the deprecation message say (C12)? | Backward compatibility and RSK-01. | FR-007, FR-009 | pandas-core team | None | OPEN |
| Q-006 | What validation applies to a condition result: bool only or any truthy value (C1 leans truthy)? Must the index match or may it align? Are list-likes of equal length allowed? | Testability of all condition types. | FR-002, FR-011, AC-001 (partly) | pandas-core team | None | OPEN |
| Q-007 | What are the deprecation periods and any target release? ("slow", "1 or 2 years", "very long", "indefinitely") | Measurable NFR placeholder. | NFR-001, NFR-002 | pandas-core team | SUGGESTED, unconfirmed: C3's "1 or 2 years" after docs discourage the old API | OPEN |
| Q-008 | Is `axis=1` to be deprecated and `DataFrame.select` added (C1)? What are `select` semantics, given an older `select` was removed (C19)? Does value-based filtering apply to columns if `axis` remains (C4)? | Only one participant addressed it. | FR-008 | pandas-core team | None | OPEN |
| Q-009 | Does `pd.col` exist or will it exist, and when? | FR-005 depends on it. | FR-005 | pandas-core team | None | OPEN |
| Q-010 | What measurable definition of "intuitive", "reasonably simple" and "low maintenance" applies (e.g. user study, lines of code, count of special cases)? | NFR-003, NFR-004 are untestable as written. | NFR-003, NFR-004 | pandas-core team / issue author | None | OPEN |
| Q-011 | Who has authority to decide and by what process (dev call, vote, named maintainer)? What is @jorisvandenbossche's position (C18)? | Needed to close all conflicts. | All BLOCKED items | pandas-core team | None | OPEN |
| Q-012 | What are the priorities among the requirements? | Priorities are unstated. | none directly | pandas-core team | None | OPEN |
| Q-013 | Should the `items` argument be renamed (C1 "will maybe want to rename")? | API surface. | FR-009 | pandas-core team | None | OPEN |

## Conflicts

(ID prefix `CF-nn` is used here; the skill defines no conflict ID.)

| ID | Competing claims | Sources | Who decides | Dependants marked BLOCKED |
|---|---|---|---|---|
| CF-01 | Extend `filter` with value-based conditions and align with PySpark/Polars/ibis, **vs.** keep `filter` label-based and extend `query` instead (C6, C14 partly C16), **vs.** leave both unchanged to avoid duplicate APIs (C14). | For: OP, C1, C2, C5, C7, C8, C10, C15. Against/alternative: C6, C9, C12 (partly), C14, C19 (neutral, outlines both). | pandas-core team (not named) | FR-001, FR-002, FR-003, FR-005, FR-008, FR-009, FR-011, NFR-005 |
| CF-02 | Callable receives the whole DataFrame **vs.** callable acts by-row **vs.** no callable support (use `pipe`). | Whole: OP, C5. By-row: C3 ("lean ... not strongly"), C4 (implies). `pipe`: C1. | pandas-core team | FR-004 |
| CF-03 | `filter` accepts query strings **vs.** only `pd.col` expressions. | For: OP, C1, C20. Against: C21 (C22 supports against). | pandas-core team | FR-003 |
| CF-04 | Deprecate `like`/`regex` (offering no new alternative) **vs.** keep them and add `cond` with mutual exclusivity and deprecate mixing. These are two non-identical proposals by the same author, plus doubts about the deprecation message. | C1, C13, C20; doubts C12, C14 | pandas-core team | FR-007, FR-009 |
| CF-05 | Deprecate `query` (OP; slow per C3) **vs.** keep `query` and do not deprecate (C6) **vs.** keep as alias indefinitely (C20). | OP, C3, C6, C7, C20 | pandas-core team | FR-010, NFR-002 |

Note on intra-thread softening: some participants said they are "fine" with keeping `query` (C3, C20). This is recorded, not treated as resolution, because the deprecation intent in OP/C3 remains.

## External evidence

None. No external tool was used; no `EV-nn` entries exist. Nothing in this spec changed because of external evidence.
