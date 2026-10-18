# Outsourced iPhone App (HN "Ask HN" post) Requirements Specification

Version 0.1 | Date 2026-10-18 | Author: agent (requirements-spec skill) | Source: `tests/inputs/T6_hn_outsourcing-budget-question.md` (Ask HN post by "sendos", https://news.ycombinator.com/item?id=1251155). Session mode: non-interactive. Status: not implementation-ready.

Legend: CONFIRMED = stated in source (quoted). INFERRED = agent inference. EXTERNAL = outside evidence (none in this spec). UNRESOLVED = open.

## 1. Problem restatement and scope

**Restatement.** The author wants an iPhone app ("moderate": up to five views, some stored user data, uses the location framework) built by an outsourced developer if the cost is not prohibitive. The post is mainly a request for a price estimate and for dev-shop recommendations; the app's actual purpose is never stated.

**Input classification (Section 1 borderline rules).** The input is a *mixed input*: a system is described (thinly) and non-spec questions are asked. Per the skill, the described system is specified below (mostly UNRESOLVED) and the other questions are listed as out of scope and not answered. It was not treated as a decline-set input because a platform, a size bound, a data need and a platform-capability need are stated.

**In scope (described system).** An iPhone app with up to five views, user-data storage and use of location capability.

**Out of scope (not answered here; recorded, not addressed).**
- "Am I looking at $5,000, $10,000, $20,000, more?" (cost estimate for outsourcing).
- "do you guys know of good iPhone dev shops?" (vendor recommendations).
- "how to use [elance.com, odesk.com] to find a good developer" (procurement how-to).
- Developer-selection criteria: "I don't want bottom-of-the-line developers" and the SF Bay Area / local-shop preference (vendor selection, not software behaviour).
- Finding an existing product that does this: not applicable (no product need is stated).

**Unknown scope.** What the app does, who its end users are, what the five views contain, what data is stored and where, what the location data is used for, which iPhone/iOS versions, how it is distributed, whether it earns revenue, deadline, quality attributes. See Q-001 to Q-010.

## 2. Glossary

| Term | Status | Note |
|---|---|---|
| iPhone app | CONFIRMED (term) / UNRESOLVED (meaning) | Source: "outsource iPhone app development". Native vs web, device models, OS versions, iPad: undefined, see Q-005, ASM-01. |
| moderate app / "Nothing too complicated" | CONFIRMED (term) / UNRESOLVED (meaning) | Source: "Assume a moderate app, with, say, up to five different views...". Only partly defined by the three listed features. Undefined as a buzzword, see Q-001, Q-002. |
| view | Undefined, see Q-002 | Screen? Navigation state? Not defined in source. |
| user data / "storing of some user data" | CONFIRMED (term) / UNRESOLVED (meaning) | Which data, on device or server, sensitivity: see Q-003. |
| location framework | CONFIRMED (term) / UNRESOLVED (meaning) | Likely the platform's location services (see ASM-02, INFERRED); purpose undefined, see Q-004. |
| outsource | CONFIRMED (term) | "I'd like to outsource it" if "the cost is not too prohibitive". |
| prohibitive / "several thousand dollars" | Undefined, see Q-007 | No number given. |
| good developers / bottom-of-the-line | Undefined (out of scope, vendor selection) | Recorded only because it feeds CF-01. |

## 3. Actors

| ID | Actor | Status | Source / rationale |
|---|---|---|---|
| ACT-01 | App owner / requester / payer ("sendos", the author) | CONFIRMED | "There is an app that I would like to code up"; "I don't mind paying up to several thousand dollars". |
| ACT-02 | Outsourced developer or dev shop | CONFIRMED (as conditional intent) | "I'd like to outsource it" (if cost acceptable). Identity unknown. |
| ACT-03 | End users of the app | INFERRED | "storing of some user data" implies users exist; who they are is unstated, see Q-001. |

No regulator, operator or platform-owner actor is stated; see RSK-01 and Q-006, Q-009.

## 4. Functional requirements

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The app shall consist of no more than five distinct views. | CONFIRMED (upper bound, approximate: "say, up to five") / UNRESOLVED (definition of "view"; content of views) | "up to five different views" | unprioritised | AC-001 (placeholder, needs Q-002) | NEEDS CLARIFICATION (lacks definition of "view"; see tie-break) |
| FR-002 | The app shall store some user data. | CONFIRMED (term) / UNRESOLVED (meaning: which data, where, how long) | "storing of some user data" | unprioritised | none yet, see Q-003 | BLOCKED (what data is stored decides what is built and the privacy posture; Q-003, Q-009) |
| FR-003 | The app shall use the location framework. | CONFIRMED (term) / UNRESOLVED (meaning: for what, precision, foreground/background) | "using the location framework" | unprioritised | none yet, see Q-004 | BLOCKED (purpose of use decides what is built; Q-004, Q-001) |

No other functional requirement is recorded: the app's core function is not stated (Q-001). The agent has not added features that "apps like this usually have".

## 5. Non-functional requirements

None identified, because the source states no quality attribute (performance, security, privacy, reliability, usability, portability, maintainability, compliance). The phrase "good developers" concerns vendor quality, not product quality. Candidate NFR topics are open in Q-009. Privacy relevance of stored user data plus location is recorded as RSK-01 (INFERRED), not as a requirement.

## 6. Constraints

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| CON-01 | The app shall target the iPhone platform. | CONFIRMED (platform) / UNRESOLVED (device models, OS versions, iPad support) | "iPhone app development" | unprioritised | AC-002 (placeholder, needs Q-005) | NEEDS CLARIFICATION (lacks versions/devices; behaviour otherwise understood) |
| CON-02 | Development shall be performed by an external party (outsourced), conditional on the cost not being prohibitive. | CONFIRMED (as stated, conditional) / UNRESOLVED (threshold of "prohibitive") | "if the cost is not too prohibitive, I'd like to outsource it" | unprioritised | none yet, see Q-007 | BLOCKED (depends on the budget threshold in CON-03, which is in CF-01) |
| CON-03 | Total outsourcing cost shall not exceed a budget ceiling, stated only as "up to several thousand dollars". | CONFIRMED (term) / UNRESOLVED (amount) | "I don't mind paying up to several thousand dollars" | unprioritised | none yet, see Q-007 | BLOCKED (CF-01) |

No deadline is stated ("it might take me a month or more to code, but ... maybe two weeks" is an effort estimate, not a deadline). Time constraint: none identified, see Q-008.

## 7. Dependencies

| ID | Dependency | Status | Rationale |
|---|---|---|---|
| DEP-01 | Platform location services available on the target device | INFERRED | Follows from "location framework" (ASM-02). |
| DEP-02 | Author's decisions on Q-001 to Q-010 before any design or quote | INFERRED | Without them an outside developer cannot price or build reliably. |
| DEP-03 | Availability of a suitable external developer meeting the budget | INFERRED | Follows from CON-02; selection itself is out of scope. |

## 8. Risks

| ID | Risk | Status | Likelihood / impact |
|---|---|---|---|
| RSK-01 | Stored user data plus location data may be personal data, bringing privacy, policy or legal obligations that are not yet identified. | INFERRED | Unknown / potentially high. Not verified; no legal claim made. |
| RSK-02 | Budget expectation ("several thousand") may not match the candidate range the author himself lists ($5,000 to $20,000 and "more"), see CF-01. | INFERRED | Unknown / high for CON-02. |
| RSK-03 | Outsourcing against an underspecified brief may cause mismatched deliverables or cost overrun. | INFERRED | Unknown / medium-high. |
| RSK-04 | The author's own effort estimate ("a month or more" himself, "maybe two weeks" for an expert) is unverified and relies on an undefined "moderate" size. | INFERRED | Unknown / medium. |

## 9. Acceptance criteria

| ID | Requirement | Pass/fail test | Status |
|---|---|---|---|
| AC-001 | FR-001 | Pass if, using the agreed definition of "view" (Q-002), the delivered app contains at most 5 distinct views. Fail otherwise. Placeholder: definition pending. | placeholder |
| AC-002 | CON-01 | Pass if the app installs and launches on each iPhone model / OS version in the list agreed under Q-005. Fail otherwise. Placeholder: list pending. | placeholder |

FR-002, FR-003, CON-02, CON-03: no acceptance criterion yet, see Q-003, Q-004, Q-007. No stub criteria were invented.

## 10. Traceability matrix

| Source quote | Requirement / item | Acceptance criterion |
|---|---|---|
| "up to five different views" | FR-001 | AC-001 (placeholder) |
| "storing of some user data" | FR-002 | none yet (Q-003) |
| "using the location framework" | FR-003 | none yet (Q-004) |
| "iPhone app development" | CON-01 | AC-002 (placeholder) |
| "if the cost is not too prohibitive, I'd like to outsource it" | CON-02, ACT-02 | none yet (Q-007) |
| "I don't mind paying up to several thousand dollars" | CON-03, CF-01 | none yet (Q-007) |
| "I don't mind paying extra for good developers" ; "$5,000, $10,000, $20,000, more?" | CF-01 (potential), RSK-02 | n/a |
| "I would prefer to have wasted money than 1 or 2 months of my life" | context for CON-03 (willingness to spend); also implies a revenue hope, see Q-006 | n/a |
| "I'd like to code up" / "There is an app" | ACT-01, Q-001 | n/a |

## 11. Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-18 | Initial draft from the single source document. No stakeholder answers (non-interactive). |

## 12. Effect of external evidence

None. No external evidence consulted, because the open questions concern the stakeholder's own decisions (budget, scope, platform choices) and the app's category is unknown, so no comparable public project could be named without guessing. No EV entries exist. No DeepWiki call was made.
