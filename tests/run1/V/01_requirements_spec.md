# Sovereign Agentic Harness (working name) Requirements Specification
Version 0.1 | 2026-10-16 | Author: requirements-spec agent | Sources: `V_varthik.md` (one sentence, "Assignment 1 brief"); EV-01 (DeepWiki, background only)

Status legend: CONFIRMED (stated in input), INFERRED (agent inference, needs confirmation), EXTERNAL (outside evidence, needs confirmation), UNRESOLVED (open).

## 1. Problem restatement and scope
Restatement: Varthik wants a software system described only as a "sovereign local-first, enterprise-grade, model-agnostic, self-hosted, privacy-preserving, fully autonomous agentic harness". The input gives a list of adjectives and no tasks, users, scale, environment, or success measures. Most of this spec is therefore UNRESOLVED, which is the correct result for input this short.

- In scope (stated): an "agentic harness" with the quality labels above (FR-001 to FR-006, NFR-001 to NFR-003).
- Out of scope: not stated. See Q-001, Q-010.
- Unknown scope: what the harness runs/orchestrates, which agents, tools, tasks, users, size, timeline, budget. See Q-001, Q-010, Q-011.
- Not invoked as "do not invoke": input is a software idea needing a spec; no spec exists. OK to proceed.

## 2. Glossary
| Term | Definition |
|---|---|
| Agentic harness | undefined, see Q-001 (candidates: runtime that runs LLM agents with tools; orchestration framework; testing/eval harness; agent SDK) |
| Sovereign | undefined, see Q-002 |
| Local-first | undefined, see Q-003 |
| Enterprise-grade | undefined, see Q-004 |
| Privacy-preserving | undefined, see Q-005 |
| Fully autonomous | undefined, see Q-006 |
| Model-agnostic | partly clear (not tied to one model); "which models" undefined, see Q-007 |
| Self-hosted | partly clear (run by the adopter, not a vendor SaaS); environment undefined, see Q-008 |
| India's first | undefined (first by what measure?), see Q-009 |
| "really push it" | undefined (ambition? schedule? scope?), see Q-009 |

## 3. Actors
| ID | Actor | Status | Source / rationale |
|---|---|---|---|
| ACT-01 | Varthik (idea originator, speaker of the brief) | CONFIRMED | "I have an idea for a software system"; role (sponsor? user? builder?) unknown, Q-010 |
| ACT-02 | Organisation that self-hosts and operates the system | INFERRED | "self-hosted" and "enterprise-grade" imply an operating organisation |
| ACT-03 | End users of the harness | INFERRED | Any system has users; who they are is UNRESOLVED (Q-010) |
| ACT-04 | Model provider(s) | INFERRED | "model-agnostic" implies models are supplied by someone |
| ACT-05 | Regulator / data authority | INFERRED | "sovereign", "privacy-preserving" suggest a regulatory context; unconfirmed (Q-012) |

## 4. Functional requirements
Priority is "unprioritised" for all (none stated; "really push it" is not a priority ranking).

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The system shall be an agentic harness (meaning of "harness" is a placeholder, [UNRESOLVED, Q-001]). | CONFIRMED (term) / UNRESOLVED (meaning) | "agentic harness" | unprioritised | none yet, see Q-001 | NEEDS CLARIFICATION |
| FR-002 | The system shall work with more than one AI model without being tied to a single model vendor (set of supported models: [UNRESOLVED, Q-007]). | CONFIRMED | "model-agnostic" | unprioritised | none yet, see Q-007 | NEEDS CLARIFICATION |
| FR-003 | The system shall be deployable and operable by the adopting organisation on infrastructure it controls (environment: [UNRESOLVED, Q-008]). | CONFIRMED | "self-hosted" | unprioritised | none yet, see Q-008 | NEEDS CLARIFICATION |
| FR-004 | The system shall operate local-first (what must work locally/offline: [UNRESOLVED, Q-003]). | CONFIRMED (term) / UNRESOLVED (meaning) | "local-first" | unprioritised | none yet, see Q-003 | NEEDS CLARIFICATION |
| FR-005 | The system shall carry out agent tasks without human intervention to the degree of [UNRESOLVED autonomy level, Q-006]. | CONFIRMED (term) / UNRESOLVED (meaning) | "fully autonomous" | unprioritised | none yet, see Q-006 | NEEDS CLARIFICATION |

No further functional requirements are recorded: the input names no tasks, inputs, outputs or integrations. Adding typical agent features (tool use, memory, logging, approval gates) would be invention. They are raised only as questions (Q-001, Q-006).

## 5. Non-functional requirements
| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | The system shall be "sovereign" per a measurable definition of [UNRESOLVED, Q-002] (e.g. data residency in India, Indian ownership, no foreign-controlled dependency). | CONFIRMED (term) / UNRESOLVED (meaning) | "sovereign" | unprioritised | none yet, see Q-002 | NEEDS CLARIFICATION |
| NFR-002 | The system shall meet enterprise-grade targets for [UNRESOLVED attributes and thresholds, Q-004]. | CONFIRMED (term) / UNRESOLVED (meaning) | "enterprise-grade" | unprioritised | none yet, see Q-004 | NEEDS CLARIFICATION |
| NFR-003 | The system shall preserve privacy as defined by [UNRESOLVED data classes, threat model, and rules, Q-005]. | CONFIRMED (term) / UNRESOLVED (meaning) | "privacy-preserving" | unprioritised | none yet, see Q-005 | NEEDS CLARIFICATION |

Categories with no entries: performance, reliability, usability, portability, maintainability, compliance have none identified, because the input states nothing about them (compliance may come from Q-012).

## 6. Constraints
| ID | Statement | Status | Source |
|---|---|---|---|
| CON-01 | Deployment is self-hosted (no vendor-run service) | CONFIRMED | "self-hosted" |
| CON-02 | Technology, law, budget, time: none identified, because none is stated (see Q-011, Q-012) | n/a | n/a |

## 7. Dependencies
| ID | Dependency | Status | Rationale |
|---|---|---|---|
| DEP-01 | Access to one or more AI models (local or hosted) | INFERRED | "model-agnostic" |
| DEP-02 | Infrastructure on which to self-host | INFERRED | "self-hosted" |
| DEP-03 | Stakeholder decisions on all Q-001 to Q-014 | INFERRED | Spec cannot progress without them |

## 8. Risks
| ID | Risk | Status | Likelihood / impact |
|---|---|---|---|
| RSK-01 | Requirements are driven by buzzwords; builders interpret them differently and build the wrong thing | INFERRED | High / High |
| RSK-02 | Tension: "fully autonomous" vs "enterprise-grade" and "privacy-preserving" (enterprises usually expect oversight and audit; autonomy raises data-exposure risk) | INFERRED | Unknown / High |
| RSK-03 | Tension: "local-first/self-hosted/sovereign" vs "model-agnostic" if some models are only available through foreign cloud APIs | INFERRED | Unknown / High |
| RSK-04 | "India's first" and "really push it" may imply an unstated schedule or scope-growth pressure | INFERRED | Unknown / Medium |
| RSK-05 | Autonomous agents acting without confirmation may cause irreversible actions (EV-01 shows real projects offer confirmation modes) | EXTERNAL (EV-01) + INFERRED | Unknown / High |

## 9. Acceptance criteria
No acceptance criteria can be written yet. Each requirement lacks a measurable definition, so any criterion would be invented.

| ID | Requirement | Pass/fail test |
|---|---|---|
| (none) | FR-001 to FR-005, NFR-001 to NFR-003 | none yet, see the Q-nnn listed per requirement |

## 10. Traceability matrix
| Source | Requirement | Acceptance criterion |
|---|---|---|
| "agentic harness" | FR-001 | none yet (Q-001) |
| "model-agnostic" | FR-002 | none yet (Q-007) |
| "self-hosted" | FR-003, CON-01 | none yet (Q-008) |
| "local-first" | FR-004 | none yet (Q-003) |
| "fully autonomous" | FR-005 | none yet (Q-006) |
| "sovereign" | NFR-001 | none yet (Q-002) |
| "enterprise-grade" | NFR-002 | none yet (Q-004) |
| "privacy-preserving" | NFR-003 | none yet (Q-005) |
| "India's first", "really push it" | none (not a requirement) | Q-009 |
| EV-01 | no requirement; informs Q-006, Q-008, RSK-05 | n/a |

## 11. Change log
| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-16 | Initial draft from the one-sentence brief; EV-01 consulted. |

## Effect of external evidence (EV-01)
EV-01 changed: (a) Q-006 now offers concrete autonomy options (confirm-before-high-risk-actions toggle, optional action risk analysis, no confirmation); (b) Q-008 offers deployment options (container, headless VM with API-key auth, desktop app); (c) RSK-05 added. No requirement was created or promoted from EV-01.
