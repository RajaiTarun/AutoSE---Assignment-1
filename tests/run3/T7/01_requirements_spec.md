# pandas DataFrame.filter Value-Based Filtering: Requirements Specification

- Version: 0.1 (draft, non-interactive run)
- Date: 2026-10-18
- Author: agent (requirements-spec skill)
- Source document: `tests/inputs/T7_github_conflicting-discussion.md` (GitHub issue pandas-dev/pandas#61317 "ENH: Make DataFrame.filter accept filters in new formats", issue body plus 22 comments, fetched via GitHub API, unedited, undated)
- External evidence: EV-01 (DeepWiki, pola-rs/polars), raw files `ev_00_discover.txt`, `ev_00_headers.txt`, `ev_01.txt`

**Source index (used for all citations).** `OP` = issue body (@datapythonista). Comments numbered in document order:
C01 @rhshadrach (partial proposal) | C02 @datapythonista ("I like the idea") | C03 @rhshadrach (UDF, query deprecation pace) | C04 @simonjayhawkins (filter acts on labels) | C05 @datapythonista (pyspark-like, `pandas.col`) | C06 @Dr-Irv (why deprecate `query`) | C07 @datapythonista | C08 @rhshadrach (other libraries) | C09 @Dr-Irv | C10 @datapythonista | C11 @rhshadrach | C12 @simonjayhawkins (keep filter as is; deprecation message) | C13 @datapythonista (replacement for `like`/`regex`) | C14 @simonjayhawkins (boolean indexing, duplicate API) | C15 @datapythonista | C16 @simonjayhawkins ("let me think on this some more") | C17 @simonjayhawkins (#12401) | C18 @simonjayhawkins (pings @jorisvandenbossche) | C19 @Dr-Irv (history, `query` on labels+data) | C20 @rhshadrach (`cond` argument) | C21 @jbrockmendel (against string overload) | C22 @rhshadrach (maintenance, edge cases).

**Reading note.** The input is a design discussion with no recorded decision, not a stakeholder brief. Every feature in it is a proposal. Per Section 1 of the skill, a proposal is `CONFIRMED (as proposal)` and never READY. Quotes are short; see the source file for context.

---

## 1. Problem restatement and scope

**Restatement (two lines).** Contributors to the pandas library propose extending `DataFrame.filter` (today a label-based filter) so it can filter rows by value, using a boolean Series, a list-like, a query-style string or a callable, and deprecating overlapping parameters and possibly `query`. The thread did not converge: participants disagree on where value-based filtering should live, what a callable receives, whether strings are supported, how much to deprecate and how fast.

**Check of "do not invoke" cases.** Software: yes. Not a request for code or design: it is a request to settle behaviour. No approved complete spec exists. Not borderline or mixed. The skill applies. No instructions aimed at the agent were found in the input.

**In scope (proposed, contested items marked in Sections 4 to 6):**
- Accepting value-based conditions in `filter` (or, per CF-03, in `query`).
- Treatment of the existing `filter` parameters `items`, `like`, `regex`, `axis`.
- The future of `DataFrame.query` relative to `filter`.
- Validation and alignment rules for conditions.

**Out of scope (recorded, not answered):**
- Whether `query` is a good name versus `filter` (opinion exchange C09, C10, C11); no requirement derived.
- General opinions on boolean indexing as best practice (C14, C15); only the underlying concerns are recorded as RSK-02/RSK-03.
- How to implement anything (no design or code).

**Unknown scope (see Q-numbers):**
- Whether `pandas.col` (`pd.col`) itself is part of this work or a later step (Q-013, DEP-01).
- Whether other objects with a `filter` method (for example Series) are affected (Q-018). The input names only DataFrame.
- Whether the `items` parameter or the method is renamed (C01 "will maybe want to rename this argument?"; C18 "not adverse to renaming the method", a guess about a third party) (Q-007).
- Whether `DataFrame.select` is added and what it means (Q-009).

## 2. Glossary

| Term | Definition / status |
|---|---|
| filter (current) | Method that filters by index/column labels, not by contents (C04: "the filter is applied to the labels of the index"). Parameters named in the input: `items`, `like`, `regex`, `axis`. CONFIRMED |
| filter (proposed) | Method that also filters rows by value. CONFIRMED (as proposal), contested (CF-03) |
| query | `DataFrame.query`, filters by a string expression; allows labels and data combined (C19). CONFIRMED |
| UDF | User-defined function (a lambda/callable). Meaning of what it receives: undefined, see Q-003 |
| by-row | Callable invoked once per row, like `apply(..., by_row=True)` (C01). Whether adopted: undefined, see Q-003 |
| `pd.col` / `pandas.col` | Expression helper, e.g. `pd.col('age') > 3`; "discussed before" (C05), "nudging people ... toward pd.col" (C21). Existence and release status: undefined, see Q-013 |
| truthiness | Whether non-bool condition values are accepted via Python truth value (C01). Undefined, see Q-010 |
| backward compatibility | Stated as a main challenge (OP). Meaning undefined, see Q-014 |
| intuitive | Used by OP, C03, C05 to justify opposite designs. Buzzword, undefined, see Q-015 |
| simple / trivial / maintenance-wise simpler | OP ("reasonably simple"), C02 context, C21. Buzzword, undefined, see Q-015 |
| slow / long deprecation | "slow" and "1 or 2 years" (C03), "very long deprecation timeline" (C07). Only partly quantified, see Q-016 |
| syntactic sugar / duplicate API | Opinion terms used in C14, C15. Not defined; handled under CF-03 |
| method chaining | Writing successive method calls without intermediate variables (OP, C15). Working definition from usage; INFERRED |
| label-based vs value-based filtering | Filtering by index/column labels vs by cell contents (C04, C12). CONFIRMED usage |
| align on index | A Series condition is matched to the DataFrame by index label (C01). Behaviour on mismatch undefined, see Q-011 |
| cond | New parameter proposed in C20. CONFIRMED (as proposal) |

## 3. Actors

| ID | Actor | Status | Source / rationale |
|---|---|---|---|
| ACT-01 | pandas end users writing filters, including method-chaining users | CONFIRMED (term "users") / INFERRED (sub-groups) | OP ("users"), C05 ("user base"), C15 |
| ACT-02 | pandas maintainers / core team who decide on API changes | CONFIRMED (that core team is consulted: C03 "cc @pandas-dev/pandas-core") / UNRESOLVED (who has decision authority, Q-001) | C03 |
| ACT-03 | Owners of existing code using `filter(items/like/regex/axis)` or `query` | CONFIRMED (one such owner, @Dr-Irv: "I have LOTS of code that uses that") / INFERRED (wider class) | C06 |
| ACT-04 | Documentation maintainers | INFERRED | C03 ("change the docs to discourage the use of `query`") |

Named discussion participants and the positions they stated (roles beyond "participant" are not stated in the input): @datapythonista (author, proponent), @rhshadrach (proponent, refined proposal, positions shifted, see CF-05), @simonjayhawkins (clarifier, raised objections, later undecided C16), @Dr-Irv (opposes deprecating `query`), @jbrockmendel (opposes string overload), @jorisvandenbossche (pinged C18, no response in thread).

## 4. Functional requirements

All priorities are "unprioritised": the input never ranks the features (Q-019). Host method for every FR below is "filter" as proposed; CF-03 questions that choice.

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The filter method shall accept a boolean Series as its condition and return only the rows where the condition is true, equivalent to `df[cond]`. | CONFIRMED (as proposal) | OP ("df.filter(df["age"] > 18)  # same as `df[df["age"] > 18`"), C01 ("Series (will align on index)"), C02. Contested host: C06, C12, C14 | unprioritised | AC-001 | BLOCKED (CF-03, CF-04) |
| FR-002 | The filter method shall accept a non-Series list-like of booleans whose length equals the number of rows of the DataFrame. | CONFIRMED (as proposal) / UNRESOLVED (behaviour when length differs) | C01 ("Non-Series list-likes (must be same length as df)") | unprioritised | AC-002 (positive case); mismatch: none yet, see Q-011 | BLOCKED (CF-03, CF-04) |
| FR-003 | The filter method shall accept a string condition and evaluate it as `DataFrame.query` does. | CONFIRMED (as proposal) | OP ("same as `df.query("age > 18")`"), C01 ("strings a la `query`"), C20 ("`.filter` will behave exactly the same on string input"). Contested: C21, C22 | unprioritised | AC-003 | BLOCKED (CF-03, CF-05) |
| FR-004 | The filter method shall accept a callable condition. | CONFIRMED (term: callable accepted, as proposal) / UNRESOLVED (meaning: what the callable receives and returns) | OP (`df.filter(lambda df: df["age"] > 18)`), C01 ("UDFs to be discussed"), C03, C05 | unprioritised | none yet, see Q-003, Q-004 | BLOCKED (CF-01, CF-06) |
| FR-005 | The filter method shall deprecate the `like` and `regex` parameters. | CONFIRMED (as proposal) | C01 ("Deprecate `like`, `regex`; offer no alternatives"), corrected by C03 ("I should have said no _new_ alternatives"), replacement example C13. Contested: C12, C14, C18 (unverified guess) | unprioritised | none yet, see Q-008 (warning text and replacement unsettled) | BLOCKED (CF-03, CF-04, CF-07) |
| FR-006 | The filter method shall deprecate `axis=1`. | CONFIRMED (as proposal) | C01 ("Deprecate `axis=1` but add `DataFrame.select`") | unprioritised | none yet, see Q-009 | BLOCKED (CF-03, CF-07; depends on FR-007) |
| FR-007 | The library shall provide a `DataFrame.select` method. | CONFIRMED (term and as proposal) / UNRESOLVED (meaning; history C19: an older `select` was "deprecated and removed") | C01 (refs #55289), C19 | unprioritised | none yet, see Q-009 | BLOCKED (meaning of term decides what is built) |
| FR-008 | The documentation shall discourage use of `query` once the new filter forms exist. | CONFIRMED (as proposal) / UNRESOLVED (wording; whether at all, CF-02) | C03 ("first introduce filter and change the docs to discourage the use of `query`"). Contested: C06, C12, C08, C20 | unprioritised | none yet, see Q-005 | BLOCKED (CF-02) |
| FR-009 | The library shall begin deprecating `query` 1 or 2 years after the new filter forms are introduced. | CONFIRMED (as proposal) / UNRESOLVED (whether at all) | OP ("I think `.query` should be deprecated if this is implemented"), C03 ("after 1 or 2 years"), C07. Contested: C06, C12; softened by C08, C20 ("fine ... indefinitely") | unprioritised | none yet, see Q-005, Q-016 | BLOCKED (CF-02) |
| FR-010 | The filter method shall accept a new parameter `cond` and shall reject calls that specify `cond` together with any of `items`, `like`, `regex`. | CONFIRMED (as proposal) / UNRESOLVED (error type; whether `items` is repurposed instead, per C01) | C20 ("Users can either specify some nonempty subset of {items, like, regex} or `cond`, but not both") versus C01 (reuse `items`) | unprioritised | AC-004 | BLOCKED (CF-04) |
| FR-011 | When a condition contains non-boolean values, the filter method shall [treat each by Python truthiness OR raise an error: UNRESOLVED]. | UNRESOLVED (a stated lean toward truthiness, not a decision) | C01 ("I would lean toward the latter"); EV-01 (polars is strict) | unprioritised | none yet, see Q-010 | NEEDS CLARIFICATION (only a choice between two understood behaviours is missing; inherits host decision from CF-03) |
| FR-012 | When a Series condition has an index that differs from the DataFrame's, the filter method shall align it on index labels. | CONFIRMED (as proposal) for Series / UNRESOLVED (labels missing on either side; callable results, C03 "Must it be a Series with the same index as `df`, or are we going to allow alignment") | C01, C03 | unprioritised | AC-005 (same labels, different order); mismatch: none yet, see Q-011 | NEEDS CLARIFICATION (inherits host decision from CF-03) |
| FR-013 | Rows whose condition value is missing (NA) shall be excluded from the result. | EXTERNAL (candidate only, SUGGESTED, unconfirmed; not discussed in the input) | EV-01 ("Rows where the filter predicate evaluates to `False` or `null` are discarded") | unprioritised | none yet, see Q-012 | NEEDS CLARIFICATION |
| FR-014 | The filter method shall accept a `pd.col(...)` expression as its condition, e.g. `pd.col('age') > 3`. | CONFIRMED (as proposal) | C05, C15 ("If we manage to implement the syntax below"), C21 ("support `df.filter(pd.col("age") > 3)`"), C22 | unprioritised | AC-006 (precondition DEP-01) | BLOCKED (depends on DEP-01 and Q-013, an unresolved scope/availability decision) |

## 5. Non-functional requirements

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | The new filter forms shall be intuitive to users. | CONFIRMED (term) / UNRESOLVED (meaning). Measurable placeholder: [success measure and threshold TBD] | OP ("filter can be intuitive"), C03 ("I do not find it intuitive"), C05 ("more intuitive") | unprioritised | none yet, see Q-015 | NEEDS CLARIFICATION (buzzword with placeholder; unsure vs BLOCKED, so the tie-break default applies) |
| NFR-002 | The solution shall be simple to maintain. | CONFIRMED (term) / UNRESOLVED (meaning). Measurable placeholder: [measure TBD] | OP ("reasonably simple"), C21 ("maintenance-wise ... much simpler"), C22 ("In addition to maintenance") | unprioritised | none yet, see Q-015 | NEEDS CLARIFICATION |
| NFR-003 | The filter method's condition forms shall be consistent with the filter method of other DataFrame libraries (PySpark, Polars, ibis). | CONFIRMED (as stated rationale) / UNRESOLVED ("consistent" is undefined; EV-01 shows Polars does not accept strings or callables, CF-08) | C05, C08 | unprioritised | none yet, see Q-015 | NEEDS CLARIFICATION |
| NFR-004 | A value-based filter shall be expressible as a single method call inside a method chain, without bracket indexing or a named intermediate variable. | CONFIRMED (as proposal) | OP ("useful for method chaining"), C15 ("I don't think the square brackets is a good API for method chaining") | unprioritised | AC-007 | NEEDS CLARIFICATION (proposal; testable; satisfiable by several designs, so not blocked by CF-01/CF-05) |

Categories with none identified, because nothing is stated: performance (no expectation given), reliability, privacy, portability, compliance (no regulatory context in the input). Security: no requirement stated; see RSK-06 (INFERRED).

## 6. Constraints

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| CON-01 | The change shall keep backward compatibility and still work with the current `filter` parameters. | CONFIRMED (term) / UNRESOLVED (meaning: no behaviour change, or a deprecation cycle) | OP ("still work with the current parameters. And particularly, keeping backward compatibility") | unprioritised | none yet, see Q-014 | BLOCKED (CF-07; meaning of a term decides what is built) |
| CON-02 | Any deprecation shall follow a slow/long timeline. | CONFIRMED (as proposal) / UNRESOLVED (length: "1 or 2 years" is one speaker's figure) | C03, C07 ("very long deprecation timeline") | unprioritised | none yet, see Q-016 | BLOCKED (CF-02: depends on whether deprecation happens) |

## 7. Dependencies

| ID | Dependency | Status | Source |
|---|---|---|---|
| DEP-01 | `pd.col` expression feature (needed by FR-014; release state unknown) | INFERRED | C05, C21 |
| DEP-02 | `DataFrame.select` (new, FR-007) and issue #55289 | CONFIRMED (that it is referenced) / UNRESOLVED (state) | C01 |
| DEP-03 | A decision by the pandas core team (collective or named person; none recorded) | INFERRED | C03 (cc), C18 (ping) |
| DEP-04 | Prior issues #12401 and #26642 (outcomes not in the input) | CONFIRMED (referenced) / UNRESOLVED (outcome) | C17, C18 |
| DEP-05 | pandas' deprecation policy (not stated in the input) | INFERRED | C03, C12 ("When we deprecate we say something like ...") |

## 8. Risks

Likelihood and impact are not estimable from the input and are not guessed.

| ID | Risk | Status | Source |
|---|---|---|---|
| RSK-01 | Deprecating `query` forces migration of large existing code bases. | CONFIRMED (as concern raised) | C06 |
| RSK-02 | Overlapping ways to filter (boolean indexing, `filter`, `query`) confuse users. Disputed. | CONFIRMED (as concern raised; disputed in C15, partly conceded in C16) | C14 |
| RSK-03 | Mixing label-based and value-based filtering in one method, with a hard transition path for current label users. | CONFIRMED (as concern raised) | C04, C12 |
| RSK-04 | Overloading `filter` with strings brings edge cases (integer column labels), the `@` syntax tripping linters, f-string behaviour that depends on string representation; added maintenance. Typing complexity of an overloaded signature is INFERRED. | CONFIRMED (concerns) / INFERRED (typing) | C21, C22 |
| RSK-05 | Users misread the callable's argument `x` as a row/column/element rather than the whole DataFrame. | CONFIRMED (as concern raised) | C03 |
| RSK-06 | String conditions are evaluated as expressions; building them from untrusted input could be unsafe. | INFERRED (the input mentions `query/eval` only in C21; no security statement) | C21 |
| RSK-07 | The thread stalls without a decision (no resolution recorded; a named participant pinged and silent). | INFERRED | C16, C18 |
| RSK-08 | Divergence from other libraries' validation (Polars rejects non-boolean predicates) if pandas adopts truthiness. | INFERRED (from EV-01, unverified) | C01, EV-01 |

## 9. Acceptance criteria

Criteria below are written from the input's own "same as" statements. Method name is conditional on CF-03. Where the input gives no observable outcome, the requirement says "none yet" in its table row.

| ID -> requirement | Pass/fail test |
|---|---|
| AC-001 -> FR-001 | For a DataFrame with an `age` column, `df.filter(df["age"] > 18)` equals `df[df["age"] > 18]` (same rows, order, index, columns, dtypes). |
| AC-002 -> FR-002 | For a list `m` of booleans with `len(m) == len(df)`, `df.filter(m)` equals `df[m]`. (Length-mismatch behaviour: none yet, Q-011.) |
| AC-003 -> FR-003 | `df.filter("age > 18")` equals `df.query("age > 18")`. |
| AC-004 -> FR-010 | A call giving `cond` and `like` (or `items` or `regex`) raises an error (type TBD, Q-007); a call giving only `cond` succeeds. |
| AC-005 -> FR-012 | For a boolean Series `s` with the same labels as `df.index` in a different order, `df.filter(s)` equals `df[s.reindex(df.index)]`. (Mismatched labels: none yet, Q-011.) |
| AC-006 -> FR-014 | With `pd.col` available, `df.filter(pd.col("age") > 3)` equals `df[df["age"] > 3]`. |
| AC-007 -> NFR-004 | A chain `df.<step>().filter(<cond>).<step>()`, with `<cond>` a form that does not name an intermediate variable, runs as one expression and gives the expected rows. |

Requirements with no AC yet: FR-004, FR-005, FR-006, FR-007, FR-008, FR-009, FR-011, FR-013, NFR-001, NFR-002, NFR-003, CON-01, CON-02 (each points to its Q-nnn).

## 10. Traceability matrix

| Source | Requirement | Acceptance criterion |
|---|---|---|
| OP code line 1, C01, C02 | FR-001 | AC-001 |
| C01 | FR-002 | AC-002 |
| OP code line 2, C01, C20 | FR-003 | AC-003 |
| OP code line 3, C01, C03, C05 | FR-004 | none yet (Q-003, Q-004) |
| C01, C03, C13 | FR-005 | none yet (Q-008) |
| C01 | FR-006, FR-007 | none yet (Q-009) |
| OP, C03, C07 | FR-008, FR-009 | none yet (Q-005, Q-016) |
| C20 (vs C01) | FR-010 | AC-004 |
| C01, EV-01 | FR-011 | none yet (Q-010) |
| C01, C03 | FR-012 | AC-005 |
| EV-01 | FR-013 | none yet (Q-012) |
| C05, C15, C21, C22 | FR-014 | AC-006 |
| OP, C03, C05 | NFR-001 | none yet (Q-015) |
| OP, C21, C22 | NFR-002 | none yet (Q-015) |
| C05, C08 | NFR-003 | none yet (Q-015) |
| OP, C15 | NFR-004 | AC-007 |
| OP | CON-01 | none yet (Q-014) |
| C03, C07 | CON-02 | none yet (Q-016) |

## 11. Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-18 | Initial draft from the T7 input; EV-01 consulted. No answers received (non-interactive). |

## 12. Effect of external evidence

**EV-01** (DeepWiki `ask_wiki_question`, repo `pola-rs/polars`, 2026-10-18; raw: `ev_01.txt`). Status: unverified (the saved answer has mostly empty citation markers). Effects on this spec:
- Created Q-010 option "strict boolean only" next to C01's truthiness lean, and RSK-08 / CF-08 (tool vs input).
- Created candidate FR-013 (NA rows excluded) and Q-012; FR-013 stays EXTERNAL and is not a requirement until a stakeholder confirms.
- Annotated NFR-003: Polars reportedly does not accept strings or callables, so the "consistency with other libraries" rationale (C05, C08) does not obviously support FR-003/FR-004. Nothing changed in the stated requirements; the input outranks the evidence.
- No requirement was set to CONFIRMED because of EV-01.
