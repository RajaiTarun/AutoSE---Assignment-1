# Assumptions, Open Questions, Conflicts and Evidence

## Assumptions
| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | One person makes the trip (single route, not split between several walkers). | Post says "I will be walking". | Need multi-route splitting. | Answer to Q-006 |
| ASM-02 | The list contains street addresses or names that can be mapped to coordinates. | A boutique list normally has addresses. | RSK-02: routing cannot start without geocoding effort. | Answer to Q-003 |
| ASM-03 | The need is for a software capability (FR-001..004), not only a tool recommendation. | Needed to apply this skill at all. | Whole spec may be unnecessary (RSK-04). | Answer to Q-001 |

## Open questions
All unanswered: the session was non-interactive, so none could be asked. No suggested default has been applied.

| ID | Question | Why it matters | Requirements blocked | Who can answer | Suggested default (NOT applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | Do you want new software built, or a recommendation of an existing tool/workaround that handles 100+ stops? Options: build a tool; recommend existing tool; both; other. | Decides if this skill applies and what scope is. | All (FR-001..007) | Poster (ACT-01) | Treat as "need a capability" | Open |
| Q-002 | What is the maximum number of locations (100, 150, 500, other)? | Sets size limits and test data. | FR-001, AC-001, NFR-001 | ACT-01 | SUGGESTED, unconfirmed: 200 | Open |
| Q-003 | What does the list look like (spreadsheet/CSV, plain text, addresses, names, lat/long)? | Defines input handling and geocoding need. | FR-001, DEP-01, ASM-02 | ACT-01 | SUGGESTED, unconfirmed: CSV of addresses | Open |
| Q-004 | What should the route optimise: shortest walking distance, shortest time, fewest backtracks, other? ("route" undefined) | Defines success of the core function. | FR-002, FR-005 | ACT-01 | SUGGESTED, unconfirmed: minimum total walking time | Open |
| Q-005 | What form must the result take (ordered list, map, turn-by-turn, exported to a phone/maps app)? | Defines output. | FR-006, AC for FR-002 | ACT-01 | SUGGESTED, unconfirmed: ordered list | Open |
| Q-006 | Is this one walk on one day, or spread over several days/walkers? | May require splitting into several routes. | FR-002, ASM-01 | ACT-01 | SUGGESTED, unconfirmed: multiple days, split by area | Open |
| Q-007 | Must visits respect boutique opening hours or appointments? | Adds time-window requirements; affects feasibility. | FR-002, RSK-03 | ACT-01 | None | Open |
| Q-008 | Any constraint on cost (free only?), platform (web, phone, spreadsheet) or deadline? | Constraints currently empty. | CON-03, DEP-03 | ACT-01 | None | Open |
| Q-009 | Is walking the only mode, or may subway/bus be used between stops? | Changes distance/time model. | FR-004 | ACT-01 | None | Open |
| Q-010 | Where does the route start and end (home, hotel, a boutique, anywhere)? | Required by routing problems. | FR-007 | ACT-01 | None | Open |
| Q-011 | How long may the tool take to produce a route? | NFR-001 untestable without it. | NFR-001 | ACT-01 | None | Open |
| Q-012 | Is the list or plan confidential or should it stay on the user's device (privacy)? | Affects use of online services. | NFR (privacy), DEP-02 | ACT-01 | None | Open |

## Conflicts
None identified. The input is a single short statement from one author and contains no contradictory claims.

## External evidence
| ID | Server | Tool | Arguments | Date | Repo | Result summary | Effect on spec |
|---|---|---|---|---|---|---|---|
| EV-01 | DeepWiki MCP (`https://mcp.deepwiki.com/mcp`) | `ask_wiki_question` | `{repoName: "VROOM-Project/vroom", question: "What input limits (number of stops) and optimisation objectives does vroom define for route optimisation, and what inputs does it require?"}` | 2026-10-16 | VROOM-Project/vroom | Answer: VROOM "does not define a global hard limit on the number of stops"; objectives include minimising cost, duration, distance and maximising assigned tasks; inputs are jobs, vehicles, optional matrices; per-vehicle `max_tasks`, time windows and start/end exist. Status: grounded in repo wiki, treated as EXTERNAL, not independently verified. | Shaped Q-004 (candidate objectives), Q-007 (time windows), Q-010 (start/end), and added DEP-03 and the FR-005/FR-007 placeholders. No requirement was made CONFIRMED from it. Raw log: `ev_deepwiki.txt`. |

Note: `tools/list` returned the three expected tools (`ask_wiki_question`, `read_wiki_structure`, `read_wiki_contents`) among others. The answer arrived after a keep-alive ping; no retry was needed.
