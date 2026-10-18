# Evaluation log, round 1 (skill v1, run 2026-10-16)

Method: 8 fresh agents, each given only `skill_v1.md` and one input, non-interactive. Outputs in `tests/run1/`. Verdicts read from each `03_validation_summary.md`; Varthik, T1 and the T1/V evidence reviewed in detail by me. Other outputs were checked at verdict and summary level only.

## Results

| Input | Tool used | Verdict | Notes |
|---|---|---|---|
| V Varthik | DeepWiki (OpenHands) | Not implementation-ready, 0 READY | Every buzzword kept as undefined term; EV-01 changed Q-006, Q-008 and added RSK-05; no requirement promoted from EV |
| T1 route software | DeepWiki (VROOM) | Not ready, 0 READY | EV-01 added DEP-03 and candidate objectives for FR-005, both EXTERNAL |
| T2 news app | none (no comparable repo) | Not ready | Mostly-open spec |
| T3 background jobs | DeepWiki (Celery) | Not ready | Declined to answer the "which stack?" question |
| T4 HN monitor | none | Not ready | |
| T5 agency advice | none | Declined, not a software spec | Correct behaviour, but format was improvised |
| T6 outsourcing cost | none | Not ready, 1 BLOCKED | Same-author budget contradiction logged |
| T7 pandas debate | none | Not ready; 16 reqs: 1 READY, 3 NEEDS CLARIFICATION, 12 BLOCKED, 5 conflicts | Conflicts preserved, not resolved; escalation rule triggered |

What worked: no run produced a "ready" spec from vague input; no run invented numbers; T5 declined; T7 kept conflicts; tools were used only on 3 of 8 inputs and always as EXTERNAL.

## Defects found (from agent reports) and fixes in v2

| # | Defect | Reported by | Fix |
|---|---|---|---|
| 1 | No output format for declined / non-software inputs | T5, T6 | Added "decline set" (Section 5) and borderline rules (Section 1) |
| 2 | "Is there a tool for X?" and mixed inputs not covered | T1, T2, T3, T4 | Borderline rules in Section 1 |
| 3 | Output folder rule vs a user-named folder | all 8 | User-named folder wins |
| 4 | "NOT READY" label undefined (only READY / NEEDS CLARIFICATION / BLOCKED exist) | T1, T2, T3, V | Replaced with defined labels |
| 5 | BLOCKED too narrow (scope questions) | T1 | Extended definition |
| 6 | Stated-but-undefined terms, partial facts, proposals fit no status | V, T1, T2, T7 | Term/meaning split, lower bounds, "as proposal" |
| 7 | No ID for conflicts (agent invented CF-nn) | T7 | Added `CF-nn` |
| 8 | Same-source contradictions and conflicts with no dependents | T4, T6, V | Rule extended |
| 9 | Acceptance criterion required on items that cannot have one | T6 | "N/A, not testable" allowed for non-requirements |
| 10 | Escalation ("stop and ask") vs "never stall" in non-interactive runs | T7, V | Non-interactive escalation rule |
| 11 | 5-question limit vs large open-question register | T7 | Clarified: limit is per interactive round |
| 12 | No rule for when to skip the MCP tool; no repo choice rule when user names none | T2, T4, T6, V | Skip rule in step 4; repo-choice rule in Section 8 |
| 13 | EV entry detail unspecified; effect on spec was not required in the template | T3 | Raw file + 2-4 line summary; new template section 12 "Effect of external evidence" |
| 14 | Check granularity and "cannot judge" | T4, T6 | "Cannot judge" is FAIL naming the question; N/A only when check cannot apply |

## Not fixed / open limitations
- All runs were non-interactive, so the "ask clarifying questions" path was never exercised.
- Agent reports are self-assessments; they found real defects but are not independent grading.
- Single run per input, so no variance estimate.
- DeepWiki answers are AI-generated summaries and were not independently verified.

## Round 2 (todo)
Re-run T5, T7, T6 and V with skill v2 and compare against round 1.

## Round 2 (skill v2, 2026-10-18): V, T5, T6, T7
Decline path, tool-skip reasons, CF-nn use and escalation placement worked without improvisation. V went from 0 to 7 BLOCKED (over-blocking, escalation fires); T7 had 0 READY (round 1: 1). Four defects found: BLOCKED vs NEEDS CLARIFICATION overlap; potential conflicts from undefined terms; NFR vs CON and status for non-requirement items; empty AC stubs. Fixed in skill v2.1 (v2 kept as `skill/skill_v2.md`).

## Round 3 (skill v2.1 = submitted skill.md, 2026-10-18)
| Input | Result |
|---|---|
| V | 0 READY, 1 NC, 7 BLOCKED, 13 Q, 3 potential CF, 2 EV, escalation |
| T6 | 0 READY, 2 NC, 4 BLOCKED, 10 Q, 1 potential CF, tool skipped, escalation |
| T7 | 20 reqs: 0 READY, 7 NC, 13 BLOCKED, 20 Q, 8 CF (5 actual, 3 potential), escalation |
| V interactive | Asked 5 questions; simulated stakeholder answers (incl. "don't know" and a self-contradiction); statuses updated; contradiction logged as actual CF-02, not resolved; legal point escalated; 20 items: 0 READY, 14 NC, 6 BLOCKED |

Tie-break reduced T7 BLOCKED share (86% to 65%); V unchanged (expected for one-sentence input). Remaining ambiguities reported by agents were deliberately NOT fixed so the submitted skill equals the tested skill; they are listed in the report's limitations.
