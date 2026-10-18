# Weekly News Curation App Requirements Specification
Version 0.1 | Date 2026-10-16 | Author: agent (requirements-spec skill) | Source: T2_hn_consumer-app-idea.md (Ask HN post by droyson, https://news.ycombinator.com/item?id=28651142). No external evidence consulted (see 03).

## 1. Problem restatement and scope
Restatement: A person who wants to read news only once a week is looking for an app or site that curates one week of news and orders it by relevance, because existing news apps show only the latest items and bury older relevant ones. The post is phrased as a question ("Is there a site..."), so it is unclear whether the goal is to find an existing product or to build one (Q-001); this spec records what the system would need to do if it were built.

Sources available: the single post. No stakeholder is reachable (non-interactive run).

- In scope (CONFIRMED): weekly news curation; one-week window; relevance-ordered presentation; app or site form.
- Out of scope: none stated.
- Unknown scope: news topics, regions, languages, sources, personalisation, delivery method, business model, who builds it (Q-001 to Q-007).

Do-not-invoke check: not a request for code or design; it is not clear that an approved spec exists; software-related. Proceeding.

## 2. Glossary
| Term | Definition |
|---|---|
| Curate | undefined, see Q-002 (selection by editors? algorithm? both?) |
| News | undefined, see Q-003 (topics, regions, languages, source types) |
| Relevance | undefined, see Q-004 (relevant to whom, by what signal) |
| Weekly | Candidate: a 7-day window, delivered/updated once per week. Exact window and day undefined, see Q-005 |
| Instant news | Author's term for apps (Google News, Apple News, NewsInShorts) that surface the most recent items first. Quote: "serve the purpose of instant news" |
| App/site | undefined platform, see Q-006 |

## 3. Actors
| ID | Actor | Status | Source |
|---|---|---|---|
| ACT-01 | Reader who checks news once a week (the post author) | CONFIRMED | "I can check the news once a week" |
| ACT-02 | News sources / publishers whose articles are curated | INFERRED | Curation implies articles from somewhere; rationale: not stated who supplies them |
| ACT-03 | Operator/curator of the service (editors or maintainers) | INFERRED | Rationale: someone must run it; whether human editors exist is Q-002 |

Payers, regulators: none identified, because the post says nothing about cost or compliance (Q-007).

## 4. Functional requirements
| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The system shall present a curated selection of news once per week cycle. | CONFIRMED (term "curate" UNRESOLVED) | "an app/site that will curate news weekly" | unprioritised | AC-001 (placeholder) | NEEDS CLARIFICATION |
| FR-002 | The system shall limit each edition to news from a single 1-week period. | CONFIRMED (window boundaries UNRESOLVED) | "curates news of 1 week" | unprioritised | AC-002 (placeholder) | NEEDS CLARIFICATION |
| FR-003 | The system shall order the items in each edition by relevance rather than by recency alone. | CONFIRMED (relevance UNRESOLVED) | "serves in level of relevance"; "only the most recent news is served up and I need to scroll way down for older relevant articles" | unprioritised | AC-003 (placeholder) | NEEDS CLARIFICATION |
| FR-004 | The system shall obtain articles from one or more news sources. | INFERRED | Rationale: curation needs an input of articles; not stated | unprioritised | AC-004 | NEEDS CLARIFICATION |
| FR-005 | The system shall be available to the reader as an app or a website. | CONFIRMED (which one UNRESOLVED, Q-006) | "looking for an app/site" | unprioritised | AC-005 (placeholder) | NEEDS CLARIFICATION |

Not added (no feature padding): notifications, accounts, search, bookmarks, personalisation. These are open questions only (Q-008).

## 5. Non-functional requirements
| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | The reader shall find the most relevant items of the week without having to scroll past less relevant or merely older items; target: the top [N] items of an edition are the relevant ones. N is a measurable placeholder. | UNRESOLVED (need confirmed; N missing) | Complaint: "I need to scroll way down for older relevant articles". SUGGESTED, unconfirmed: N = 10 | unprioritised | AC-006 (placeholder) | NEEDS CLARIFICATION |

Performance, security, privacy, reliability, portability, maintainability, compliance: none identified, because the post states nothing on them. Whether these matter is asked in Q-007.

## 6. Constraints
| ID | Constraint | Status | Source |
|---|---|---|---|
| CON-01 | Consumption cadence is once per week; the product must not depend on daily checking. | CONFIRMED | "I can check the news once a week"; "only checking news once in a week should be sufficient" (attributed to the book Make Time) |

Technology, legal, budget, time, deployment: none identified, because none stated.

## 7. Dependencies
| ID | Dependency | Status | Source |
|---|---|---|---|
| DEP-01 | Access to news content (feeds, APIs or licences) from publishers | INFERRED | Follows from FR-004; licensing terms unknown |
| DEP-02 | A source of relevance signals (editors, popularity, user input) | INFERRED | Follows from FR-003; see Q-004 |

## 8. Risks
| ID | Risk | Status | Likelihood / impact |
|---|---|---|---|
| RSK-01 | "Relevance" is subjective; the result may not match the reader's expectation. | INFERRED | not estimable / high |
| RSK-02 | Content licensing or copyright limits on republishing articles. | INFERRED | not estimable / possibly high |
| RSK-03 | Existing products may already satisfy the need, making a build unnecessary (Q-001). | INFERRED | not estimable / high |

## 9. Acceptance criteria
| ID | Requirement | Pass/fail test | Status |
|---|---|---|---|
| AC-001 | FR-001 | A new edition is available once in each [period TBD, Q-005]; no more than one per period. Passes if count per period = 1. | Placeholder, depends on Q-002, Q-005 |
| AC-002 | FR-002 | Every item in an edition has a publication date inside the edition's 7-day window [boundaries TBD, Q-005]. Passes if 100% of items are inside. | Placeholder |
| AC-003 | FR-003 | Given a ranking method defined by Q-004, items appear in non-increasing relevance score order, and at least one older item outranks a newer less relevant one in a test edition. | Placeholder, depends on Q-004 |
| AC-004 | FR-004 | Each item names its source and links to the original article. | Proposed |
| AC-005 | FR-005 | The edition can be opened on [platform TBD, Q-006] by a first-time user. | Placeholder |
| AC-006 | NFR-001 | In a test review of [M] editions, a reviewer rates >= [X]% of the top [N] items as relevant. M, X, N unconfirmed. | Placeholder |

## 10. Traceability matrix
| Source (quote) | Requirement | Acceptance criterion |
|---|---|---|
| "curate news weekly" | FR-001 | AC-001 |
| "curates news of 1 week" | FR-002 | AC-002 |
| "serves in level of relevance" | FR-003 | AC-003 |
| "app/site" | FR-005 | AC-005 |
| "need to scroll way down for older relevant articles" | NFR-001 | AC-006 |
| "I can check the news once a week" | CON-01, ACT-01 | (covered by AC-001) |
| (inference: curation needs articles) | FR-004, DEP-01 | AC-004 |

## 11. Change log
| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-16 | Initial draft from the single post. No answers received. |
