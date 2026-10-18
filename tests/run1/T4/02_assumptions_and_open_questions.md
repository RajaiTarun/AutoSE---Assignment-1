# Assumptions, Open Questions, Conflicts, Evidence

## Assumptions
| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | The user is a single person/team owner, not a multi-tenant audience. | Post uses "I"/"my". | Multi-user needs accounts, per-user examples, privacy. | User answer to Q-002. |
| ASM-02 | "Posts" means HN submissions, not comments. | Wording says "posts". | Missing comment-level matches. | Q-004. |
| ASM-03 | Notification is wanted close to real time. | "monitor" and "notify". | Over- or under-engineering of timing. | Q-005. |

## Open questions
| ID | Question | Why it matters | Blocks | Who can answer | Suggested default (not applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | Do you want to find an existing tool, or have a new one specified? Is the Reddit tool to be reused? | Changes whether this spec is needed at all, and its scope. | All FR (scope) | Post author (8ta4) | Treat as a new-tool spec | Open |
| Q-002 | Who are the users, how many, and where must it run (local, hosted)? Any budget/deadline/legal limits? | Scope, privacy, deployment. | NFR-003, CON-02, ASM-01 | Post author | Single user | Open |
| Q-003 | How does the user mark an example (paste URL, click, browser extension)? Can examples be removed or edited? Minimum number of examples? | Defines FR-001/005/006 testability. | FR-001, FR-005, FR-006 | Post author | None | Open |
| Q-004 | What counts as a "post" and "new": stories only, or comments, Show HN, Ask HN? Backfill of history? | Defines monitored set. | FR-002 | Post author | Stories only | Open |
| Q-005 | What does "diligently" mean: maximum delay between publication and notification? | Measurable NFR. | NFR-001, FR-002 | Post author | SUGGESTED, unconfirmed: a few minutes | Open |
| Q-006 | What is "similarity" and what makes a "match": fixed score threshold, top-N per day, user-tunable? What error rate is acceptable? Can the user give feedback on matches? | Core behaviour of FR-003/004; testability. | FR-003, FR-004, NFR-002 | Post author | None | Open |
| Q-007 | How should notifications arrive (email, push, chat, other) and how often (each match or digest)? | Defines FR-004. | FR-004, DEP-02 | Post author | None | Open |
| Q-008 | What does "engage" mean, and should the tool itself post or only alert? Any rules about promotional engagement? | Autonomy level; community-rules risk. | RSK-02, scope | Post author | Alert only, never posts | Open |
| Q-009 | What is the product and what problem class is it for? | Needed to judge example quality and scoring feasibility. | FR-003 | Post author | None | Open |
| Q-010 | Must the tool follow HN terms/API rules, and are there data-retention limits? | Compliance and access. | NFR-003 | Post author / legal | Comply with published rules | Open |
| Q-011 | Are there other quality needs (reliability, privacy of stored examples, usability)? | Category currently empty. | NFR-004 | Post author | None | Open |
| Q-012 | Which motivation is intended for engaging (see C-01)? | Defines success criteria. | none directly (RSK-04) | Post author | None | Open |

## Conflicts
| ID | Competing claims | Sources | Who decides | Dependents |
|---|---|---|---|---|
| C-01 | "there is more to life than karma, awards, and gold" vs "there is nothing more to life than karma, awards, and gold" | Source lines 15 and 16 (same author) | Post author | No functional requirement depends on it; affects only the success/motivation definition (RSK-04, Q-012). No item marked BLOCKED. Not resolved by the agent. |

## External evidence
None. No external tool consulted (DeepWiki not used: the source names no product category or repository that would reduce uncertainty beyond what the questions above already ask).

| ID | Entry |
|---|---|
| (none) | no external evidence consulted |
