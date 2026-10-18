# Assumptions, Open Questions, Conflicts

## Assumptions
| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | The goal is a product that could be specified (built), not only a pointer to an existing site. | The skill needs a system to specify; the post is ambiguous (Q-001). | Spec is wasted effort if the author only wants a recommendation. | Author's answer to Q-001. |
| ASM-02 | The reader is a single general consumer, not a team or enterprise. | Post is first-person, personal habit. | Multi-user, admin and compliance needs would be missed. | Author or product owner answer to Q-007. |

## Open questions
| ID | Question | Why it matters | Requirements blocked | Who can answer | Suggested default (not applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | Do you want to find an existing weekly news service, or have one built? Options: (a) find existing, (b) build new, (c) both, (d) other. | Determines whether a spec is needed at all. | All (scope) | Post author / product owner | (b) for the purpose of this spec | Open |
| Q-002 | What does "curate" mean? (a) human editors, (b) algorithmic ranking, (c) hybrid, (d) other. | Changes operating model, actors, cost. | FR-001, ACT-03, AC-001 | Product owner | none | Open |
| Q-003 | Which news? Topics (general, tech, local), regions, languages, source types. | Defines content scope and sources. | FR-004, DEP-01 | Product owner | SUGGESTED, unconfirmed: general world news in English | Open |
| Q-004 | What does "relevance" mean and to whom? (a) same for everyone (importance/popularity), (b) personalised to reader interests, (c) editor choice, (d) other. | Core of the idea; cannot test ordering without it. | FR-003, NFR-001, AC-003, AC-006 | Post author | none | Open |
| Q-005 | What is the weekly cycle? Rolling last 7 days at time of opening, or a fixed edition on a set day? Which day/time zone? | Defines the window and release. | FR-001, FR-002, AC-001, AC-002 | Post author | SUGGESTED, unconfirmed: fixed weekly edition covering the previous 7 days | Open |
| Q-006 | App or site? (a) mobile app, (b) website, (c) both, (d) email digest, (e) other. | Deployment environment. | FR-005, AC-005 | Post author | none | Open |
| Q-007 | Are there privacy, cost, licensing, scale or compliance expectations (e.g. free, no tracking, number of users)? | Security and privacy posture, feasibility. | NFR set (empty), RSK-02, DEP-01 | Product owner | none | Open |
| Q-008 | Any desired extras (reminders, accounts, search, saved items)? | Scope; not added without confirmation. | none now | Post author | none | Open (low impact) |
| Q-009 | How many items per edition is "right", and what counts as success (the [N] in NFR-001)? | Makes NFR-001 testable. | NFR-001, AC-006 | Post author | SUGGESTED, unconfirmed: N = 10 | Open |

No stakeholder was reachable, so none of these were asked interactively; all dependants are marked NEEDS CLARIFICATION.

## Conflicts
None identified. The post makes no contradictory claims. (Mild tension noted, not a conflict: the author says daily news is overrated yet wants relevant older articles surfaced; this is consistent with the weekly goal.)

## External evidence
None. No EV entries: no tool was called (see 03).
