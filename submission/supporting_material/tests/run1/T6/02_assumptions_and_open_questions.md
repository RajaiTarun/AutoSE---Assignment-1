# Assumptions, Open Questions, Conflicts, Evidence

## Assumptions
| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | The target is iPhone only (not iPad, Android or web). | Title and text say "iPhone" only. | Scope and cost change if other platforms are wanted. | Author's answer to Q-006 |
| ASM-02 | The author is the sole stakeholder and decision maker for budget and scope. | Post is written in first person, no other party named. | Hidden stakeholders (partners, investors) could override decisions. | Author's answer to Q-008 |
| ASM-03 | The requested spec covers the app only; choosing a vendor and estimating price are separate activities. | Skill stops at requirements; pricing needs a defined scope first. | Author may expect a price from this document. | Author's confirmation (Q-010) |
| ASM-04 | "Location framework" means the platform's native location-services API. | Natural reading for an iPhone app. | A different capability (maps, geocoding, geofencing) would change FR-003. | Author's answer to Q-004 |

## Open questions
Suggested defaults are suggestions only and are NOT applied. No stakeholder was available (non-interactive run), so all dependants are NOT READY.

| ID | Question | Why it matters | Requirements blocked | Who can answer | Suggested default (not applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | What does the app do (purpose, target users, the main thing a user accomplishes)? | No functional requirement beyond platform and three technical hints can be written; "moderate" is undefined. | FR-001..004, NFR-003, ACT-03 | Author | none | Open |
| Q-002 | What counts as a "view" and what are the up to five? Is five a hard maximum or an estimate? | FR-001 cannot be tested without the definition. | FR-001, AC-001 | Author | Treat as hard upper bound of distinct screens (SUGGESTED, unconfirmed) | Open |
| Q-003 | What user data is stored, how sensitive, kept on-device or on a server, any account/sync need? | Determines security, privacy, whether a backend exists. | FR-002, NFR-002 | Author | On-device only (SUGGESTED, unconfirmed) | Open |
| Q-004 | What is location used for (foreground only or background, accuracy, history kept)? | Defines FR-003 and privacy impact. | FR-003, NFR-001 | Author | Foreground only (SUGGESTED, unconfirmed) | Open |
| Q-005 | What is the maximum budget: "several thousand dollars" or up to $20,000+? What does it include (design, testing, submission, maintenance)? | Budget is a hard constraint and the post contradicts itself. | CON-01 | Author | none | Open |
| Q-006 | Which iPhone models and iOS versions must be supported? iPad? | Platform and test scope. | FR-004, AC-002 | Author | none | Open |
| Q-007 | Which tasks are in the outsourced scope: UI design, backend, testing, App Store submission, post-launch fixes? Who holds the developer account? | Scope boundary and cost drivers. | CON-02, ACT-04 | Author | none | Open |
| Q-008 | Who owns the source code/IP and who has final decision authority? | Contractual and decision rights. | CON-02 | Author | none | Open |
| Q-009 | What makes a developer "good" in measurable terms (portfolio, references, App Store apps shipped, rate range)? | CON-03 is untestable as written. | CON-03 | Author | none | Open |
| Q-010 | Is the author asking this spec to produce a price/vendor recommendation, or only a requirements document to give to vendors? Any deadline? | Sets scope of this work and whether CON-05 matters. | scope statement, CON-05 | Author | Requirements document only (SUGGESTED, unconfirmed) | Open |
| Q-011 | Are there privacy or legal duties for location and user data (e.g. jurisdiction of users, minors)? | Compliance may constrain design; cannot be verified by the agent. | NFR-001, NFR-002 | Author (and counsel if needed) | none | Open |

## Conflicts
| ID | Competing claims | Sources | Who decides | Blocked |
|---|---|---|---|---|
| C-01 | "I don't mind paying up to several thousand dollars" (suggests ceiling of a few thousand) vs. "Am I looking at $5,000, $10,000, $20,000, more?" (considers up to $20,000+). Both are the author's statements; no winner is picked. | S1, same post | Author | CON-01 (BLOCKED) |

## External evidence (EV)
None. DeepWiki was not consulted: the idea is a generic mobile app with no product category whose open-source projects would clarify a buzzword, and the open questions are about the author's intent, which external evidence cannot answer. No EV entries exist. Effect on spec: none.
