# Assumptions, Open Questions, Conflicts and Evidence Register

Date: 2026-10-18. Source index (OP, C01 to C22) is defined in `01_requirements_spec.md`. Suggested defaults are suggestions only and are NOT applied.

## Assumptions

| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | The thread is a complete snapshot; no later comment or decision exists. | Input says "Fetched via GitHub API ... unedited" but gives no date or close state. | Spec may be stale; a decision may already exist and resolve CF-01 to CF-05. | Current issue state and any merged PR/decision from a maintainer. |
| ASM-02 | The core team decides collectively (the C03 cc to `@pandas-dev/pandas-core`), and individual participants cannot decide alone. | Participants use "I think", "lean", "propose". | If one named maintainer has decision authority, the right person to ask changes. | Answer to Q-001. |
| ASM-03 | `pd.col` is an existing or planned pandas feature. | C05, C21 refer to it as discussed and as a direction. | FR-014 depends on a non-existent feature; scope grows. | Answer to Q-013. |
| ASM-04 | Statements phrased as opinion or preference ("I lean", "I think", "I'd bet") are proposals, not decisions. | The skill treats un-agreed ideas as proposals. | Requirements could be wrongly held back, or wrongly promoted. | Stakeholder statement of decisions. |
| ASM-05 | The requirements concern public behaviour of the pandas API, not internal implementation. | Skill describes what, not how. | None material. | n/a |
| ASM-06 | Typo in OP code line 1 (`df[df["age"] > 18` with an unclosed bracket) is a transcription slip, with no meaning of its own. | Obvious from context. | Negligible. | Author confirmation. |

## Open questions

All status OPEN (non-interactive run; no stakeholder answered). Who can answer: the pandas core team in general (`@pandas-dev/pandas-core`) unless a named person is given.

| ID | Question | Why it matters | Requirements blocked | Who can answer | Suggested default (not applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | Who has the authority to decide the direction of this change (a vote of the core team, one named maintainer, someone else), and has a decision been made after the thread? | Every conflict needs a decider; none may be picked by the agent. | All BLOCKED items (via CF-01 to CF-08) | pandas core team | none | OPEN |
| Q-002 | Where should value-based filtering live: (a) in `DataFrame.filter`, (b) in `DataFrame.query` with `filter` left label-based (C06, C12), (c) nowhere, leave both unchanged (C14, possibly C16)? | Decides what is built (CF-03). | FR-001 to FR-007, FR-010, FR-014 | pandas core team | none | OPEN |
| Q-003 | What does a callable passed as the condition receive: (a) the whole DataFrame (OP, C05), (b) each row as a Series (C01, C03), (c) callables are not accepted and `pipe` is used (C01, C03)? | Decides FR-004 and its tests (CF-01). | FR-004, FR-012 (callable results), NFR-001 | pandas core team | none | OPEN |
| Q-004 | What may a callable return, and is the result validated: Series with identical index only, aligned Series, or list-likes of equal length (C03)? | Needed to write pass/fail tests. | FR-004, FR-012 | pandas core team | none | OPEN |
| Q-005 | What happens to `query`: (a) discouraged in docs then deprecated (OP, C03), (b) kept indefinitely as an alias for string `filter` (C08, C20), (c) kept and extended instead (C06, C12)? | Decides FR-008, FR-009 and impact on existing users (CF-02). | FR-008, FR-009, CON-02 | pandas core team; @Dr-Irv as affected user | none | OPEN |
| Q-006 | Should `filter` accept strings at all (OP, C01, C20 yes; C21, C22 prefer `pd.col` only), now or later? | Scope and maintenance (CF-05). | FR-003 | pandas core team, @jbrockmendel, @rhshadrach | none | OPEN |
| Q-007 | What is the API shape: (a) reuse and possibly rename `items` for the new types (C01), (b) add a new `cond` parameter, exclusive with `items`/`like`/`regex` (C20), (c) other; which of `items`, `like`, `regex` are deprecated; is the method or a parameter renamed? What error is raised for mixing? | Decides FR-005, FR-010 (CF-04). | FR-005, FR-006, FR-010, CON-01 | pandas core team; @rhshadrach (proposer) | none | OPEN |
| Q-008 | For deprecated `like`/`regex`, what is the exact warning text and replacement (C13 gives `df.filter(df["col"].index.str.contains("xxx"))`), and what replaces them for column labels (axis=1)? | A deprecation must name what to use instead (C12). | FR-005, FR-006 | @datapythonista, @rhshadrach, @simonjayhawkins | none | OPEN |
| Q-009 | What exactly is `DataFrame.select`, how does it relate to the removed older `select` (C19) and to #55289, and is `axis=1` deprecated without it? | Meaning of a new public method. | FR-006, FR-007 | pandas core team; @rhshadrach | none | OPEN |
| Q-010 | For non-boolean condition values: accept by truthiness (C01 lean) or raise an error (EV-01: Polars raises)? | Defines validation and error tests (CF-08). | FR-011 | pandas core team | none | OPEN |
| Q-011 | What happens when a Series condition's labels do not match (missing on either side), or a list-like has the wrong length: error, drop, treat as False? Which error type? | Needed for testable alignment/length rules. | FR-002, FR-012 | pandas core team | none | OPEN |
| Q-012 | How are missing (NA) condition values treated: row excluded, error, other? (Not discussed in the input.) | Common edge case; EV-01 suggests exclusion. | FR-013 | pandas core team | SUGGESTED, unconfirmed: exclude the row (EV-01) | OPEN |
| Q-013 | Is `pd.col` already available or part of this work, and is FR-014 in the first release (C15: "as a first step it needs the changes proposed in this issue")? | Scope and dependency. | FR-014 | pandas core team; @datapythonista | none | OPEN |
| Q-014 | What does "backward compatibility" mean here: no behaviour change at all for existing calls, or compatibility through a deprecation cycle with warnings? | Decides whether deprecations are allowed (CF-07). | CON-01, FR-005, FR-006, FR-010 | pandas core team | none | OPEN |
| Q-015 | How are "intuitive", "simple" and "consistent with other libraries" judged (evidence, user study, maintainer vote, precedent), and with what threshold? | Buzzwords used to justify opposite designs. | NFR-001, NFR-002, NFR-003, CF-01 | pandas core team | none | OPEN |
| Q-016 | What is the deprecation length: "1 or 2 years" (C03), "very long" (C07) or a count of releases per the pandas policy? | Needed for a testable timeline. | CON-02, FR-009 | pandas core team | none | OPEN |
| Q-017 | Has @jorisvandenbossche (pinged in C18, with #12401 and #26642) responded, and what were the outcomes of those two issues? | A named participant's input is outstanding; history may settle CF-03/CF-04. | CF-03, CF-04 dependents | @jorisvandenbossche | none | OPEN |
| Q-018 | Do other objects with a `filter` method (for example Series, if any) fall in scope, or only DataFrame? (The input names only DataFrame.) | Scope. | All FR | pandas core team | none | OPEN |
| Q-019 | What is the priority order of the condition forms (Series, list-like, string, callable, `pd.col`) and of the deprecations for a first release? | The input gives no priorities. | All FR (priority field) | pandas core team | none | OPEN |
| Q-020 | Are there security expectations for string conditions built from untrusted input? | Autonomy/security posture questions must not be guessed (RSK-06). | FR-003 | pandas core team | none | OPEN |

Total open questions: 20.

## Conflicts

Type "actual" = stated disagreement. Type "potential" = tension from undefined terms, internal inconsistency, or tool-vs-input. No conflict is resolved here. Authority to decide: see Q-001.

| ID | Type | Competing claims and sources | Who decides | Dependent requirements |
|---|---|---|---|---|
| CF-01 | actual | Callable receives the whole DataFrame: OP, C02 (via the example), C05 ("way more clear and intuitive that the lambda works the way I described"). Callable acts by row: C01 ("I would more expect ... row-wise"), C03 ("Overall, I lean toward operate by-row here, but not strongly"; whole-frame "very close to duplicating `pipe`"), C04 (supports by-row reasoning). Use `pipe` instead: C01, C03. | Unknown (Q-001) | FR-004 (and NFR-001, FR-012 for callable results) |
| CF-02 | actual | Deprecate `query`: OP, C03 (slowly, after 1 or 2 years), C07. Keep `query` and extend it instead: C06 ("I have LOTS of code that uses that"), C12. Keep `query` indefinitely as an alias: C08, C20. (@rhshadrach's own position moves from C03 to C20; recorded, not resolved.) | Unknown (Q-001) | FR-008, FR-009, CON-02 |
| CF-03 | actual | Add value-based filtering to `filter`: OP, C01, C02, C05, C07, C08, C10, C13, C15, C20. Leave `filter` label-based and expand `query`: C06, C12 ("That makes sense to me"). Leave both unchanged, avoid a duplicate API: C14; softened by C16 ("I don't disagree. Let me think on this some more."), so stance pending. | Unknown (Q-001) | FR-001 to FR-007, FR-010, FR-014 (and FR-008, FR-009) |
| CF-04 | actual | Reuse `items` for the new types, deprecate `like`/`regex`, `axis=1` to `select`: C01. Add a new `cond` parameter, exclusive with `items`/`like`/`regex`, and deprecate the former: C20 (also touches `items`). Unease about deprecating `like`/`regex` with no replacement: C12 (replacement then given in C13). A guess that @jorisvandenbossche wants to retain the syntactic sugar: C18 (unverified, third-party). | Unknown (Q-001) | FR-005, FR-006, FR-007, FR-010, CON-01 |
| CF-05 | actual | Support string conditions: OP, C01, C20. Prefer only `pd.col`, squeamish about the string overload: C21; edge-case and maintenance reasons: C22. (@rhshadrach appears on both sides across C20 and C22.) | Unknown (Q-001) | FR-003 |
| CF-06 | potential (inside one source) | OP shows `df.filter(lambda df: df["age"] > 18)` (callable gets the whole frame) yet calls it "same as `df[df['age'].apply(lambda x: x > 18)]`", where the callable is applied per element of a column. The two forms use the callable differently. Inference: the "same as" comment is loose, not a competing design. | Author (@datapythonista), then Q-003 | FR-004 (AC-test authoring) |
| CF-07 | potential | OP asks for backward compatibility and for filter to "still work with the current parameters". C01 and C20 propose deprecating `like`, `regex`, `axis=1` and (C20) `items`. The tension exists only because "backward compatibility" is undefined (a deprecation cycle may count). | Unknown (Q-001, Q-014) | CON-01, FR-005, FR-006, FR-010 |
| CF-08 | potential (tool vs input) | C01 leans toward accepting any value by truthiness; EV-01 reports Polars requires Boolean and raises otherwise, and accepts neither strings nor callables, while C05/C08 cite Polars/PySpark as the model. Per the skill the stated input outranks the evidence; EV-01 is unverified. | Unknown (Q-001, Q-010) | FR-011, NFR-003 |

## Evidence (EV)

| ID | Server | Tool | Arguments | Date | Repo | Raw file | Status |
|---|---|---|---|---|---|---|---|
| EV-01 | DeepWiki MCP, `https://mcp.deepwiki.com/mcp` (Streamable HTTP, called directly via curl; `initialize` and `tools/list` confirmed `ask_wiki_question`, `read_wiki_structure`, `read_wiki_contents`) | `ask_wiki_question` | `{repoName: "pola-rs/polars", question: "What kinds of predicate arguments does DataFrame.filter accept (boolean expressions, boolean Series, list of booleans, SQL-like strings, Python callables/lambdas), and how does it validate or reject non-boolean predicate results?"}` | 2026-10-18 | pola-rs/polars (judgment call: named in C08 as a comparable library; chosen from what the input mentioned; PySpark and ibis not queried, to keep to one focused call) | `ev_01.txt` (discovery: `ev_00_discover.txt`, `ev_00_headers.txt`) | unverified: citation markers in the answer are mostly empty |

**Summary of the relevant result.** Polars `filter` accepts boolean expressions, boolean Series, lists and NumPy arrays of booleans, and keyword equality constraints. SQL-like strings and Python callables are reportedly not accepted. Non-boolean predicates raise `TypeError` or `ComputeError`; rows where the predicate is False or null are dropped.

**Effect on the spec.** See Section 12 of `01_requirements_spec.md`: informs Q-010, Q-012, FR-013 (EXTERNAL candidate), RSK-08, CF-08, NFR-003. No stakeholder-stated requirement was changed.
