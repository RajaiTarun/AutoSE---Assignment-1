# Test inputs (real, unedited, scraped 2026-10-16)

| ID | Source | Size | What it stresses in the skill |
|---|---|---|---|
| Varthik | Assignment brief (supplied scenario) | 1 sentence | Buzzwords, undefined autonomy, privacy, deployment |
| T1 | HN Ask: route software for 100+ locations (2011) | very short | Clear need, but missing users, scale limits, output form |
| T2 | HN Ask: app that curates news weekly | short | Consumer idea, vague quality terms ("curates"), no constraints |
| T3 | HN Ask: website that runs long background jobs | short | Technical ask, NFRs without numbers |
| T4 | HN Ask: tool to monitor Hacker News | medium | Concrete use case, partial detail |
| T5 | HN Ask: dev/design agency alongside AI | long | **Mostly not a software spec** (business advice). Should trigger the "do not invoke / escalate" rule |
| T6 | HN Ask: cost to outsource iPhone app development | medium | Budget question, no actual product described |
| T7 | pandas issue #61317 (22 comments) | long | **Conflicting stakeholders** (maintainers disagree on design), backward-compat constraint |

Fetched with the HN Algolia API (`hn.algolia.com/api/v1/items/<id>`) and GitHub REST API. Text is the original post (HTML tags stripped only).
