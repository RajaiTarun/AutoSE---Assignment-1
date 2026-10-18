# Multi-Stop Walking Route Tool (100+ Boutiques, NYC) Requirements Specification
Version 0.1 (draft) | Date 2026-10-16 | Author: requirements-spec agent | Source documents: SRC-1 = `T1_hn_vague-short-business-need.md` (Ask HN post by klbarry, item 2359784); EV-01 (DeepWiki, see file 02)

Intake restatement: A person who will walk to 100+ fashion boutiques in NYC has a list of them and finds that most route software accepts at most 25 inputs. They want a way to get a route without working it out by hand.
Do-not-invoke check: the post asks "Does anyone know a way to...", which could mean "recommend an existing tool" rather than "build software". Recorded as Q-001; the skill is applied on the reading "a tool/software capability is needed". Input is very short, so most of this spec is UNRESOLVED by design.

## 1. Problem restatement and scope
- **In scope (CONFIRMED):** producing a route over a list of 100+ locations (boutiques in NYC) for one person travelling on foot, without manual ordering.
- **Out of scope:** none stated. No exclusions identified, because the input gives none.
- **Unknown scope:** whether this means building new software or choosing an existing tool (Q-001); input/output formats (Q-003, Q-005); optimisation goal (Q-004); single day or multi-day (Q-006); any hours/appointment limits (Q-007); platform (Q-008).

## 2. Glossary
| Term | Meaning |
|---|---|
| Location | A fashion boutique in NYC that the user will visit. Taken from "100+ fashion boutiques in NYC". |
| List | "a list ready put together" by the user. Content and format undefined, see Q-003. |
| Route software | Software producing a visiting order/route over locations. Unmeasured: what "route" optimises is undefined, see Q-004. |
| Route | Undefined: ordered list, turn-by-turn map, or both. See Q-005. |
| "by hand" | Manual ordering of the stops by the user. Taken literally. |
| "100+" | At least 100. Upper bound undefined, see Q-002. |
| "up to 25" | A stated limit of other products ("most route software"), not a limit of this system. |

## 3. Actors
| ID | Actor | Status | Source / rationale |
|---|---|---|---|
| ACT-01 | Walker/planner (the poster); visits the boutiques and owns the list | CONFIRMED | "I will be walking 100+ fashion boutiques ... I have a list" |
| ACT-02 | Boutique owners/staff at the visited locations (affected party) | INFERRED | Implied by visiting boutiques; their opening hours may constrain visits (Q-007) |
| ACT-03 | Operator/maintainer of the tool | UNRESOLVED | Not stated; depends on Q-001 (build vs use) |

## 4. Functional requirements
| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The system shall accept an input list of at least 100 locations in a single run. | CONFIRMED (lower bound only) | "100+ fashion boutiques"; "most route software only allows input of up to 25" | unprioritised | AC-001 | NEEDS CLARIFICATION (upper bound Q-002, format Q-003) |
| FR-002 | The system shall produce a visiting order covering every location in the input list. | INFERRED | Implied by "route software" and "route"; rationale: a route needs an order | unprioritised | AC-002 | NEEDS CLARIFICATION (Q-004, Q-005) |
| FR-003 | The system shall produce the visiting order without the user having to sequence stops manually. | CONFIRMED | "without figuring it all out by hand" | unprioritised | AC-003 | NEEDS CLARIFICATION (blocked by scope Q-001 for how it is delivered) |
| FR-004 | The system shall base the route on walking travel. | CONFIRMED | "I will be walking" | unprioritised | AC-004 | NEEDS CLARIFICATION (walking vs mixed with transit, Q-009) |
| FR-005 | The system shall minimise `<measure TBD>` over the route. | UNRESOLVED | No objective stated (candidate measures from EV-01: total duration, total distance; EXTERNAL) | unprioritised | none yet, see Q-004 | NEEDS CLARIFICATION |
| FR-006 | The system shall present the route in `<output form TBD>`. | UNRESOLVED | Not stated | unprioritised | none yet, see Q-005 | NEEDS CLARIFICATION |
| FR-007 | The system shall fix the start and end point of the route to `<TBD>`. | UNRESOLVED | Not stated; EV-01 shows routing engines take start/end locations (EXTERNAL) | unprioritised | none yet, see Q-010 | NEEDS CLARIFICATION |

No other functional requirements were added. Features that similar tools often have (time windows, saving routes, sharing) appear only as questions (Q-006, Q-007).

## 5. Non-functional requirements
| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | The system shall return a route for 100 locations within `<N seconds, TBD>`. | UNRESOLVED (SUGGESTED, unconfirmed: none proposed) | "fast"-type expectation not even stated; placeholder only | unprioritised | none yet, see Q-011 | NEEDS CLARIFICATION |
| NFR-002 | Cost, privacy, reliability, usability, portability, maintainability, compliance | none identified, because the input states nothing on them. Cost and platform are asked in Q-008 and Q-012. | - | - | - | - |

## 6. Constraints
| ID | Constraint | Status | Source |
|---|---|---|---|
| CON-01 | All locations are in New York City. | CONFIRMED | "fashion boutiques in NYC" |
| CON-02 | The traveller moves on foot. | CONFIRMED | "I will be walking" |
| CON-03 | Technology, budget, deadline, regulation | none identified, because not stated (see Q-008, Q-012) | - |

## 7. Dependencies
| ID | Dependency | Status | Source |
|---|---|---|---|
| DEP-01 | The user's existing list of locations | CONFIRMED | "I have a list ready put together" (format unknown, Q-003) |
| DEP-02 | Map/address data to locate and measure walking between boutiques | INFERRED | Needed to relate addresses to distances; provider unknown |
| DEP-03 | A routing/optimisation engine handling more than 25 stops | EXTERNAL (candidate) | EV-01: VROOM states no global hard limit on number of jobs. Not a decision; needs stakeholder confirmation (Q-001) |

## 8. Risks
| ID | Risk | Status | Likelihood / impact |
|---|---|---|---|
| RSK-01 | Chosen map/routing service silently applies a ~25-stop cap, repeating the user's problem | CONFIRMED as a fact about "most route software"; applicability to any chosen service INFERRED | Likelihood medium, impact high (estimate, unverified) |
| RSK-02 | Input list lacks usable addresses/coordinates, so routing cannot start | INFERRED | unknown |
| RSK-03 | Boutiques are closed at planned visit times, making the route unusable in practice | INFERRED | unknown (Q-007) |
| RSK-04 | Request misread: user wanted a tool recommendation, not a new system | INFERRED | unknown (Q-001) |

## 9. Acceptance criteria
| ID | Requirement | Pass/fail test |
|---|---|---|
| AC-001 | FR-001 | Given a list of 100 locations, the system accepts it in one run with no error; given 101, same. (Upper bound pending Q-002.) |
| AC-002 | FR-002 | For an input of N locations, the output contains each of the N exactly once; any omission or duplicate = fail. |
| AC-003 | FR-003 | A user can go from supplying the list to receiving an ordered result without entering or editing any stop order; any required manual reordering step = fail. |
| AC-004 | FR-004 | Distances/durations used are for pedestrian travel; the system's stated travel mode is "walking"; any other mode = fail. |

## 10. Traceability matrix
| Source | Requirement | Acceptance criterion |
|---|---|---|
| SRC-1 "100+ fashion boutiques", "up to 25" | FR-001 | AC-001 |
| SRC-1 "route software" (inferred) | FR-002 | AC-002 |
| SRC-1 "without figuring it all out by hand" | FR-003 | AC-003 |
| SRC-1 "I will be walking" | FR-004, CON-02 | AC-004 |
| SRC-1 "in NYC" | CON-01 | n/a (constraint) |
| SRC-1 "I have a list" | DEP-01 | n/a |
| EV-01 (candidate measures) | FR-005 (placeholder), DEP-03 | none yet (Q-004) |
| (none: gap) | FR-006, FR-007, NFR-001 | none yet (Q-005, Q-010, Q-011) |

## 11. Change log
| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-16 | Initial draft from SRC-1; EV-01 consulted; no stakeholder answers (non-interactive). |
