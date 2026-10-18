# Long-Running Background Job Website Requirements Specification
Version 0.1 (draft) | Date 2026-10-16 | Author: requirements-spec agent | Sources: SRC-1 = `T3_hn_technical-system-ask.md` (Ask HN post, ryeguy_24); EV-01 (DeepWiki, celery/celery)

Status legend: CONFIRMED / INFERRED / EXTERNAL / UNRESOLVED. Readiness: READY / NEEDS CLARIFICATION / BLOCKED.

## 1. Problem restatement and scope
**Restatement.** The author is building a simple website where a user uploads data and the backend then starts a long data computation/analytics process. They ask whether Flask + Celery + RabbitMQ is a decent approach and for alternatives.
Note on intake: the post's literal question is a design/architecture question, which is outside this skill. Only the underlying system (website + upload + long job) is specified here. The stack question is recorded as CON-01/Q-001 and not answered.

- **In scope (stated):** website; user data upload; backend-started long process doing computation and analytics.
- **Out of scope:** choosing or reviewing an architecture/technology; any implementation.
- **Unknown scope:** everything else (users, accounts, result delivery, data types, scale, hosting). See section 2 and file 02.

## 2. Glossary
| Term | Definition |
|---|---|
| Job | The long backend process kicked off after an upload (SRC-1: "a long process"). |
| "simple" | Undefined, see Q-002. |
| "long" | Undefined (seconds? hours?), see Q-003. |
| "data computation and analytics" | Undefined: type of computation unknown, see Q-004. |
| "decent" / "better" approach | Undefined evaluation criteria, see Q-001. |
| "data" (uploaded) | Format, size, sensitivity unknown, see Q-005. |

## 3. Actors
| ID | Actor | Status | Source |
|---|---|---|---|
| ACT-01 | End user who uploads data | CONFIRMED | SRC-1: "the user uploads data" |
| ACT-02 | Builder/owner of the website (the poster) | CONFIRMED | SRC-1: "I am building a simple website" |
| ACT-03 | Operator/maintainer who runs the system | INFERRED | Rationale: any long-running backend needs someone to run it; not stated. |
| ACT-04 | Data owners / regulators affected by uploaded data | UNRESOLVED | No data sensitivity stated, see Q-006. |

## 4. Functional requirements
| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The system shall provide a website accessible to users. | CONFIRMED | "a simple website" | unprioritised | AC-001 | NEEDS CLARIFICATION (users/access undefined, Q-007) |
| FR-002 | The system shall allow a user to upload data. | CONFIRMED | "the user uploads data" | unprioritised | AC-002 (placeholder) | NEEDS CLARIFICATION (format/size, Q-005) |
| FR-003 | The system shall start a backend process after a user uploads data. | CONFIRMED | "the backend kicks off a long process" | unprioritised | AC-003 | NEEDS CLARIFICATION (trigger details, Q-008) |
| FR-004 | The system shall perform data computation and analytics on the uploaded data in that process. | CONFIRMED | "(data computation and analytics)" | unprioritised | none yet, see Q-004 | NEEDS CLARIFICATION |
| FR-005 | The system shall show the user the status of their job. | INFERRED | Rationale: a user waiting on a long job usually needs feedback; not stated. | unprioritised | AC-005 (placeholder) | NEEDS CLARIFICATION (Q-009) |
| FR-006 | The system shall make the job's result available to the user. | INFERRED | Rationale: a computation with no output has no purpose; delivery mechanism not stated. | unprioritised | none yet, see Q-009 | NEEDS CLARIFICATION |
| FR-007 | The system shall report to the user when a job fails. | EXTERNAL / INFERRED | EV-01 (Celery exposes FAILURE state); no stakeholder statement. | unprioritised | none yet, see Q-010 | NEEDS CLARIFICATION |

## 5. Non-functional requirements
| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | A job shall be able to run for up to [TBD, see Q-003] without the user's browser connection being required to stay open. | UNRESOLVED | "long process" (duration undefined) | unprioritised | none yet, Q-003 | NEEDS CLARIFICATION |
| NFR-002 | The system shall be "simple" per a measurable definition [TBD]. | UNRESOLVED | "simple website" | unprioritised | none yet, Q-002 | NEEDS CLARIFICATION |
| NFR-003 | The system shall support [TBD] concurrent users/jobs. | UNRESOLVED | not stated | unprioritised | none yet, Q-011 | NEEDS CLARIFICATION |
| NFR-004 | Uploaded data shall be protected per [TBD privacy/security level]. | UNRESOLVED | not stated | unprioritised | none yet, Q-006 | NEEDS CLARIFICATION |
| NFR-005 | A queued/running job shall not be lost on [TBD failure scenarios]. SUGGESTED, unconfirmed. | INFERRED | Rationale: long jobs make restarts likely. | unprioritised | none yet, Q-010 | NEEDS CLARIFICATION |

Categories with nothing identified: usability, portability, maintainability, compliance. None identified because the input says nothing about them (not because they do not apply).

## 6. Constraints
| ID | Constraint | Status | Source |
|---|---|---|---|
| CON-01 | Technology: Python Flask, Celery, RabbitMQ | UNRESOLVED (stated as "preliminary approach", i.e. a proposal, not clearly a fixed constraint) | "My preliminary approach is to use Python Flask with Celery and RabbitMQ" |
| CON-02 | Technology choices are open to alternatives | CONFIRMED | "Can anyone else recommend alternative/better approaches?" |
| CON-03 | Budget, deadline, hosting environment, law | UNRESOLVED: none stated, Q-012 | n/a |

CON-01 and CON-02 are in tension only if CON-01 is meant as fixed; see Q-001.

## 7. Dependencies
| ID | Dependency | Status | Source |
|---|---|---|---|
| DEP-01 | Flask, Celery, RabbitMQ (only if CON-01 is adopted) | UNRESOLVED | SRC-1 |
| DEP-02 | A store for job state/results (Celery uses a "result backend") | EXTERNAL | EV-01; needs stakeholder confirmation and only relevant if CON-01 adopted |

## 8. Risks
| ID | Risk | Status | Likelihood/impact |
|---|---|---|---|
| RSK-01 | Long jobs run inside a web request time out or block the site | INFERRED | not estimable |
| RSK-02 | Jobs lost or stuck on worker/broker failure | INFERRED (EV-01 shows retries exist but are configurable) | not estimable |
| RSK-03 | Untrusted uploaded data (malicious or oversized files) | INFERRED | not estimable |
| RSK-04 | Stack chosen before requirements are known | INFERRED | not estimable |

## 9. Acceptance criteria
| ID | Req | Pass/fail test | Note |
|---|---|---|---|
| AC-001 | FR-001 | Given the site is deployed, an ACT-01 user can load its page in a browser. | Placeholder; access model unknown |
| AC-002 | FR-002 | Given a file of an accepted format <= [TBD size], upload completes and the system confirms receipt. | Format/size TBD (Q-005) |
| AC-003 | FR-003 | After a successful upload, a job record exists and processing begins within [TBD seconds]. | Threshold TBD |
| AC-005 | FR-005 | After upload, the user can see whether their job is queued, running, finished or failed. | States are INFERRED; confirm Q-009 |

AC for FR-004, FR-006, FR-007 and NFR-001..005: none yet (see linked Q).

## 10. Traceability matrix
| Source | Requirement | AC |
|---|---|---|
| SRC-1 "simple website" | FR-001, NFR-002 | AC-001 |
| SRC-1 "user uploads data" | FR-002, ACT-01 | AC-002 |
| SRC-1 "backend kicks off a long process" | FR-003, NFR-001 | AC-003 |
| SRC-1 "(data computation and analytics)" | FR-004 | none yet |
| SRC-1 "preliminary approach ... Flask, Celery, RabbitMQ" | CON-01, DEP-01 | n/a |
| SRC-1 "recommend alternative/better approaches" | CON-02 | n/a |
| EV-01 | FR-007 (candidate), DEP-02, RSK-02 | none yet |
| Agent inference | ACT-03, FR-005, FR-006, NFR-005, RSK-01, RSK-03, RSK-04 | AC-005 only |

## 11. Change log
| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-16 | Initial draft. EV-01 added; it changed only: FR-007 candidate, DEP-02, RSK-02 wording and Q-010. |
