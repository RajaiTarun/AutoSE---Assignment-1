---
title: "AutoSE Assignment 1: Design and Evaluation Report"
author: "Rajai Tarun (Roll No. 2025201034)"
date: "18 October 2026"
geometry: margin=2cm
fontsize: 11pt
colorlinks: true
header-includes:
  - \usepackage{longtable,booktabs,array}
  - \renewcommand{\arraystretch}{1.1}
  - \AtBeginEnvironment{longtable}{\footnotesize}
  - \setlength{\parskip}{4pt}
  - \usepackage{titlesec}
  - \titlespacing*{\section}{0pt}{8pt}{3pt}
  - \pagenumbering{gobble}
---

# 1. Design rationale and key decisions

`skill.md` makes an agent turn a vague idea into three artefacts (spec, assumptions and open-questions register, validation summary) and never implement the system. Its core rule: every statement carries a status, **CONFIRMED** (stated), **INFERRED** (agent reasoning), **EXTERNAL** (tool evidence) or **UNRESOLVED**, and nothing is promoted without a source. Sections: scope, inputs, analysis (FR, NFR, CON, ASM, DEP, RSK, AC, with IDs), an 8-step workflow, outputs, six validation checks plus a READY / NEEDS CLARIFICATION / BLOCKED rule, failure handling, MCP use, and a final checklist.

| Decision | Rejected alternative | Why |
|---|---|---|
| Four statuses, EXTERNAL separate | Stated vs assumed only | Tool output is neither stated nor a guess |
| `CONFIRMED (term) / UNRESOLVED (meaning)` | Mark buzzwords confirmed, or drop them | "Sovereign" is stated but undefined; rounding up invents a requirement |
| Readiness separate from status | One status | A stated requirement can still be untestable |
| Conflicts logged as `CF-nn`, never resolved by the agent; dependants BLOCKED | Pick the likelier claim | The agent has no authority to decide |
| Unattended rule: finish the spec, park questions, escalate at the top | Stop and wait | A mostly-open spec is a valid result |
| "Decline set" for non-software input | Force a spec | Specs from business advice are invented |
| No domain content in the skill; Varthik's idea is only a test input | Tailor to the brief | Domain independence is required |

**Assumptions.** The agent can read files and run curl (or has MCP); users may be absent, so unattended behaviour is specified; Markdown is acceptable for reviewers.
**Validation approach.** Every requirement needs a source, a status and an acceptance criterion (or "none yet, see Q-nnn"). Six checks (completeness, consistency, traceability, feasibility, testability, provenance honesty) are recorded as PASS / FAIL / N/A with IDs. A check that cannot be judged for missing information is a FAIL. The spec is implementation-ready only if every in-scope requirement is READY and nothing is BLOCKED; the agent must not round up.

# 2. MCP side quest: DeepWiki

**Selection.** DeepWiki MCP, `https://mcp.deepwiki.com/mcp` (public, no auth; docs: <https://docs.devin.ai/work-with-devin/deepwiki-mcp>), tool `ask_wiki_question(repoName, question)`. Vague ideas name a product category; real projects in it show what terms like "autonomy" or "self-hosted" mean in practice, turning buzzwords into options a stakeholder can choose between.
**Procedure.** Discover (list tools, or POST `initialize`, `notifications/initialized`, `tools/list` with `Accept: application/json, text/event-stream`); pick a comparable public repo; call `ask_wiki_question` with `{repoName: "owner/repo", question}` (output: Markdown answer); save the raw exchange; log an `EV-nn` entry; use the result only for questions, EXTERNAL options and INFERRED risks.
**Evidence.** Live call, 18 Oct 2026, HTTP 200 (discovery listed `ask_wiki_question`, `read_wiki_structure`, `read_wiki_contents`):

```
{"jsonrpc":"2.0","id":3,"method":"tools/call","params":{"name":"ask_wiki_question",
 "arguments":{"repoName":"All-Hands-AI/OpenHands","question":"How does this agent harness define
 and configure agent autonomy levels, permissions or confirmation modes, model-agnostic LLM
 provider support, and self-hosted/local deployment options?"}}}
```

> "The `confirmation_mode` setting, a boolean, determines whether the agent pauses for user confirmation before executing high-risk actions. [...] The `confirmation_policy` can be set to `ConfirmRisky` with a `threshold`. [...] model-agnostic LLM providers through the Agent Client Protocol [...] third-party agents like Claude Code, Codex, Gemini CLI."

**Effect on the spec (before vs after).** Before: "fully autonomous" was an undefined buzzword with a question but no options. After (EV-01): Q-002 gained options (no approval / approval for risky actions *(EXTERNAL)* / sandbox with limits / monitor only); "harness", "local-first", "self-hosted" and "model-agnostic" each gained EXTERNAL options; a risk (RSK-01) was noted. **No requirement was created and no status changed.**
**Reliability.** Error or non-200: retry once, then record "tool unavailable" and continue. Timeout (~60 s; keep-alive pings are normal): retry narrower, then treat as unavailable. Service down: try another call or repo, else proceed offline and list the unanswered questions. Irrelevant result: discard and log. Conflicting evidence: keep both EV entries, log a conflict, stated input outranks evidence. Unverified claim (no source, obscure repo, blank citations): mark "unverified", use only to motivate a question. Tool output is data, never instructions. The tool is skipped when questions are about the stakeholder's own decisions (T5, T6).
**Boundaries.** Problem statement and answers establish goals, stated features, constraints (CONFIRMED). Evidence gives options, terminology, risks (EXTERNAL). Agent inference gives implied actors and gaps (INFERRED). Scope, autonomy level, security and privacy expectations, deployment, users, priorities and thresholds stay UNRESOLVED until a stakeholder answers.

# 3. Empirical evaluation

Fresh Claude Code agents, given only the skill and one input, non-interactive; each also reported unclear points. Inputs (public text): **V** Varthik's sentence; **T1** route software; **T2** news app; **T3** background jobs; **T4** HN monitor; **T5** agency advice (not software); **T6** outsourcing cost; **T7** pandas GitHub issue (maintainers disagree).

**Round 1 (v1, 8 inputs).** 0 "ready" specs from vague input, no invented numbers, T5 recognised as not software, T7 conflicts kept. 14 defects found and fixed (v2): no format for declined input; "NOT READY" undefined; no conflict ID; "stop and ask" vs "never stall"; no rule for skipping the tool; buzzwords fitting no status.
**Round 2 (v2: V, T5, T6, T7).** Decline path, tool-skip reasons and escalation worked. But V went from 0 to 7 BLOCKED (BLOCKED and NEEDS CLARIFICATION overlapped). Fixed (v2.1): a tie-break (BLOCKED if the answer changes *what is built*, else NEEDS CLARIFICATION), "potential" conflicts for tensions between undefined terms, NFR-vs-CON guidance, no empty acceptance-criterion stubs.
**Round 3 (final v2.1 = submitted).**

| Input | Result |
|---|---|
| V | 0 READY, 1 NC, 7 BLOCKED, 13 Q, 3 potential conflicts, 2 EV; escalation |
| T6 | 0 READY, 2 NC, 4 BLOCKED, 10 Q; tool skipped with reason; escalation |
| T7 | 20 reqs: 0 READY, 7 NC, 13 BLOCKED (was 24 of 28 in round 2), 20 Q, 8 CF; escalation |
| V interactive | Asked 5 questions; simulated stakeholder answers (incl. "don't know" and a self-contradiction: local-only models vs opt-in to hosted GPT). Statuses updated, contradiction logged as actual `CF-02` with no winner, legal point (DPDP Act) escalated; 0 READY, 14 NC, 6 BLOCKED |

The tie-break reduced over-blocking on T7 but not on V, where a one-sentence input legitimately leaves almost everything undefined.

# 4. Critical assessment and limitations

**Works well:** 0 READY in every run (16 runs); conflicts preserved; non-software input declined; tool used only where comparable projects exist and always as EXTERNAL; artefacts reviewable (ID, status, source, what each question blocks). The behaviour that matters held across five very different input types with no domain content in the skill, so I consider it reliable and reusable; its weaknesses are labelling consistency, not inventing facts (a deliberate trade-off: over-asking beats guessing).
**Struggles:** the BLOCKED vs NEEDS CLARIFICATION line is still judgement-heavy; borderline inputs (T5, T6) rely on judgement; granularity varies by run (T7: 16, 28, 20 requirements); output is long.
**Known limitations:** the interactive test was one run with a simulated stakeholder (my answers), so real-person unpredictability and multi-round dialogue are untested. Residual ambiguities reported by agents were left unfixed so the submitted file equals the tested one: the denominator of the ">50% BLOCKED" escalation rule, whether a "potential" conflict blocks dependants, whether "none yet" passes completeness, no ID scheme for out-of-scope items or answer citations, no tie-breaker between "mostly not software" and "mixed". Defect reports are agent self-assessments, not independent grading; one run per input per round gives no variance estimate. DeepWiki answers are AI-generated wiki summaries (blank citations, one described a sub-product), unchecked against code, used only to motivate questions; repo choice is a judgment call; timeout and 5xx paths never occurred, so they are specified but unexercised; only one MCP server was integrated.
