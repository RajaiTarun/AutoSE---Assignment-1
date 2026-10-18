# Assumptions, Open Questions, Conflicts, Evidence

## Assumptions
| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | The post describes a single system for one author/team, not a multi-tenant product. | Post is written in first person about one site. | Scale/security needs far higher. | Answer to Q-007, Q-011 |
| ASM-02 | The Flask/Celery/RabbitMQ stack is a proposal, not a mandate. | Author asks for alternatives. | Spec would wrongly reopen a fixed decision. | Answer to Q-001 |
| ASM-03 | "Analytics" means batch processing of the uploaded data, not interactive queries. | Phrase "long process". | FR-004/NFR-001 wrong. | Answer to Q-004 |

## Open questions
| ID | Question | Why it matters | Blocks | Who can answer | Suggested default (NOT applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | Is Flask+Celery+RabbitMQ a fixed constraint, or is the stack open? By what criteria is an approach "decent"/"better" (cost, simplicity, scale, team skill)? | Decides if CON-01 is a constraint. | CON-01, DEP-01 | Poster (ACT-02) | Treat as open; stack is a design-stage question | Open |
| Q-002 | What does "simple website" mean (number of pages, no login, single user type)? Options: single upload page / multi-page with accounts / other | Buzzword; scope | NFR-002, FR-001 | ACT-02 | single page, no accounts | Open |
| Q-003 | How long is "long" (seconds, minutes, hours, days)? | Drives architecture and NFR-001 | NFR-001, AC-003 | ACT-02 | none | Open |
| Q-004 | What computation/analytics is performed, on what data? Fixed or user-chosen? | Core function undefined | FR-004 | ACT-02 | none | Open |
| Q-005 | Upload formats, max size, number of files? | Testability of upload | FR-002, AC-002 | ACT-02 | none | Open |
| Q-006 | Is the data sensitive (personal, commercial, regulated)? Retention and deletion expectations? | Security/privacy posture | NFR-004, ACT-04 | ACT-02, data owners | none | Open |
| Q-007 | Who are the users and must they log in? Public or internal? | Scope and security | FR-001, ASM-01 | ACT-02 | none | Open |
| Q-008 | Is the job started automatically on upload or by user action? One job per upload? | Behaviour of FR-003 | FR-003 | ACT-02 | automatic on upload | Open |
| Q-009 | How does the user learn status and receive results (page polling, email, download)? | Defines FR-005/006 | FR-005, FR-006 | ACT-02 | none | Open |
| Q-010 | What should happen if a job fails or the system restarts (retry, notify, discard)? | Reliability | FR-007, NFR-005 | ACT-02 | notify user | Open |
| Q-011 | Expected concurrent users/jobs and upload volume? | Scale | NFR-003 | ACT-02 | none | Open |
| Q-012 | Budget, deadline, hosting/deployment environment, regulatory context? | Constraints | CON-03 | ACT-02 | none | Open |

Stakeholder is not available in this session (non-interactive); all questions remain open and dependants are NEEDS CLARIFICATION.

## Conflicts
| Competing claims | Sources | Who decides |
|---|---|---|
| None recorded. Tension only: CON-01 ("preliminary approach") vs CON-02 ("alternatives welcome"). They are compatible if CON-01 is a proposal. | SRC-1 | ACT-02 (Q-001) |

## External evidence
| ID | Server | Tool | Arguments | Date | Result summary | Repo | Effect on spec |
|---|---|---|---|---|---|---|---|
| EV-01 | DeepWiki MCP (https://mcp.deepwiki.com/mcp) | ask_wiki_question | repoName `celery/celery`; question "How does Celery handle task failure, retries, result storage and job status tracking for long-running tasks?" | 2026-10-16 | Celery tasks have states (PENDING, STARTED, SUCCESS, FAILURE, RETRY, REVOKED); retries configurable (`autoretry_for`, `max_retries`, backoff); results stored in a configurable result backend with expiry; `update_state` can report progress; time limits available. Cited source files present in raw output. Status: not independently verified beyond DeepWiki; relevant. Raw output: `ev_deepwiki.txt`. | celery/celery | Added FR-007 (EXTERNAL candidate), DEP-02, supported wording of RSK-02, shaped Q-009/Q-010 (status and failure options). Nothing was made CONFIRMED. |
