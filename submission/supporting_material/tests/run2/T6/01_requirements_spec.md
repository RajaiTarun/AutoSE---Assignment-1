# Outsourced iPhone Location App ("sendos app") Requirements Specification

- Version: 0.1 (draft, non-interactive run)
- Date: 2026-10-18
- Author: agent (requirements-spec skill)
- Source documents: `T6_hn_outsourcing-budget-question.md` (Ask HN post, https://news.ycombinator.com/item?id=1251155, author sendos, "Original post, unedited"). Cited below as `L<n>` = line number in that file.
- Input handling: **mixed input** (Section 1 "Borderline inputs"). A system is described (L9) and non-spec questions are asked (L13, L21). The described system is specified; the other questions are listed as out of scope and are not answered. This is not a decline-set case, because a system is described, although very thinly.

## 1. Problem restatement and scope

**Restatement (2 lines).** The author wants an iPhone app built and would outsource it if the cost is not prohibitive. The post describes the app only by size and capabilities (up to five views, stores some user data, uses the location framework), not by purpose, and mostly asks how much it would cost and who could build it.

**Sources available.** Only the post itself. No stakeholder is reachable (the post is a fixed original, unedited), so this is a non-interactive run and all questions stay open.

**Stated context (not requirements):**
- L11: "I think it might take me a month or more to code, but it should take a more experienced iPhone coder maybe two weeks." CONFIRMED as the author's own estimate; not verified (see RSK-02).
- L19: "if I can't make that money back from the app, I would prefer to have wasted money than 1 or 2 months of my life." CONFIRMED as the author's stated attitude; whether the app is commercial is INFERRED and asked in Q-010.

**In scope**
- Requirements for the single iPhone app described at L9, to the extent the post states them.

**Out of scope (listed, not answered)**
- "Am I looking at $5,000, $10,000, $20,000, more?" (L13): a price-estimate question.
- "do you guys know of good iPhone dev shops?" (L21): a vendor-recommendation question.
- "I take it that the standard procedure is to look on elance.com, odesk.com ... not sure how to use them to find a good developer" (L21): a vendor-sourcing/process question.
- Preference for a local SF Bay Area dev shop (L23): a vendor-selection preference, not a system property.
- Selecting developers of "good" quality rather than "bottom-of-the-line" (L17): vendor-selection criterion.
- Find-an-existing-product: not raised in the post; not applicable.

**Unknown scope** (see Section 2 and `02_assumptions_and_open_questions.md`)
- What the app does (Q-001), what "user data" is (Q-003), what the location is used for (Q-004), target devices and OS (Q-005), whether a back end or accounts exist (Q-009).

## 2. Glossary

| Term | Definition |
|---|---|
| iPhone app | Stated (L1, L11). Taken to mean an app for iPhone; native vs web is ASM-01 (not confirmed). |
| moderate app | Undefined, see Q-007. Buzzword. |
| "Nothing too complicated" | Undefined, see Q-007. Buzzword. |
| view | Undefined, see Q-002. The post says "five different views" without defining a view (screen? tab? mode?). |
| user data | Undefined, see Q-003. |
| location framework | Stated (L9). Interpreted as the iPhone platform's own location API: INFERRED (ASM-02), not confirmed. What it is used for is Q-004. |
| several thousand dollars | Undefined amount, see Q-006. Buzzword. |
| good developers | Undefined, see Q-011 (and out of scope as a vendor criterion). |

## 3. Actors

| ID | Actor | Status | Source / rationale |
|---|---|---|---|
| ACT-01 | Author (sendos): owner of the idea, would pay for and (if not outsourced) code the app | CONFIRMED | L5 "an app that I would like to code up ... I'd like to outsource it"; L19 "I don't mind paying" |
| ACT-02 | Outsourced developer or dev shop that would build the app | CONFIRMED (as prospective) | L5 "outsource it"; L21 "iPhone dev shops" |
| ACT-03 | End users of the app, who generate the "user data" | INFERRED | Rationale: "storing of some user data" (L9) implies someone whose data it is. Who they are is UNRESOLVED (Q-001). |
| ACT-04 | Platform owner / app distribution channel (Apple) | INFERRED | Rationale: an iPhone app is typically distributed and policy-governed by the platform owner. Not stated; needs confirmation (Q-009). |
| ACT-05 | Operators, regulators, payers other than the author | UNRESOLVED | None identified in the post. Q-008 asks about regulatory context. |

## 4. Functional requirements

Partly-stated items are split per Section 1 of the skill. Priority is "unprioritised" throughout; the post gives none.

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The app shall contain no more than five distinct views. | CONFIRMED (illustrative upper bound: "say, up to five") / UNRESOLVED (meaning of "view"; whether five is a cap or an example) | L9 "Assume a moderate app, with, say, up to five different views" | unprioritised | AC-001 (provisional) | NEEDS CLARIFICATION (measurable once Q-002 answered) |
| FR-002 | The app shall store some user data. | CONFIRMED (term "storing of some user data") / UNRESOLVED (which data, where stored, how long, who can access) | L9 "storing of some user data" | unprioritised | none yet, see Q-003 | BLOCKED (meaning depends on open scope questions Q-003 and Q-001: on-device vs server changes what the requirement means) |
| FR-003 | The app shall use the platform's location information. | CONFIRMED (that the "location framework" is used) / UNRESOLVED (purpose, accuracy, foreground vs background, frequency) | L9 "using the location framework" | unprioritised | none yet, see Q-004 | BLOCKED (purpose unknown, Q-001 and Q-004 change what is built) |
| FR-004 | The app shall provide its core user-facing function: [UNDEFINED]. | UNRESOLVED (placeholder only; existence of an app is CONFIRMED, its function is not stated) | L5 "There is an app that I would like to code up" (existence only) | unprioritised | none yet, see Q-001 | BLOCKED (open scope question Q-001) |

## 5. Non-functional requirements

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | The app shall protect the user's location data and stored user data to a standard [UNDEFINED, measurable placeholder]. | INFERRED | Rationale: the app handles location and stored user data (L9); privacy/security posture is not stated. Candidate only, not stated by the author. | unprioritised | none yet, see Q-008 | NEEDS CLARIFICATION |

Other NFR categories (performance, reliability, usability, portability, maintainability, compliance): none identified, because the post states no quality attribute of the app. ("Good developers", L17, is a vendor criterion, not an app attribute.) Each is open under Q-007 (what "moderate" and "nothing too complicated" imply) and Q-008.

## 6. Constraints

Readiness applies to constraints (Section 6 of the skill).

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| CON-01 | The app shall target the iPhone platform. | CONFIRMED (platform: "iPhone") / UNRESOLVED (device models, OS versions, iPad) | L1, L11 "iPhone" | unprioritised | AC-002 (provisional) | NEEDS CLARIFICATION (Q-005) |
| CON-02 | The cost to the author of building the app shall not exceed "several thousand dollars". | CONFIRMED (term: "up to several thousand dollars") / UNRESOLVED (amount; hard cap or soft; scope of cost) | L19 "I don't mind paying up to several thousand dollars" | unprioritised | none yet, see Q-006 | NEEDS CLARIFICATION |
| CON-03 | Development is intended to be performed by external (outsourced) developers, conditional on cost not being prohibitive. | CONFIRMED (as stated intention, conditional) | L5 "if the cost is not too prohibitive, I'd like to outsource it" | unprioritised | N/A for the app itself; project-level, see Q-006 | NEEDS CLARIFICATION (condition "not too prohibitive" is undefined, Q-006) |

Note on budget wording: the author says "up to several thousand dollars" (L19) while asking whether the price is "$5,000, $10,000, $20,000, more?" (L13). The second is a question, not a claim, so this is recorded as an ambiguity in Q-006, not as a CF entry. No conflicts were identified (see `02`, conflicts table).

## 7. Dependencies

| ID | Dependency | Status | Source / rationale |
|---|---|---|---|
| DEP-01 | Availability of the iPhone platform's location capability on the target devices | CONFIRMED (mention) / UNRESOLVED (details) | L9 "using the location framework" |
| DEP-02 | End-user permission to access location | INFERRED | Rationale: platform-controlled location access normally depends on user consent. Not stated. |
| DEP-03 | Author's answers on app purpose, data, location use, budget (Q-001 to Q-008) | UNRESOLVED | Stakeholder decisions not available in this run. |
| DEP-04 | App distribution channel and developer accounts | INFERRED | Rationale: an iPhone app must be delivered to devices somehow. Not stated (Q-009). |

## 8. Risks

| ID | Risk | Status | Likelihood / impact |
|---|---|---|---|
| RSK-01 | The real cost of the intended app exceeds "several thousand dollars", so the app is not built or scope is cut. | INFERRED | Not estimable from the input; price is out of scope. |
| RSK-02 | The author's effort estimate (one month for self, two weeks for an expert, L11) is wrong because the app is under-specified. | INFERRED | Not estimable. |
| RSK-03 | Location and stored user data raise privacy or compliance obligations nobody has assessed. | INFERRED | Not estimable until Q-001, Q-003, Q-004, Q-008 are answered. |
| RSK-04 | Because the app's purpose is unknown, any developer given this spec would have to invent requirements. | INFERRED | High likelihood, high impact (this is why the spec is not implementation-ready). |

## 9. Acceptance criteria

| ID | Requirement | Pass/fail test | Note |
|---|---|---|---|
| AC-001 | FR-001 | Pass if the agreed inventory of views (definition of "view" per Q-002) contains 5 or fewer entries and the built app exposes no view outside that inventory. Fail otherwise. | Provisional until Q-002 is answered. |
| AC-002 | CON-01 | Pass if the app installs and launches on each device model and OS version listed in the agreed support matrix (Q-005). Fail otherwise. | Provisional until Q-005 is answered. |

All other items: "none yet" with the question that blocks them (FR-002: Q-003; FR-003: Q-004; FR-004: Q-001; NFR-001: Q-008; CON-02/CON-03: Q-006).

## 10. Traceability matrix

| Source | Requirement | Acceptance criterion |
|---|---|---|
| L9 "up to five different views" | FR-001 | AC-001 (provisional) |
| L9 "storing of some user data" | FR-002 | none yet (Q-003) |
| L9 "using the location framework" | FR-003 | none yet (Q-004) |
| L5 "There is an app" | FR-004 (placeholder) | none yet (Q-001) |
| L9 + agent inference (privacy of location/user data) | NFR-001 (INFERRED) | none yet (Q-008) |
| L1, L11 "iPhone" | CON-01 | AC-002 (provisional) |
| L19 "up to several thousand dollars" | CON-02 | none yet (Q-006) |
| L5 "I'd like to outsource it" | CON-03 | none yet (Q-006) |

## 11. Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-18 | Initial draft from the Ask HN post. No answers received. |

## 12. Effect of external evidence

None. No EV entries exist. No external evidence consulted, because the open questions concern the author's own decisions and business (app purpose, budget, data policy, device targets) or are unknown because the app's category is not stated, so no comparable public project can be named without guessing. See `03_validation_summary.md`.
