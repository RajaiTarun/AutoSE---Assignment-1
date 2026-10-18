# Outsourced iPhone App (project slug: outsourced-iphone-app) Requirements Specification

Version 0.1 (draft) | Date 2026-10-16 | Author: requirements-spec agent (non-interactive run)
Source documents: S1 = `T6_hn_outsourcing-budget-question.md` (Ask HN post by "sendos", the only source; quotes below are from it). No external evidence consulted.

Intake restatement: An individual wants a "moderate" iPhone app built by an outsourced developer or shop and is asking what that costs and who to hire. The app's purpose is never stated; only a few size and feature hints are given.
Do-not-invoke check: software-related, no complete spec exists, user is not asking for code. Proceed. Note: the post's main question ("how much does it cost?") is a pricing and vendor-sourcing question, not a requirement; it is recorded as out of scope and cannot be answered from this spec (see Q-005, Q-010).

## 1. Problem restatement and scope
- In scope (stated): an iPhone app with "up to five different views", "storing of some user data", "using the location framework", built by an outsourced party.
- Out of scope (INFERRED, confirm with author): quoting a price, recommending dev shops or marketplaces (post asks "do you guys know of good iPhone dev shops?"); this is advice-seeking, not a system requirement.
- Unknown scope: the app's purpose and core functionality, backend/server needs, design work, testing, App Store submission, post-launch maintenance, devices and OS versions, who owns the IP.

## 2. Glossary
| Term | Definition |
|---|---|
| View | Undefined, see Q-002 (screen? tab? modal?) |
| "moderate app" / "Nothing too complicated" | Undefined buzzword, see Q-001, Q-002. Only interpretation supported by the source: up to five views + stored user data + location use |
| "location framework" | Undefined, see Q-004 (the Apple location API is the likely meaning; not confirmed) |
| "good developers" / "bottom-of-the-line developers" | Undefined, see Q-009 |
| "several thousand dollars" | Undefined amount, see Q-005 |
| "user data" | Undefined, see Q-003 |

## 3. Actors
| ID | Actor | Status | Source |
|---|---|---|---|
| ACT-01 | Author / client (commissions and pays; "I'd like to outsource it") | CONFIRMED | S1: "if the cost is not too prohibitive, I'd like to outsource it" |
| ACT-02 | Outsourced developer or dev shop | CONFIRMED | S1: "good iPhone dev shops" |
| ACT-03 | End user of the app | INFERRED (an app implies users; who they are is not stated, see Q-001) | Rationale: any app has users |
| ACT-04 | Apple (platform / App Store gatekeeper) | INFERRED, needs confirmation (distribution channel not stated, see Q-007) | Rationale: iPhone apps are normally distributed via App Store |

## 4. Functional requirements
| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The app shall contain no more than five distinct views. | CONFIRMED (upper bound only) | S1: "up to five different views" | unprioritised | AC-001 (depends on Q-002) | NEEDS CLARIFICATION |
| FR-002 | The app shall store user data. (Which data, where, how long: UNRESOLVED) | CONFIRMED (existence only) | S1: "storing of some user data" | unprioritised | none yet, see Q-003 | NEEDS CLARIFICATION |
| FR-003 | The app shall use device location. (Purpose, accuracy, background use: UNRESOLVED) | CONFIRMED (existence only) | S1: "using the location framework" | unprioritised | none yet, see Q-004 | NEEDS CLARIFICATION |
| FR-004 | The app shall run on iPhone. (Models and OS versions: UNRESOLVED) | CONFIRMED (platform only) | S1: title, "iPhone app" | unprioritised | AC-002 (depends on Q-006) | NEEDS CLARIFICATION |

No other functional requirement is recorded. The app's core purpose is not stated, so no business functionality can be specified (Q-001). Features that "such apps usually have" (login, push notifications, backend sync, analytics) are deliberately not added.

## 5. Non-functional requirements
| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | The app shall obtain the user's permission before accessing device location. | INFERRED | Rationale: location is personal data and the platform generally gates it; not stated in S1 | unprioritised | none yet, see Q-004, Q-011 | NEEDS CLARIFICATION |
| NFR-002 | The app shall protect stored user data to a level of [UNRESOLVED: placeholder, "SUGGESTED, unconfirmed": defined once Q-003 names the data and its sensitivity]. | UNRESOLVED | none; gap found at workflow step 3 | unprioritised | none yet, see Q-003, Q-011 | NEEDS CLARIFICATION |
| NFR-003 | The app shall meet performance, usability and reliability targets of [UNRESOLVED]. | UNRESOLVED | none stated; "moderate", "nothing too complicated" are not measurable | unprioritised | none yet, see Q-001 | NEEDS CLARIFICATION |

## 6. Constraints
| ID | Statement | Status | Source | Readiness |
|---|---|---|---|---|
| CON-01 | The development budget shall not exceed [UNRESOLVED amount]. Source is ambiguous: "I don't mind paying up to several thousand dollars" vs. asking whether the cost is "$5,000, $10,000, $20,000, more?" | UNRESOLVED | S1 (both quotes); see conflict C-01 | BLOCKED |
| CON-02 | Development shall be done by an outsourced party, not the author. | CONFIRMED | S1: "I'd like to outsource it" | NEEDS CLARIFICATION (no AC yet) |
| CON-03 | The developers engaged shall not be "bottom-of-the-line"; the author accepts paying extra for quality. Measurable criterion [UNRESOLVED]. | CONFIRMED (intent) / UNRESOLVED (measure) | S1: "I don't want bottom-of-the-line developers. I don't mind paying extra for good developers" | NEEDS CLARIFICATION (see Q-009) |
| CON-04 | A developer or shop in the SF Bay Area is preferred; a non-local shop is acceptable. | CONFIRMED (preference, not a hard limit) | S1: "I'm in the SF Bay Area, so a good local dev shop would be great, even though I wouldn't mind a dev shop from elsewhere" | NEEDS CLARIFICATION (no AC yet) |
| CON-05 | No delivery deadline is stated. The author's effort estimates ("a month or more" for the author, "maybe two weeks" for an experienced coder) are estimates, not deadlines. | CONFIRMED (absence of deadline); see Q-010 | S1 | N/A (not a requirement) |

## 7. Dependencies
| ID | Dependency | Status | Source |
|---|---|---|---|
| DEP-01 | A decision by the author on budget (Q-005) and scope (Q-001, Q-002) before any vendor can quote | INFERRED | Rationale: vendors cannot price an unspecified app |
| DEP-02 | Apple's iOS location and data-storage facilities | CONFIRMED (location: "location framework"); storage mechanism not stated | S1 |
| DEP-03 | A marketplace or vendor-selection channel (elance.com, odesk.com mentioned) | CONFIRMED as mentioned by author, not as a requirement | S1: "look on elance.com, odesk.com" |

## 8. Risks
| ID | Risk | Status | Likelihood / impact |
|---|---|---|---|
| RSK-01 | Budget (stated "several thousand") is too low for a quality developer; author's own ranges go up to "$20,000, more" | INFERRED | not estimable from source |
| RSK-02 | Underspecified scope leads to disputed quotes or scope creep with an outsourced party | INFERRED | likely / high |
| RSK-03 | Author is unable to judge or vet developer quality ("not sure how to use them to find a good developer") | CONFIRMED (author's own statement) | not estimable |
| RSK-04 | Location and user data raise privacy obligations not yet identified | INFERRED | unknown |
| RSK-05 | Author's own effort estimate may be wrong in either direction | INFERRED | unknown |

## 9. Acceptance criteria
| ID | Requirement | Pass/fail test | Caveat |
|---|---|---|---|
| AC-001 | FR-001 | Pass if a count of distinct views (as defined by Q-002) in the delivered app is five or fewer. | "View" undefined |
| AC-002 | FR-004 | Pass if the app installs and launches on [UNRESOLVED: device models / iOS versions, Q-006]. | placeholder |
All other requirements: none yet, see the referenced Q-nnn.

## 10. Traceability matrix
| Source (S1 quote) | Requirement | Acceptance criterion |
|---|---|---|
| "up to five different views" | FR-001 | AC-001 |
| "storing of some user data" | FR-002 | none yet (Q-003) |
| "using the location framework" | FR-003 | none yet (Q-004) |
| "iPhone app" (title) | FR-004 | AC-002 |
| (no source; inference, location is personal data) | NFR-001 | none yet |
| (no source; gap) | NFR-002, NFR-003 | none yet |
| "I'd like to outsource it" | CON-02 | none yet |
| "up to several thousand dollars" / "$5,000, $10,000, $20,000, more" | CON-01 | none yet (blocked) |
| "I don't want bottom-of-the-line developers" | CON-03 | none yet (Q-009) |
| "SF Bay Area ... local dev shop" | CON-04 | none yet |

## 11. Change log
| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-16 | Initial draft from S1. No answers or external evidence received. |
