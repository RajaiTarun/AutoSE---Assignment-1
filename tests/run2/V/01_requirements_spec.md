# Sovereign Agentic Harness (Varthik's idea) Requirements Specification

- Version: 0.1 (initial draft, non-interactive run)
- Date: 2026-10-18
- Author: Claude (agent), following skill `requirements-spec`
- Source documents: `tests/inputs/V_varthik.md` (one sentence, line 4). External evidence: EV-01 (DeepWiki, see section 12).
- Status legend: CONFIRMED / INFERRED / EXTERNAL / UNRESOLVED. `CONFIRMED (term) / UNRESOLVED (meaning)` means the wording is in the source but what it means is not defined.

**Headline:** the source is a single sentence made almost entirely of undefined buzzwords. Most of this spec is therefore UNRESOLVED, which is the correct result for this input. It is **not implementation-ready**.

## 1. Problem restatement and scope

**Restatement.** A stakeholder (Varthik) wants a software system described as a "sovereign local-first, enterprise-grade, model-agnostic, self-hosted, privacy-preserving, fully autonomous agentic harness", and wants to "really push it". The source gives no users, scope, environment, thresholds, deadlines or definitions of any of these terms.

**Intake checks.** Sources available: the one-sentence brief only. It is a software request (not declined), asks no non-spec question (not mixed input), and contains no instructions aimed at the agent.

| Scope bucket | Content |
|---|---|
| In scope | Writing a requirements specification for the system described in the quote. |
| Out of scope | Any code, architecture, design, stack choice or build plan. Verifying whether the system would be "India's first" (see Q-010; this is a positioning claim, not a requirement). |
| Unknown scope | Everything about what the harness does, for whom, on what, at what scale, by when and for what budget (Q-001, Q-009, Q-011, Q-014). |

## 2. Glossary

No term is defined by the source. All of the following are used by the source and are undefined.

| Term (as quoted) | Definition status | Candidate meanings are in |
|---|---|---|
| "agentic harness" | undefined, see Q-001 | Q-001 |
| "fully autonomous" | undefined, see Q-002 | Q-002 |
| "sovereign" | undefined, see Q-003 | Q-003 |
| "local-first" | undefined, see Q-004 | Q-004 |
| "self-hosted" | undefined, see Q-005 | Q-005 |
| "model-agnostic" | undefined, see Q-006 | Q-006 |
| "privacy-preserving" | undefined, see Q-007 | Q-007 |
| "enterprise-grade" | undefined, see Q-008 | Q-008 |
| "India's first" | undefined (claim of uniqueness; unverified), see Q-010 | Q-010 |
| "really push it" | undefined (effort/ambition), see Q-011 | Q-011 |

## 3. Actors

| ID | Actor | Status | Source / rationale |
|---|---|---|---|
| ACT-01 | Varthik, originator of the idea | CONFIRMED (exists) / UNRESOLVED (role and decision authority) | Input file title "V: Varthik's idea"; "I have an idea for a software system". |
| ACT-02 | Enterprise organisation adopting the system | INFERRED | Rationale: "enterprise-grade" implies enterprise buyers or users. Who they are is open (Q-009). |
| ACT-03 | Operator / administrator who installs and runs the system | INFERRED | Rationale: "self-hosted" implies someone hosts and runs it. |
| ACT-04 | End user or principal who delegates work to agents | INFERRED | Rationale: an "agentic harness" implies agents acting for someone. |
| ACT-05 | Regulator or authority relevant to sovereignty and privacy | INFERRED | Rationale: "sovereign" and "privacy-preserving" suggest an external rule-setter. Which one is open (Q-003, Q-007). |

## 4. Functional requirements

Priority is "unprioritised" throughout (nothing in the source ranks anything; Q-011).

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The system shall be an agentic harness (scope and meaning of "agentic harness" are not defined). | CONFIRMED (term) / UNRESOLVED (meaning) | L4: "agentic harness" | unprioritised | AC-001 (stub, see Q-001) | BLOCKED (scope question Q-001 changes what the requirement means) |
| FR-002 | The system shall operate "fully autonomously" (the allowed level of human involvement is not defined). | CONFIRMED (term) / UNRESOLVED (meaning) | L4: "fully autonomous" | unprioritised | AC-002 (stub, see Q-002) | BLOCKED (Q-002; CF-02) |

No other functional requirements are stated. No functions (what the agents do, tools they use, interfaces, triggers) are identified, because the source names none. Adding them would be invention.

## 5. Non-functional requirements

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | The system shall be model-agnostic (set of models and the meaning of "agnostic" are not defined). Category: portability. | CONFIRMED (term) / UNRESOLVED (meaning) | L4: "model-agnostic" | unprioritised | AC-003 (stub, see Q-006) | BLOCKED (CF-01) |
| NFR-002 | The system shall be privacy-preserving (protected data, threat model and guarantee are not defined). Category: privacy. | CONFIRMED (term) / UNRESOLVED (meaning) | L4: "privacy-preserving" | unprioritised | AC-004 (stub, see Q-007) | BLOCKED (CF-01; Q-007) |
| NFR-003 | The system shall be sovereign (what is sovereign, and with respect to whom, is not defined). Category: compliance / data residency. | CONFIRMED (term) / UNRESOLVED (meaning) | L4: "sovereign" | unprioritised | AC-005 (stub, see Q-003) | BLOCKED (CF-01; Q-003 changes what the requirement means) |
| NFR-004 | The system shall be enterprise-grade (no attributes or thresholds are defined). Category: umbrella quality attribute. | CONFIRMED (term) / UNRESOLVED (meaning) | L4: "enterprise-grade" | unprioritised | AC-006 (stub, see Q-008) | BLOCKED (CF-02; Q-008 changes what the requirement means) |

No performance, reliability, usability, maintainability or scalability requirements are stated; none are added. If "enterprise-grade" is meant to carry them, Q-008 asks which.

## 6. Constraints

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| CON-01 | The system shall be self-hosted (who hosts it and on what is not defined). | CONFIRMED (term) / UNRESOLVED (meaning) | L4: "self-hosted" | unprioritised | AC-007 (stub, see Q-005) | NEEDS CLARIFICATION |
| CON-02 | The system shall be local-first (meaning and its relation to "self-hosted" are not defined). | CONFIRMED (term) / UNRESOLVED (meaning) | L4: "local-first" | unprioritised | AC-008 (stub, see Q-004) | BLOCKED (CF-01; Q-004 changes what the requirement means) |

None identified for: budget, deadline, technology stack, named law or standard, existing system integration. Reason: the source states none of these (Q-014). Note that "sovereign" and "privacy-preserving" may imply legal constraints, but which law applies is unknown (Q-003, Q-007, Q-016).

## 7. Dependencies

| ID | Dependency | Status | Rationale |
|---|---|---|---|
| DEP-01 | AI models the harness drives, hosted locally or remotely (some access path to them) | INFERRED | "model-agnostic" implies models exist outside the harness. EV-01 (EXTERNAL) shows a comparable project reaches models through a configurable endpoint (`llm.base_url`) or by delegating to external agent CLIs. |
| DEP-02 | Compute, hosting and network infrastructure on which the system is self-hosted | INFERRED | "self-hosted" and "local-first" need somewhere to run. Owner unknown (Q-005). |
| DEP-03 | Stakeholder decisions on Q-001 to Q-016 from Varthik (authority unconfirmed, ASM-01) | INFERRED | Nothing is specifiable without them. |
| DEP-04 | Legal or regulatory interpretation of "sovereign" and "privacy-preserving" | INFERRED | Terms suggest compliance obligations that no one has named (Q-003, Q-007, Q-016). |

## 8. Risks

Likelihood and impact are not estimable from the source; none are guessed.

| ID | Risk | Status | Basis |
|---|---|---|---|
| RSK-01 | "Fully autonomous" could mean no human confirmation at all, so the agent could take harmful or irreversible actions unchecked. | INFERRED | Gap analysis. EV-01 (EXTERNAL) indicates a comparable project treats autonomy as a configurable confirmation setting, i.e. autonomy is a spectrum, not binary. |
| RSK-02 | Model-agnostic support for remotely hosted models could send data outside the "sovereign" / "privacy-preserving" / "local-first" boundary. | INFERRED | Tension recorded as CF-01. |
| RSK-03 | The claim "India's first" may be false or unverifiable and could become a false marketing statement. | INFERRED | Claim has no cited basis (Q-010). |
| RSK-04 | With no scope, users, budget, timeline or stop criterion, "really push it" invites unbounded scope and schedule overrun. | INFERRED | Q-011, Q-014. |
| RSK-05 | "Enterprise-grade" may silently import expensive expectations (certifications, SLAs, support) that nobody has agreed. | INFERRED | Q-008. |
| RSK-06 | Sovereignty and privacy obligations may be legal or compliance obligations the agent cannot verify. | INFERRED | Q-003, Q-007, Q-016. Escalation trigger in the validation summary. |

## 9. Acceptance criteria

No acceptance criterion can be finalised because every requirement contains an undefined term. The entries below are **stubs**: a test shape with bracketed parameters that the stakeholder must fill. Status of every stub is UNRESOLVED. Any concrete suggestion is labelled "SUGGESTED, unconfirmed" and is not applied.

| ID | Requirement | Pass/fail test shape (stub) | Status |
|---|---|---|---|
| AC-001 | FR-001 | A reviewer can mark each capability in the agreed harness capability list [list: Q-001] as present or absent. | UNRESOLVED |
| AC-002 | FR-002 | For the agreed task set [Q-015], the number of human interventions required per task is at most [N: Q-002]. SUGGESTED, unconfirmed: N = 0 if "fully autonomous" means no approvals at all. | UNRESOLVED |
| AC-003 | NFR-001 | The agreed task set [Q-015] runs against [k: Q-006] different models from the agreed list [Q-006] with configuration changes only and no code change. | UNRESOLVED |
| AC-004 | NFR-002 | Observing the system during [workflow: Q-015] shows that data classes [Q-007] leave the agreed boundary [Q-003] zero times. SUGGESTED, unconfirmed. | UNRESOLVED |
| AC-005 | NFR-003 | An audit against the agreed sovereignty criteria [Q-003] finds all criteria met. | UNRESOLVED |
| AC-006 | NFR-004 | Each attribute in the agreed enterprise-grade list [Q-008] meets its own agreed threshold. | UNRESOLVED |
| AC-007 | CON-01 | The system can be installed and run on infrastructure of the agreed type [Q-005] operated by the agreed party [Q-005]. | UNRESOLVED |
| AC-008 | CON-02 | With external network access disabled, the agreed core functions [Q-004] still work. SUGGESTED, unconfirmed. | UNRESOLVED |

## 10. Traceability matrix

| Source (file line, quote) | Requirement | Acceptance criterion |
|---|---|---|
| L4: "agentic harness" | FR-001 | AC-001 (stub) |
| L4: "fully autonomous" | FR-002 | AC-002 (stub) |
| L4: "model-agnostic" | NFR-001 | AC-003 (stub) |
| L4: "privacy-preserving" | NFR-002 | AC-004 (stub) |
| L4: "sovereign" | NFR-003 | AC-005 (stub) |
| L4: "enterprise-grade" | NFR-004 | AC-006 (stub) |
| L4: "self-hosted" | CON-01 | AC-007 (stub) |
| L4: "local-first" | CON-02 | AC-008 (stub) |
| L4: "India's first" | none (positioning claim, Q-010, RSK-03) | N/A, not a requirement |
| L4: "let's really push it" | none (Q-011, RSK-04) | N/A, not a requirement |
| Agent inference from the quotes above | ACT-02 to ACT-05, DEP-01 to DEP-04, RSK-01 to RSK-06 | N/A |
| EV-01 | Candidate options in Q-001, Q-002, Q-004, Q-005, Q-006; support for RSK-01, DEP-01 | N/A |

## 11. Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-18 | Initial draft from one-sentence brief. DeepWiki consulted once (EV-01). No answers from stakeholders. |

## 12. Effect of external evidence

| EV | What it changed in this spec |
|---|---|
| EV-01 (DeepWiki, All-Hands-AI/OpenHands) | (1) Added EXTERNAL candidate options to Q-001 (harness scope), Q-002 (autonomy as a confirmation setting), Q-004 and Q-005 (local backend, Docker, desktop, embeddable library, remote backend), Q-006 (configurable model endpoint, delegation to other agent CLIs). (2) Added a supporting note to RSK-01 and the wording of DEP-01. (3) **No requirement was created, and no status was changed or promoted.** The stated input outranks this evidence. |
