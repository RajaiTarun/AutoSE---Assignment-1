# Assumptions, Open Questions, Conflicts and Evidence

Date: 2026-10-18. Source: `T6_hn_outsourcing-budget-question.md` (cited as L<n>). Non-interactive run: no question was put to anyone; all remain open. The only person who could answer is the post author (sendos), who is unavailable (fixed original post).

## Assumptions

| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | "iPhone app" means a native app installed on iPhone, not a web app viewed in the phone browser. | The post says "iPhone app" and "iPhone coder" (L1, L11). | Whole technology constraint (CON-01) and location access would change. | Author's answer to Q-005 / Q-009. |
| ASM-02 | "the location framework" means the iPhone platform's built-in location API. | Wording "framework" (L9) points to a platform library. | If something else (a third-party service), FR-003 and DEP-01 change. | Author's answer to Q-004. |
| ASM-03 | "user data" means data belonging to the end users of the app. | Natural reading of L9. | If it means the author's own data/content, privacy NFR and FR-002 differ. | Author's answer to Q-003. |
| ASM-04 | The author is the sole decision maker (product owner and payer). | Post is written in first person with no other party (L5, L19). | Other approvers could change scope and budget. | Author's answer (Q-006, Q-010). |
| ASM-05 | The spec covers the app only; the price question and vendor questions are not part of the system. | Section 1 mixed-input rule. | If the author actually wants a procurement document, a different deliverable is needed. | Author confirms intent. |

## Open questions

Suggested defaults are SUGGESTIONS ONLY, not applied anywhere in the spec.

| ID | Question | Why it matters | Requirements blocked | Who can answer | Suggested default (NOT applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | What does the app do for its users (core purpose), and who are the users? | Without a purpose nothing can be specified or tested; the post only lists capabilities. | FR-004, FR-002, FR-003, NFR-001 (affects all) | Author (sendos) | None; any default would be invention. | OPEN |
| Q-002 | What counts as a "view" (screen, tab, modal)? Is "up to five" a hard cap or an illustrative example ("say")? | Decides whether FR-001 is testable and whether five is a limit. | FR-001 | Author | Hard cap of five screens (SUGGESTED, unconfirmed). | OPEN |
| Q-003 | Which user data is stored, where (on the device, on a server, both), for how long, and can the user delete it? | On-device vs server changes architecture, privacy and cost; "some" is not testable. | FR-002, NFR-001 | Author | On-device only (SUGGESTED, unconfirmed). | OPEN |
| Q-004 | What is location used for: one-time lookup or continuous tracking; foreground only or background; required accuracy; works without a connection? | Drives privacy posture, battery impact and the meaning of FR-003. | FR-003, NFR-001 | Author | Foreground, on-demand only (SUGGESTED, unconfirmed). | OPEN |
| Q-005 | Which iPhone models and OS versions must be supported? Is iPad in scope? | Defines the platform constraint and its acceptance test. | CON-01 (AC-002) | Author | Latest two OS versions, iPhone only (SUGGESTED, unconfirmed). | OPEN |
| Q-006 | What amount is "several thousand dollars"? Is it a hard cap? Does it cover only initial build or also maintenance, accounts and fees? What does "cost is not too prohibitive" mean as a number? | CON-02/CON-03 have no measurable target; also relates to the $5,000 to $20,000+ range the author asks about (L13). | CON-02, CON-03 | Author | None; a number would be invented. | OPEN |
| Q-007 | What do "moderate app" and "nothing too complicated" mean measurably (e.g. number of data entities, network use, third-party services)? | Buzzwords that stand in for scope. | FR-001, all NFRs | Author | Use the three listed capabilities only (SUGGESTED, unconfirmed). | OPEN |
| Q-008 | What privacy, security and regulatory expectations apply to location and stored user data (consent, jurisdiction, minors, health or financial data)? | Posture-changing; also decides whether a human escalation is needed. | NFR-001, FR-002, FR-003 | Author (and legal advice if needed) | Ask for consent before location use (SUGGESTED, unconfirmed). | OPEN |
| Q-009 | Does the app need a back end, network access, user accounts, or public App Store distribution? | Not stated; changes scope, dependencies (DEP-04) and ACT-04. | FR-002, DEP-04, ACT-04 | Author | No accounts, no back end (SUGGESTED, unconfirmed). | OPEN |
| Q-010 | Is the app a commercial product? ("make that money back", L19). What counts as success or being "made back"? | Sets priorities and success thresholds, none of which are stated. | priorities of all requirements | Author | None. | OPEN |
| Q-011 | Is there a delivery deadline? What does "good developers" (L17) mean as acceptance by the author (e.g. prior apps, reviews, code ownership)? | No deadline is stated; developer quality is stated only as a wish. Vendor selection itself is out of scope. | none directly (no dependents) | Author | None. | OPEN |

## Conflicts

| ID | Competing claims | Sources | Who decides |
|---|---|---|---|
| (none) | No conflicting claims found between or within sources. Closest case: "up to several thousand dollars" (L19) vs the price range question "$5,000, $10,000, $20,000, more?" (L13). The latter is a question, not a claim, so it is logged as ambiguity in Q-006. No dependents. | L13, L19 | n/a |

## External evidence

| ID | Server / tool | Repo | Summary | Raw file |
|---|---|---|---|---|
| (none) | No external evidence consulted. Reason in `03_validation_summary.md`. | | | |
