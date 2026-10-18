# Hacker News Post-Monitoring Tool Requirements Specification
Version 0.1 | 2026-10-16 | Author: agent (requirements-spec skill) | Source: `T4_hn_tool-for-monitoring.md` (Ask HN post by 8ta4, item 36625126). No external evidence consulted. Non-interactive run: all questions left open.

## 1. Problem restatement and scope
Restatement: A product owner wants a tool that learns from posts they flag as examples of problems their product solves, scores new Hacker News posts by similarity to those examples, and notifies them of matches so they can engage. The post also asks whether such a tool already exists.

Note on invocation: the post is partly a "does a tool exist?" question. It is treated as a statement of need; the spec does not answer whether a tool exists (see Q-001).

- In scope (CONFIRMED): marking example posts, monitoring new posts, similarity scoring, notification.
- Out of scope: none stated. Building vs buying is undecided (Q-001). Implementation, design and code are out of scope for this skill.
- Unknown scope: user count, hosting, source coverage beyond posts (comments? Show HN?), history/backfill, feedback loop, other sites.

## 2. Glossary
| Term | Definition |
|---|---|
| Post | A Hacker News submission. Whether comments count is undefined, see Q-004. |
| Example | A post the user marks as "an example of a problem my product can solve" (quote, line 6 of source). |
| New posts | Posts appearing after monitoring starts. Undefined: see Q-004. |
| Similarity | Closeness of a new post to the examples. Undefined metric, see Q-006. |
| Match | A new post whose score qualifies it for notification. Undefined threshold, see Q-006. |
| "diligently" | Undefined, see Q-005. |
| "engage" | Undefined (reply? upvote? just read?), see Q-008. |
| "my product" | Undefined: product and its problem domain not described, see Q-009. |

## 3. Actors
| ID | Actor | Status | Source |
|---|---|---|---|
| ACT-01 | Product owner / user (marks examples, receives notifications, engages) | CONFIRMED | "so I can engage with the post" |
| ACT-02 | Hacker News (source of posts; operator of the platform) | INFERRED | Rationale: posts must come from somewhere; "Hacker News seems like a more promising place". Operator's access rules unknown. |
| ACT-03 | HN post authors / community (affected by engagement) | INFERRED | Rationale: user will engage with their posts. Needs confirmation. |

## 4. Functional requirements
| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The tool shall let the user mark a post as an example of a problem the user's product can solve. | CONFIRMED | "A tool that can mark a post as an example of a problem my product can solve." | unprioritised | AC-001 (placeholder: marking mechanism unspecified, Q-003) | NEEDS CLARIFICATION |
| FR-002 | The tool shall monitor new Hacker News posts. | CONFIRMED | "monitor new posts"; "Hacker News seems like a more promising place" | unprioritised | AC-002 (placeholder: "new" and frequency undefined, Q-004, Q-005) | NEEDS CLARIFICATION |
| FR-003 | The tool shall assign each monitored post a score based on its similarity to the user's marked examples. | CONFIRMED | "scoring them based on their similarity to the examples I've provided" | unprioritised | AC-003 (placeholder: similarity metric and quality undefined, Q-006) | NEEDS CLARIFICATION |
| FR-004 | The tool shall notify the user when a post is found to be a match. | CONFIRMED | "When it finds a match, it should notify me" | unprioritised | AC-004 (placeholder: match rule and channel undefined, Q-006, Q-007) | NEEDS CLARIFICATION |
| FR-005 | The tool shall retain the user's marked examples between runs. | INFERRED | Rationale: scoring against "the examples I've provided" over ongoing monitoring implies the examples persist. | unprioritised | none yet, see Q-003 | NEEDS CLARIFICATION |
| FR-006 | The tool shall let the user remove a previously marked example. | INFERRED (suggestion only) | Rationale: a user may mark a wrong post. Not stated in source. | unprioritised | none yet, see Q-003 | NEEDS CLARIFICATION |

## 5. Non-functional requirements
| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | The tool shall detect and notify of a matching post within [UNRESOLVED: X minutes] of its publication. | UNRESOLVED | "diligently monitor" (buzzword). SUGGESTED, unconfirmed: none applied. | unprioritised | none yet, see Q-005 | NEEDS CLARIFICATION |
| NFR-002 | The tool shall achieve [UNRESOLVED: precision/recall target] when classifying posts as matches against a labelled test set. | UNRESOLVED | Implied by "scoring" and a prior tool using "natural language processing and machine learning"; no target stated. | unprioritised | none yet, see Q-006 | NEEDS CLARIFICATION |
| NFR-003 | The tool shall operate within Hacker News's terms of use and access rules. | UNRESOLVED | Not stated; compliance posture unknown. | unprioritised | none yet, see Q-010 | NEEDS CLARIFICATION |
| NFR-004 | Security, privacy, availability, usability, portability, maintainability | none identified, because the source says nothing about them. Treated as unknown, see Q-002, Q-011. | | | | |

## 6. Constraints
| ID | Constraint | Status | Source |
|---|---|---|---|
| CON-01 | The target platform is Hacker News. | CONFIRMED | "Hacker News seems like a more promising place." |
| CON-02 | Technology, budget, deadline, deployment environment, legal | none identified, because none are stated (Q-002). | |

## 7. Dependencies
| ID | Dependency | Status | Source |
|---|---|---|---|
| DEP-01 | A source of Hacker News post data (access method unknown) | INFERRED | Rationale: monitoring requires data; method is a design decision, not specified. |
| DEP-02 | A notification delivery channel | INFERRED | Rationale: FR-004 needs one; channel unspecified (Q-007). |
| DEP-03 | The user's earlier Reddit tool (context only; reuse not stated) | CONFIRMED (existence) | "I've developed a similar tool for Reddit." Reuse is UNRESOLVED (Q-001). |

## 8. Risks
| ID | Risk | Status | Likelihood / impact |
|---|---|---|---|
| RSK-01 | Similarity scoring with few examples gives many false matches or misses. | INFERRED | not estimable |
| RSK-02 | Engagement with HN posts for promotion may breach community norms or site rules. | INFERRED | not estimable; see Q-008, Q-010 |
| RSK-03 | Access to HN data may be rate-limited or restricted. | INFERRED | not estimable |
| RSK-04 | The source states contradictory motivation (see conflict C-01), so the success definition may be unclear. | INFERRED | low impact on functions |

## 9. Acceptance criteria
All are placeholders; none can yet be run as a third-party pass/fail test.
| ID | -> Req | Pass/fail test | Status |
|---|---|---|---|
| AC-001 | FR-001 | Given a post, the user can mark it as an example and it appears in the example set. Mechanism UNRESOLVED (Q-003). | Placeholder |
| AC-002 | FR-002 | A post published after monitoring begins is seen by the tool within [UNRESOLVED] minutes (Q-005). | Placeholder |
| AC-003 | FR-003 | For a given new post and example set, a numeric/ranked score is produced; quality threshold UNRESOLVED (Q-006). | Placeholder |
| AC-004 | FR-004 | A post meeting the match rule [UNRESOLVED] triggers one notification on the chosen channel [UNRESOLVED]. | Placeholder |

## 10. Traceability matrix
| Source (quote) | Requirement | AC |
|---|---|---|
| "mark a post as an example of a problem my product can solve" | FR-001 (FR-005, FR-006 inferred) | AC-001 |
| "diligently monitor new posts" / "Hacker News seems..." | FR-002, NFR-001, CON-01 | AC-002 |
| "scoring them based on their similarity to the examples" | FR-003, NFR-002 | AC-003 |
| "notify me so I can engage" | FR-004 | AC-004 |
| (no source) | NFR-003 | none |

FR-005, FR-006, NFR-003 have no direct source quote; they are INFERRED/UNRESOLVED and flagged as such.

## 11. Change log
| Date | Change |
|---|---|
| 2026-10-16 | Initial draft v0.1. |
