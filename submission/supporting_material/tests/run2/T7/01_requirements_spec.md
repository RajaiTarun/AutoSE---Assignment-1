# pandas DataFrame.filter Extension Requirements Specification

Version 0.1 (draft) | Date 2026-10-18 | Author: agent (requirements-spec skill) | Run mode: non-interactive

Source documents: `T7_github_conflicting-discussion.md` (GitHub issue pandas-dev/pandas#61317, issue body plus 22 comments, snapshot fetched via GitHub API). Linked issues #55289, #12401, #26642 are referenced in the thread but were not available and were not read.

**Source keys.** The thread is cited by position: `C0` = issue body (@datapythonista); `C1` = @rhshadrach "Partial proposal"; `C2` = @datapythonista "I like the idea"; `C3` = @rhshadrach (UDFs, slow query deprecation); `C4` = @simonjayhawkins ("does not filter a Dataframe on its contents"); `C5` = @datapythonista (pyspark-like); `C6` = @Dr-Irv ("why deprecate query()"); `C7` = @datapythonista ("reasonable option"); `C8` = @rhshadrach ("at odds with other DataFrame libraries"); `C9` = @Dr-Irv (SQL joke); `C10` = @datapythonista (standard terminology); `C11` = @rhshadrach ("odd name"); `C12` = @simonjayhawkins ("what the ... would be?"); `C13` = @datapythonista (index mask alternative); `C14` = @simonjayhawkins (keep methods unchanged); `C15` = @datapythonista (duplication acceptable, `pd.col`); `C16` = @simonjayhawkins ("Let me think on this"); `C17` = @simonjayhawkins (#12401); `C18` = @simonjayhawkins (ping @jorisvandenbossche); `C19` = @Dr-Irv (history, query combines labels and data); `C20` = @rhshadrach (`cond` argument); `C21` = @jbrockmendel (squeamish about strings); `C22` = @rhshadrach (agrees, edge cases).

---

## 1. Problem restatement and scope

**Restatement.** Participants in a pandas issue discuss changing `DataFrame.filter`, which today filters on index/column labels, so that it can also filter rows by a condition (boolean Series, list-like, string like `query`, callable, or `pd.col` expression). The thread contains competing designs and unresolved disagreement about whether to change `filter` at all, how to handle callables, and what happens to `query`, `like`, `regex` and `axis=1`.

**Sources available:** the single thread above (23 items, 6 named participants plus one pinged participant). No stakeholder is reachable in this run. One read-only external lookup (DeepWiki, EV-01) was made.

**Do-not-invoke check:** software request, no approved spec exists, not a pure code/design request. Skill applies. This is a change to an existing system, so the current behaviour of `filter` is known only from what participants say (C4, C19).

**In scope (as stated, all still proposals, see CF-01):**
- Allowing `DataFrame.filter` to take value-based conditions in new formats (Series, list-like, string, callable, `pd.col`) [C0, C1, C5].
- The fate of `like`, `regex`, `axis=1`, `items` and of `DataFrame.query` as a consequence [C1, C3, C20].
- Backward compatibility and a transition path for existing label-based users [C0, C12, C19].

**Out of scope (recorded, not answered):**
- The rhetorical/naming debate about whether "query" or "filter" is the better name, and whether SQL justifies "query" [C9, C10, C11]. Not a requirement; noted in Q-008.
- Design of how conditions are parsed, evaluated or implemented (this skill stops at requirements).
- Whether other libraries should change their APIs [C9, facetious].

**Unknown scope (see Q-023):** whether extending `DataFrame.query` (C6), adding `DataFrame.select` (C1), delivering `pd.col` (C5, C21), and the outcomes of #12401 and #26642 (C17, C18) are inside this change. Whether `Series.filter` is affected is not stated.

**Mostly-open result.** Because the thread is a live disagreement rather than a decision, most of this spec is BLOCKED by conflicts. This is a correct result, not a defect.

## 2. Glossary

| Term | Meaning in this spec | Status |
|---|---|---|
| filter (current) | Existing `DataFrame.filter`; "does not filter a Dataframe on its contents, the filter is applied to the labels of the index" [C4]; "operates on labels" [C19]. Parameters named in thread: `items`, `like`, `regex`, `axis`. | CONFIRMED (per C4, C19) |
| filter (proposed) | Value-based row filtering via a condition [C0]. The same word therefore names two behaviours (label-based now, value-based proposed). | CONFIRMED (term) / UNRESOLVED (target behaviour), see CF-01 |
| label-based filtering | Selecting rows/columns by index or column labels (`items`, `like`, `regex`). | CONFIRMED [C4] |
| value-based filtering | Selecting rows by a condition on data values. | CONFIRMED [C4, C19] |
| boolean indexing / mask | `df[df["age"] > 18]` [C0, C14]. | CONFIRMED |
| `query` | Existing `DataFrame.query("age > 18")`; allows "arbitrary expressions that allow filtering on both labels and data" [C19]. | CONFIRMED |
| `pipe` | Existing `DataFrame.pipe`; cited as alternative for whole-frame callables [C1, C3]. | CONFIRMED |
| UDF / callable | A user function passed as the condition [C0, C1]. Arguments and return rules undefined, see Q-003, Q-005. | CONFIRMED (term) / UNRESOLVED (meaning) |
| by-row | Callable applied to each row "similar to `apply(..., by_row=True)`" [C1]. Exact equivalence undefined. | CONFIRMED (term) / UNRESOLVED (meaning), Q-003 |
| whole-DataFrame callable | Callable receives the entire `df` ("`x` is the same as `df`") [C3, C5]. | CONFIRMED (term) |
| `pd.col` / `pandas.col` | Column-expression helper, e.g. `pd.col('age') > 3` [C5, C15, C21]. Whether it exists, or only is planned, is not stated ("it was discussed before about adding `pandas.col`" [C5]). Assumed same object under both spellings (ASM-07). | CONFIRMED (term) / UNRESOLVED (availability), Q-012 |
| `cond` | Proposed new argument to `filter` [C20]. | CONFIRMED (as proposal) |
| `DataFrame.select` | Proposed method for label/axis=1 selection [C1]; "was deprecated and removed" earlier [C19]. Semantics undefined. | CONFIRMED (term) / UNRESOLVED (meaning), Q-011 |
| truthiness | Whether any value is accepted and evaluated for truth versus bool-only [C1]. | CONFIRMED (term) / UNRESOLVED (rule), Q-006 |
| align on index | Series condition matched to `df` by index label [C1]. Missing-label behaviour undefined. | CONFIRMED (term) / UNRESOLVED (edge rules), Q-018 |
| backward compatibility | Existing `filter` calls keep working [C0]. Duration, warnings and what counts as a break are undefined. | CONFIRMED (term) / UNRESOLVED (meaning), Q-014 |
| "slow" / "very long" deprecation | "after 1 or 2 years, start the deprecation process" [C3]; "a very long deprecation timeline" [C7]. | CONFIRMED (bound: 1 to 2 years before deprecation starts, one participant's preference) / UNRESOLVED (agreed timeline), Q-008 |
| intuitive | Used in C0, C3 and C5; participants disagree on what is intuitive [C3 vs C5]. Undefined buzzword. | undefined, see Q-015 |
| "reasonably simple", "trivial", "much simpler", "odd edge cases" | Unmeasured effort/maintenance claims [C0, C2, C21, C22]. | undefined, see Q-016 |
| "most common used case by far", "most important and convenient API change", "more popular" | Opinions without data [C2, C13, C15]. Not treated as facts. | UNRESOLVED, see Q-010 |
| method chaining | Using the result of one method as input to the next without named intermediates [C0, C15]. | CONFIRMED (term) |
| syntactic sugar / duplicate API | Methods offering a shorter way of doing something already possible [C14, C15]. | CONFIRMED (term) |

## 3. Actors

| ID | Actor | Status | Source / rationale |
|---|---|---|---|
| ACT-01 | pandas users who filter DataFrame rows by value | CONFIRMED | C0 "very nice for users" |
| ACT-02 | Existing users of label-based `DataFrame.filter` (`items`, `like`, `regex`, `axis`) | CONFIRMED | C19 "those using `filter()` for filtering on labels" |
| ACT-03 | Existing users of `DataFrame.query` | CONFIRMED | C6 "I have LOTS of code that uses that" |
| ACT-04 | pandas core team (cc'd for input; decision authority) | CONFIRMED (role as reviewer) / UNRESOLVED (who decides) | C3 "cc @pandas-dev/pandas-core for any thoughts"; Q-002 |
| ACT-05 | Maintainers/implementers of the pandas code base | INFERRED | C21, C22 raise "maintenance-wise" concerns; needs confirmation |
| ACT-06 | New users / newcomers | CONFIRMED | C14 "especially for newcomers" |
| ACT-07 | Documentation maintainers | INFERRED | C3 "change the docs to discourage the use of `query`" implies a docs owner |
| ACT-08 | Users coming from PySpark, Polars, ibis | INFERRED | C5, C8 imply such users; not stated as a user class |
| ACT-09 | @jorisvandenbossche (pinged, no answer in the thread) | CONFIRMED (pinged) / UNRESOLVED (position) | C18; participant in #12401 and #26642 |

Regulators, payers: none identified, because the input is an open-source library API discussion and states no funding, regulatory or commercial party.

## 4. Functional requirements

All "CONFIRMED (as proposal)" entries are ideas put forward in the discussion and not agreed (Section 1 of the skill). They are never READY. Priority is "unprioritised" for every item: the thread states no priorities. `Alt` marks requirements that exist only as competing alternatives.

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | `DataFrame.filter` shall accept a boolean Series as the condition and return the rows where it is true. | CONFIRMED (as proposal) | C0: "`df.filter(df["age"] > 18)  # same as `df[df["age"] > 18`" | unprioritised | AC-001 | BLOCKED (CF-01, CF-07) |
| FR-002 | A Series condition shall be aligned to `df` on the index. | CONFIRMED (as proposal) / UNRESOLVED (missing/duplicate labels) | C1: "Series (will align on index)" | unprioritised | AC-002 | BLOCKED (CF-01); edge rules Q-018 |
| FR-003 | `DataFrame.filter` shall accept a non-Series list-like condition whose length equals the length of `df`. | CONFIRMED (as proposal) / UNRESOLVED (mismatch behaviour) | C1: "Non-Series list-likes (must be same length as df)" | unprioritised | AC-003 | BLOCKED (CF-01, CF-06); Q-018 |
| FR-004 | `DataFrame.filter` shall accept a string condition and return the same result as `DataFrame.query` for that string. | CONFIRMED (as proposal) | C0: "`df.filter("age > 18")  # same as `df.query("age > 18")`"; C1: "strings a la `query`"; C20: "`.filter` will behave exactly the same on string input" | unprioritised | AC-004 | BLOCKED (CF-01, CF-05) |
| FR-005 | `DataFrame.filter` shall accept a callable as the condition. | CONFIRMED (as proposal; callable accepted) / UNRESOLVED (argument and return semantics) | C0: "`df.filter(lambda df: df["age"] > 18)`"; C1: "UDFs to be discussed." | unprioritised | AC-005 | BLOCKED (CF-01, CF-02) |
| FR-006 | **Alt A.** The callable shall receive the entire DataFrame as its argument. | CONFIRMED (as proposal) | C0 example; C5: "the function receiving the whole dataframe"; C5: "it's way more clear and intuitive that the lambda works the way I described" | unprioritised | AC-006 | BLOCKED (CF-02, CF-03) |
| FR-007 | **Alt B.** The callable shall be applied to each row ("by-row"). | CONFIRMED (as proposal; stated as a lean) / UNRESOLVED (exact semantics) | C1: "I would more expect passing a UDF to `filter` would operate row-wise similar to `apply(..., by_row=True)`"; C3: "Overall, I lean toward operate by-row here, but not strongly." C4 adds that the row Series name could carry the index label. | unprioritised | AC-007 | BLOCKED (CF-02) |
| FR-008 | `DataFrame.filter` shall validate the value returned by a callable according to a rule that is not yet decided. | UNRESOLVED | C3: "what is the validation on the result? Must it be a Series with the same index as `df`, or are we going to allow alignment. Can users returns list-likes of the same length?" | unprioritised | none yet, see Q-005 | BLOCKED (CF-02) |
| FR-009 | `DataFrame.filter` shall accept a `pd.col` expression as the condition, e.g. `df.filter(pd.col('age') > 3)`. | CONFIRMED (as proposal) | C5: "`df.filter(pd.col('age') > 3)`"; C15 (same); C21: "can we just support `df.filter(pd.col("age") > 3)`?" | unprioritised | AC-008 | BLOCKED (CF-01; DEP-01, Q-012) |
| FR-010 | `DataFrame.filter` shall apply a rule, not yet decided, for whether condition values must be bool or may be any value evaluated for truthiness. | UNRESOLVED | C1: "Do we require these to be `bool/np.bool_`, or do we allow any value an internally pandas will evaluate the truthyness of it. I would lean toward the latter." (one participant's lean). EV-01 shows a comparable library requires Boolean dtype (EXTERNAL, see CF-08). | unprioritised | none yet, see Q-006 | BLOCKED (CF-01, CF-08) |
| FR-011 | The `like` and `regex` parameters of `DataFrame.filter` shall be deprecated. | CONFIRMED (as proposal) | C1: "Deprecate `like`, `regex`; offer no alternatives." (clarified in C3 as "no _new_ alternatives"); questioned in C12; opposed by implication in C14 | unprioritised | AC-009 | BLOCKED (CF-01, CF-06) |
| FR-012 | Each deprecation warning issued under FR-011/FR-013 shall state a replacement. | INFERRED. Rationale: C12 describes the project convention ("When we deprecate we say something like 'x is deprecated. use ... instead'"), not a requirement; the shall-statement is the agent's reading. | C12 | unprioritised | AC-010 | BLOCKED (depends on FR-011/FR-013; Q-010) |
| FR-013 | `axis=1` use in `DataFrame.filter` shall be deprecated. | CONFIRMED (as proposal) | C1: "Deprecate `axis=1` but add `DataFrame.select`" | unprioritised | AC-011 | BLOCKED (CF-01, CF-06) |
| FR-014 | A `DataFrame.select` method shall be added for column-label selection. | CONFIRMED (as proposal; method name) / UNRESOLVED (semantics, scope) | C1: "add `DataFrame.select` (somewhat talked about in https://github.com/pandas-dev/pandas/issues/55289)" | unprioritised | none yet, see Q-011 | BLOCKED (CF-01, CF-06) |
| FR-015 | `DataFrame.query` shall be deprecated. | CONFIRMED (as proposal; timing contested) | C0: "I think `.query` should be deprecated if this is implemented in `.filter`"; C3: "I agree, but desire the deprecation would be slow"; contested C6, C20 | unprioritised | none yet, see Q-008 | BLOCKED (CF-04) |
| FR-016 | Before any `query` deprecation begins, the documentation shall discourage use of `query`. | CONFIRMED (as proposal) | C3: "first introduce filter and change the docs to discourage the use of `query`. Then after 1 or 2 years, start the deprecation process." | unprioritised | none yet, see Q-008 ("discourage" unmeasured) | BLOCKED (CF-04) |
| FR-017 | **Alt B.** `DataFrame.query` shall accept the same condition forms (e.g. a boolean Series). | CONFIRMED (as proposal) | C6: "maybe expand `query()` to take expressions as proposed here. So that `df.query(df["age"] > 18)` and `df.query("age > 18")` would do the same thing" | unprioritised | AC-012 | BLOCKED (CF-01, CF-04) |
| FR-018 | **Alt C.** `DataFrame.filter` shall gain a new argument `cond` that takes the new condition forms. | CONFIRMED (as proposal) | C20: "I would propose to add a new argument `cond`." | unprioritised | AC-013 | BLOCKED (CF-06) |
| FR-019 | Specifying `cond` together with any of `items`, `like`, `regex` shall be disallowed. | CONFIRMED (as proposal) / UNRESOLVED (error type) | C20: "Users can either specify some nonempty subset of `{items, like, regex}` or `cond`, but not both." | unprioritised | AC-014 | BLOCKED (CF-06) |
| FR-020 | **Alt D (competes with FR-018).** The existing first parameter `items` shall accept the new condition forms, possibly renamed. | CONFIRMED (as proposal) / UNRESOLVED (rename) | C1: "Accept `items` (will maybe want to rename this argument?) of type: ..." | unprioritised | none yet, see Q-009 | BLOCKED (CF-06) |
| FR-021 | A string condition in `DataFrame.filter` shall support expressions combining index-level names and column values. | CONFIRMED (as proposal) | C19: "it allows filtering on a combination of labels and data"; example `df.query("foo.str.contains('xxx')")`; C20: "`.filter` will behave exactly the same on string input." | unprioritised | AC-015 | BLOCKED (CF-01, CF-05) |
| FR-022 | **Alt B.** `DataFrame.filter` shall remain label-based and otherwise unchanged. | CONFIRMED (as proposal) | C6: "Why not leave `filter` as is - it operates on labels"; C14: "it is perhaps more intuitive to keep these methods unchanged." | unprioritised | AC-016 | BLOCKED (CF-01) |

FR-022 is mutually exclusive with FR-001 to FR-006, FR-009, FR-011, FR-013, FR-018 and FR-020. It is recorded, not resolved (CF-01).

## 5. Non-functional requirements

Categories performance, security, privacy, reliability, portability, compliance: none identified, because the thread states no target in any of these. (Security of evaluating user-supplied strings is not mentioned; raised only as low-impact Q-022.)

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | The new condition forms shall be usable in a method chain without a named intermediate DataFrame. | CONFIRMED | C0: "useful for method chaining"; C15: "I don't think the square brackets is a good API for method chaining" | unprioritised | AC-017 | NEEDS CLARIFICATION (AC is a draft; applies only to forms that survive CF-01) |
| NFR-002 | The API shall be "intuitive" [measurable target: UNRESOLVED placeholder]. | CONFIRMED (term) / UNRESOLVED (meaning, measure) | C0: "intuitive"; C3 and C5 disagree on which callable form is intuitive | unprioritised | none yet, see Q-015 | NEEDS CLARIFICATION |
| NFR-003 | The implementation shall have low maintenance burden and no "odd edge cases" [measurable target: UNRESOLVED placeholder]. | CONFIRMED (term) / UNRESOLVED (measure) | C21: "the maintenance-wise I think much simpler"; C22: "it has no odd edge cases (e.g. integer column labels)" | unprioritised | none yet, see Q-016 | NEEDS CLARIFICATION |
| NFR-004 | `filter` semantics shall be consistent with PySpark, Polars and ibis `filter`. | CONFIRMED (as rationale/proposal) / UNRESOLVED (which behaviours) | C8: "it's at odds with other DataFrame libraries" (PySpark, Polars, ibis; exceptions Modin, dask); contested C9 | unprioritised | none yet, see Q-017 | BLOCKED (CF-01) |

## 6. Constraints

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| CON-01 | Backward compatibility shall be kept. | CONFIRMED (term) / UNRESOLVED (meaning) | C0: "the main challenge is how to design the API ... keeping backward compatibility" | unprioritised | none yet, see Q-014 | NEEDS CLARIFICATION |
| CON-02 | A transition path shall exist for users of label-based `filter`. | CONFIRMED (stated as a need) | C12: "So the issue is how to make this transition."; C19: "how to create a transition path for those using `filter()` for filtering on labels" | unprioritised | none yet, see Q-010 | BLOCKED (CF-06) |

Tension recorded: CON-01 versus FR-011, FR-013, FR-015 (deprecation eventually removes behaviour). Budget, deadline, regulatory, deployment environment, supported Python/pandas versions: none identified, because the thread states none (Q-024).

## 7. Dependencies

| ID | Dependency | Status | Source |
|---|---|---|---|
| DEP-01 | `pd.col` / `pandas.col` being available | CONFIRMED (discussed) / UNRESOLVED (availability) | C5, C15, C21 |
| DEP-02 | `DataFrame.select` for the `axis=1` replacement; issue #55289 | CONFIRMED (as proposal) | C1 |
| DEP-03 | Outcome of issues #12401 and #26642 | CONFIRMED (referenced) / UNRESOLVED (outcome) | C17, C18 |
| DEP-04 | Decision by pandas core team (no named decider) | CONFIRMED (cc'd) / UNRESOLVED | C3; Q-002 |
| DEP-05 | Input from @jorisvandenbossche | CONFIRMED (pinged) / UNRESOLVED | C18 |
| DEP-06 | Existing `DataFrame.query` behaviour as the reference for FR-004 | CONFIRMED | C0, C20 |
| DEP-07 | Documentation updates | INFERRED | C3 |

## 8. Risks

Likelihood and impact are not estimable from the input and are not guessed.

| ID | Risk | Status | Source |
|---|---|---|---|
| RSK-01 | String conditions have edge cases (integer column labels, `@` syntax confusing linters, f-string results depending on string representation). | CONFIRMED (as stated risk) | C22 |
| RSK-02 | A whole-DataFrame callable largely duplicates `pipe`. | CONFIRMED (as stated risk) | C1, C3 |
| RSK-03 | A by-row callable is "effectively a transpose" and `x` as a Series "can only have one dtype". | CONFIRMED (as stated risk) | C3 |
| RSK-04 | Deprecating `query` disrupts existing code ("LOTS of code"). | CONFIRMED (as stated risk) | C6 |
| RSK-05 | Duplicating boolean indexing blurs separation of concerns and confuses newcomers. | CONFIRMED (as stated risk) | C14 (contested by C15) |
| RSK-06 | Not mixing label and value filtering makes the transition harder. | CONFIRMED (as stated risk) | C12 |
| RSK-07 | If `items` accepts list-likes, a list of labels and a list of booleans of the same length as `df` may be ambiguous. | INFERRED. Rationale: C1 proposes list-likes "same length as df" for the argument that already takes label lists; C0 names API compatibility as the main challenge. Needs maintainer confirmation. | C0, C1 |
| RSK-08 | The decision may stall: no named decision-maker, an invited participant has not answered. | INFERRED | C3, C18 |
| RSK-09 | `pd.col` may not be available when needed; if string conditions are dropped (CF-05) the concise forms depend on it. | INFERRED | C5, C21 |
| RSK-10 | Re-adding `DataFrame.select` reverses an earlier removal. | INFERRED (the removal is stated in C19; the risk is the agent's reading) | C19 |
| RSK-11 | Behaviour for null/NA values inside a condition is unspecified. EV-01 shows one comparable library discards such rows (EXTERNAL, needs confirmation). | INFERRED | EV-01 |
| RSK-12 | No usage data supports claims about which forms are most used or popular. | INFERRED | C2, C13, C15 |

## 9. Acceptance criteria

Drafted from the stated equivalences in the thread. Example data is illustrative (ASM-04). All are subject to resolution of the conflicts noted; none makes its requirement READY.

| ID | Req | Pass/fail test |
|---|---|---|
| AC-001 | FR-001 | Given `df` with `age = [10, 20, 30]`, `df.filter(df["age"] > 18)` returns a DataFrame equal in values, index and dtypes to `df[df["age"] > 18]`. |
| AC-002 | FR-002 | Given a Series condition whose index is a permutation of `df.index`, the selected rows equal those selected by label match. Behaviour for missing or duplicate labels: none yet, see Q-018. |
| AC-003 | FR-003 | Given a list of booleans of length `len(df)`, the result equals `df[mask]`. Behaviour for other lengths: none yet, see Q-018. |
| AC-004 | FR-004 | For `df.filter("age > 18")` and `df.query("age > 18")` on the same `df`, the results are equal. |
| AC-005 | FR-005 | `df.filter(f)` with a callable `f` returning a valid condition (per FR-006 or FR-007) returns the filtered DataFrame without raising a type error. |
| AC-006 | FR-006 | `df.filter(lambda d: d["age"] > 18)` equals `df[df["age"] > 18]`, and the object received by the lambda equals `df`. |
| AC-007 | FR-007 | The callable is invoked once per row with that row as a Series, and the result keeps exactly the rows for which it returns true. Exact equivalence to `apply(..., by_row=True)`: to be confirmed (Q-003). |
| AC-008 | FR-009 | `df.filter(pd.col("age") > 18)` equals `df[df["age"] > 18]`. |
| AC-009 | FR-011 | Calling `filter(like=...)` or `filter(regex=...)` emits a deprecation warning. Warning category and removal version: none yet, see Q-010. |
| AC-010 | FR-012 | The text of each warning from AC-009 and AC-011 names a replacement call. |
| AC-011 | FR-013 | Calling `filter(..., axis=1)` emits a deprecation warning. Category and removal version: see Q-010. |
| AC-012 | FR-017 | `df.query(df["age"] > 18)` equals `df.query("age > 18")` on the same `df`. |
| AC-013 | FR-018 | `df.filter(cond=df["age"] > 18)` equals `df[df["age"] > 18]`. |
| AC-014 | FR-019 | `df.filter(like="a", cond=df["age"] > 18)` raises an error. Error type: see Q-009. |
| AC-015 | FR-021 | Given an index named `foo`, `df.filter("foo.str.contains('xxx') and age > 18")` equals the same string passed to `df.query`. |
| AC-016 | FR-022 | Every existing `filter` call using `items`, `like`, `regex`, `axis` returns the same result and emits no new warning compared with the current release. |
| AC-017 | NFR-001 | For each surviving condition form, a single chained expression (e.g. `df.assign(...).filter(<form>)`) runs without any named intermediate DataFrame. |

No acceptance criterion yet (see the Q shown): FR-008 (Q-005), FR-010 (Q-006), FR-014 (Q-011), FR-015 (Q-008), FR-016 (Q-008), FR-020 (Q-009), NFR-002 (Q-015), NFR-003 (Q-016), NFR-004 (Q-017), CON-01 (Q-014), CON-02 (Q-010).

## 10. Traceability matrix

| Source | Requirement | Acceptance criterion |
|---|---|---|
| C0 | FR-001, FR-004, FR-005, FR-006, FR-015, NFR-001, CON-01 | AC-001, AC-004, AC-005, AC-006, none, AC-017, none |
| C1 | FR-002, FR-003, FR-005, FR-007, FR-010, FR-011, FR-013, FR-014, FR-020 | AC-002, AC-003, AC-005, AC-007, none, AC-009, AC-011, none, none |
| C3 | FR-007, FR-008, FR-015, FR-016 | AC-007, none, none, none |
| C5 | FR-006, FR-009 | AC-006, AC-008 |
| C6 | FR-017, FR-022 | AC-012, AC-016 |
| C8 | NFR-004 | none |
| C12 | FR-012, CON-02 | AC-010, none |
| C14 | FR-022 | AC-016 |
| C15 | FR-009, NFR-001 | AC-008, AC-017 |
| C19 | FR-021, CON-02 | AC-015, none |
| C20 | FR-004, FR-018, FR-019, FR-021 | AC-004, AC-013, AC-014, AC-015 |
| C21 | FR-009, NFR-003 | AC-008, none |
| C22 | NFR-003, RSK-01 | none |
| EV-01 | (no requirement; options only, see Section 12) | n/a |

## 11. Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-18 | Initial draft from T7 thread. EV-01 added (DeepWiki, pola-rs/polars). No ID withdrawn or renumbered. |

## 12. Effect of external evidence

**EV-01** (DeepWiki `ask_wiki_question`, `pola-rs/polars`, 2026-10-18; raw record in `ev_01.txt`). It changed the following, and nothing else:
- Added an EXTERNAL candidate option to Q-006 (predicate must be Boolean dtype; non-boolean values rejected) and to FR-010's source note.
- Added CF-08 (EV-01 versus one participant's lean toward truthiness in C1).
- Added candidate options to Q-005 and Q-018 (result must be the same length as the frame; nulls in the condition drop the row).
- Added RSK-11 (null handling unspecified).
- Gave NFR-004 a concrete comparison point for Q-017.
- It did not change any requirement status, did not create any requirement, and did not resolve any conflict.

No external evidence was consulted for the open questions about the maintainers' own decisions (who decides, deprecation timeline for `query`, naming, whether to extend `filter` or `query`, API signature, priorities, scope), because outside projects cannot answer these.
