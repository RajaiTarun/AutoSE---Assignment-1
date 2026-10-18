# Assumptions, Open Questions, Conflicts, Evidence

Date 2026-10-18. Non-interactive run: nothing below has been asked or answered; all suggested defaults are suggestions only and are NOT applied.

## Assumptions

| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | "iPhone app" means a native app for iPhone devices (not a web app or cross-platform product). | The post says "iPhone app" and "iPhone coder". Status INFERRED. | Wrong technology class changes cost and scope. | Author's answer to Q-005. |
| ASM-02 | "The location framework" means the platform's built-in location services. | Only reading consistent with "using the location framework" on iPhone. Status INFERRED. | Different capability (e.g. third-party maps) changes FR-003 and DEP-01. | Author's answer to Q-004. |
| ASM-03 | "User data" is data about the app's end users and may include personal data. | "user data" plus location. Status INFERRED. | Privacy posture (RSK-01) mis-scoped. | Author's answer to Q-003, Q-009. |
| ASM-04 | The post is taken at face value as the author's current need; the terms, platforms and vendor sites it names may reflect an older period. | The source file carries no date; the platform and site names suggest it may be dated. Status INFERRED, unverified. | Platform versions, norms and price points may not apply today. | Author confirms current need and date (Q-005, Q-007). |

## Open questions

Suggested defaults are SUGGESTED, unconfirmed.

| ID | Question | Why it matters | Blocks | Who can answer | Suggested default (not applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | What does the app do (core purpose, main user tasks) and who are its end users? | Without it no meaningful functional requirement exists; "moderate" and "nothing too complicated" cannot be judged. Options: (a) consumer utility, (b) social/location sharing, (c) tracking/logging, (d) other. If unanswered: FR-001..003 stay blocked or vague. | FR-001, FR-002, FR-003 (meaning) | ACT-01 | none | Open |
| Q-002 | What are the (up to five) views, and what counts as a "view"? | Needed to make FR-001 testable. Options: screen / navigation destination / other. | FR-001, AC-001 | ACT-01 | Define view = distinct full-screen state (SUGGESTED) | Open |
| Q-003 | What user data is stored, how sensitive is it, where (on device only, or a server), for how long, and are user accounts needed? | Decides scope, backend need and privacy posture. Options: device-only / own server / third-party service / other. | FR-002, RSK-01 | ACT-01 | none | Open |
| Q-004 | What is location used for, with what accuracy, and only while the app is open or also in background? | Decides what is built and its privacy impact. | FR-003, DEP-01 | ACT-01 | none | Open |
| Q-005 | Which iPhone models and OS versions must be supported; iPad too; native or cross-platform? | Makes CON-01 testable; affects cost. | CON-01, AC-002 | ACT-01 | Support the current and previous major OS version (SUGGESTED) | Open |
| Q-006 | How is the app distributed (public App Store / private) and is it meant to earn revenue (paid, ads, in-app purchase, none)? | "make that money back from the app" implies a revenue hope; this may add requirements (payments) or constraints (store rules). | scope (unknown) | ACT-01 | none | Open |
| Q-007 | What is the budget ceiling as a number, what does "several thousand" mean, and how does it reconcile with "pay extra for good developers" and the $20,000+ range listed? Is the outsourcing decision conditional on a specific figure? | Resolves CF-01; defines CON-02, CON-03. Options: up to $5k / up to $10k / up to $20k / more / other. | CON-02, CON-03, CF-01 | ACT-01 (decider) | none | Open |
| Q-008 | Is there a deadline or target launch date? | No time constraint stated; affects plan and cost. | none currently (constraint absent) | ACT-01 | none | Open |
| Q-009 | What quality expectations apply: privacy/security of stored data, performance, offline use, accessibility, languages, legal/policy compliance? | No NFR is stated at all. | NFR section (empty), FR-002 | ACT-01 | none | Open |
| Q-010 | What does "done" mean and who accepts delivery (source code ownership, testing on real devices)? | Needed for acceptance criteria. | AC for all | ACT-01 | none | Open |

Note: 10 questions total. In an interactive session the highest-impact batch (max 5) would be Q-001, Q-007, Q-003, Q-004, Q-005.

## Conflicts

| ID | Competing claims | Sources | Type | Who decides | Dependents |
|---|---|---|---|---|---|
| CF-01 | (A) Budget "I don't mind paying up to several thousand dollars" / only "not too prohibitive" cost. (B) "I don't mind paying extra for good developers" and the author himself asks whether the cost is "$5,000, $10,000, $20,000, more?". | Same source, point 2 vs point 1 and the price-question line. | Potential (single-source tension arising from the undefined terms "several thousand", "extra", "prohibitive"). Inference: if "several thousand" means below about $10,000 it may conflict with the $20,000+ range; if it covers it, there is no conflict. Not picked as winner. | ACT-01 | CON-03, CON-02 (via threshold). |

## External evidence (EV)

None. No external evidence consulted, because the open questions concern the stakeholder's own decisions (budget, scope, platform choices, data policy) and the app's category is unknown; naming a comparable public repository would be a guess. No tool calls were made, so there are no raw tool output files.
