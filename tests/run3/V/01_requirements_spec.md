# Sovereign Agentic Harness (Varthik's idea) Requirements Specification

Version 0.1 | Date 2026-10-18 | Author: requirements-spec agent (non-interactive run) | Source documents: `tests/inputs/V_varthik.md` (the only stakeholder source); external evidence EV-01, EV-02 (raw files `ev_01.txt`, `ev_02.txt`).

Status legend: CONFIRMED / INFERRED / EXTERNAL / UNRESOLVED. `CONFIRMED (term) / UNRESOLVED (meaning)` = wording is in the source, meaning is not.

## 1. Problem restatement and scope

**Restatement.** The stakeholder wants a software system described as an "agentic harness" carrying nine adjectives (sovereign, local-first, enterprise-grade, model-agnostic, self-hosted, privacy-preserving, fully autonomous, plus the claim "India's first"). No purpose, users, scale, platform, timeline, budget or acceptance threshold is stated. Nearly every adjective is a buzzword whose meaning is undefined, so most of this spec is UNRESOLVED by design.

Source quote (L3 of input): *"I have an idea for a software system. Let's make India's first sovereign local-first, enterprise-grade, model-agnostic, self-hosted, privacy-preserving, fully autonomous agentic harness, and let's really push it."*

Check of "do not invoke" cases: software system is named, so the skill applies; it is not a request for code/architecture, no existing approved spec, no non-spec question asked (no mixed input).

- **In scope (CONFIRMED):** a software system called an "agentic harness" with the attributes listed in section 4/5/6.
- **Out of scope:** none stated. (Implementation, architecture and design are out of scope of this skill, not of the project.)
- **Unknown scope (UNRESOLVED):** what the harness does and harnesses (Q-001), who uses it (Q-011), scale, timeline, budget (Q-013), regulatory context (Q-012), what "India's first" and "really push it" mean (Q-009, Q-010).

## 2. Glossary

| Term | Status | Definition |
|---|---|---|
| agentic harness | undefined, see Q-001 | Stated term. Candidate meanings: runtime that orchestrates LLM agents and tools; agent evaluation/test harness; workflow platform. ASM-01 notes the working reading; EV-01/EV-02 show real products using the term for agent runtimes (EXTERNAL, not adopted). |
| fully autonomous | undefined, see Q-002 | Candidates: no human approval for any action; autonomous inside a policy sandbox with escalation; autonomous for read-only actions only. |
| sovereign | undefined, see Q-003 | Candidates: data residency in India; Indian ownership/control of code and operation; no dependency on foreign vendors/clouds/models; Indian legal jurisdiction. |
| local-first | undefined, see Q-004 | Candidates: works fully offline; local data is primary with optional sync; local inference by default. |
| enterprise-grade | undefined, see Q-005 | Candidates: SSO/RBAC, audit logging, availability SLA, compliance certification, vendor support. |
| model-agnostic | undefined, see Q-006 | Candidates: pluggable any-provider API; open-weight local models only; any hosted or local model. |
| self-hosted | undefined, see Q-007 | Candidates: customer on-prem server; customer VPC/cloud; developer laptop; Indian cloud. |
| privacy-preserving | undefined, see Q-008 | Candidates: no data leaves the deployment; encryption at rest/in transit; compliance with an applicable data-protection law; data minimisation. |
| India's first | undefined, see Q-009 | A positioning claim, not a requirement; "India's" and "first" are both undefined and "first" is unverifiable here. |
| "let's really push it" | slogan, see Q-010 | Not a requirement. Possible intent: prioritisation, timeline urgency, ambition level. |

## 3. Actors

| ID | Actor | Status | Source / rationale |
|---|---|---|---|
| ACT-01 | Varthik, idea owner | CONFIRMED (name, as owner of the idea) / UNRESOLVED (role and decision authority) | Input title "V: Varthik's idea". Q-011. |
| ACT-02 | Operators/administrators of the organisation that self-hosts the system | INFERRED | "self-hosted", "enterprise-grade" imply someone installs and runs it. Needs confirmation. |
| ACT-03 | End users who delegate work to agents | INFERRED | "agentic harness" implies users of agents. Q-011. |
| ACT-04 | AI agents / models executed by the harness | INFERRED | "agentic", "model-agnostic". |
| ACT-05 | Regulators / legal authorities | INFERRED (weak) | "sovereign", "privacy-preserving" may imply legal obligations. Q-012. |

## 4. Functional requirements

Priority is "unprioritised" for all: "let's really push it" gives no ranking (Q-010).

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The system shall function as an "agentic harness" [what it harnesses and the capabilities it provides: TBD, Q-001]. | CONFIRMED (term) / UNRESOLVED (meaning) | L3 "agentic harness" | unprioritised | none yet, see Q-001 | BLOCKED (scope: Q-001 decides what would be built) |
| FR-002 | The system shall operate "fully autonomously" [list of action classes requiring no human approval: TBD, Q-002]. | CONFIRMED (term) / UNRESOLVED (meaning) | L3 "fully autonomous" | unprioritised | none yet, see Q-002 | BLOCKED (CF-01, CF-03, Q-002) |

## 5. Non-functional requirements

Classification doubt: "sovereign" and "local-first" could be constraints (law/deployment) rather than quality attributes; placed in NFR per Section 3 rule ("if unsure, choose NFR and note the doubt").

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | The system shall be "sovereign" [measurable criterion: TBD, Q-003]. (Doubt: may be a CON.) | CONFIRMED (term) / UNRESOLVED (meaning) | L3 "sovereign" | unprioritised | none yet, see Q-003 | BLOCKED (CF-02, CF-03, Q-003) |
| NFR-002 | The system shall be "local-first" [measurable criterion: TBD, Q-004]. (Doubt: may be a CON.) | CONFIRMED (term) / UNRESOLVED (meaning) | L3 "local-first" | unprioritised | none yet, see Q-004 | BLOCKED (CF-02, Q-004) |
| NFR-003 | The system shall be "enterprise-grade" [attribute list and thresholds: TBD, Q-005]. | CONFIRMED (term) / UNRESOLVED (meaning) | L3 "enterprise-grade" | unprioritised | none yet, see Q-005 | BLOCKED (CF-01, Q-005) |
| NFR-004 | The system shall be "privacy-preserving" [data classes, threat model, applicable law: TBD, Q-008, Q-012]. | CONFIRMED (term) / UNRESOLVED (meaning) | L3 "privacy-preserving" | unprioritised | none yet, see Q-008 | BLOCKED (CF-02, CF-03, Q-008) |
| NFR-005 | The system shall be "model-agnostic" [set of models/providers to support: TBD, Q-006]. | CONFIRMED (term) / UNRESOLVED (meaning) | L3 "model-agnostic" | unprioritised | none yet, see Q-006 | BLOCKED (CF-02, Q-006) |

## 6. Constraints

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| CON-01 | The system shall be "self-hosted" [hosting party and target environment: TBD, Q-007]. | CONFIRMED (term) / UNRESOLVED (meaning) | L3 "self-hosted" | unprioritised | none yet, see Q-007 | NEEDS CLARIFICATION (understood behaviour, only the who/where detail is missing; judgment call, see 03) |

Technology, legal, budget and time constraints: none identified, because none are stated in the input (Q-012, Q-013 ask).

## 7. Dependencies

| ID | Dependency | Status | Rationale |
|---|---|---|---|
| DEP-01 | One or more AI models/providers | INFERRED | "model-agnostic", "agentic". |
| DEP-02 | Hosting infrastructure controlled by the adopting organisation | INFERRED | "self-hosted". |
| DEP-03 | Stakeholder answers to Q-001..Q-013 (Varthik or delegate) | INFERRED | Nothing else can define the buzzwords. |
| DEP-04 | Legal interpretation of "sovereign" and "privacy-preserving" in India | INFERRED | Possible regulatory obligations; unverified (Q-012). |

## 8. Risks

Likelihood and impact are not estimated (no basis in the input).

| ID | Risk | Status | Rationale |
|---|---|---|---|
| RSK-01 | The adjective stack contains mutually straining demands (CF-01, CF-02, CF-03) so the system as worded may be unbuildable or mean different things to different readers. | INFERRED | Term analysis. |
| RSK-02 | "Fully autonomous" combined with access to data and network could cause unintended actions or data leaving the deployment unless autonomy and sandboxing/permissions are specified separately. | INFERRED, motivated by EV-01 (real harness exposes autonomy as an approval policy separate from sandbox/permission profiles) | EV-01 EXTERNAL; not a requirement. |
| RSK-03 | "India's first" may be unverifiable or false, creating reputational or legal exposure if used publicly. | INFERRED | No evidence either way; tools not applicable. |
| RSK-04 | Unverified legal/compliance obligations (data protection, sovereignty rules) may apply and are unknown to this analysis. | INFERRED | Q-012. |
| RSK-05 | "Let's really push it" with no scope boundary invites unbounded scope. | INFERRED | Q-010, Q-013. |

## 9. Acceptance criteria

No acceptance criterion can be written for any requirement because no term has a measurable meaning. No AC-nnn IDs were issued (an empty stub criterion is discouraged by Section 3).

| Requirement | AC |
|---|---|
| FR-001 | none yet, see Q-001 |
| FR-002 | none yet, see Q-002 |
| NFR-001 | none yet, see Q-003 |
| NFR-002 | none yet, see Q-004 |
| NFR-003 | none yet, see Q-005 |
| NFR-004 | none yet, see Q-008 |
| NFR-005 | none yet, see Q-006 |
| CON-01 | none yet, see Q-007 |

## 10. Traceability matrix

| Source (input L3 quote) | Requirement / item | Acceptance criterion |
|---|---|---|
| "agentic harness" | FR-001 | none yet (Q-001) |
| "fully autonomous" | FR-002 | none yet (Q-002) |
| "sovereign" | NFR-001 | none yet (Q-003) |
| "local-first" | NFR-002 | none yet (Q-004) |
| "enterprise-grade" | NFR-003 | none yet (Q-005) |
| "privacy-preserving" | NFR-004 | none yet (Q-008) |
| "model-agnostic" | NFR-005 | none yet (Q-006) |
| "self-hosted" | CON-01 | none yet (Q-007) |
| "India's first" | not a requirement; glossary, Q-009, RSK-03 | N/A, not testable |
| "let's really push it" | not a requirement; glossary, Q-010, RSK-05 | N/A, not testable |
| Title "Varthik's idea" | ACT-01 | N/A |

## 11. Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-18 | Initial draft from V_varthik.md; DeepWiki consulted (EV-01, EV-02). No stakeholder answers received (non-interactive). |

## 12. Effect of external evidence

- **EV-01** (openai/codex, DeepWiki `ask_wiki_question`): showed a real agent harness expresses autonomy as an approval policy (untrusted / on-request / never), separate from sandbox and permission profiles, with a pluggable model-provider abstraction. Changes: gave candidate options (EXTERNAL) for Q-002 and Q-006; added RSK-02 (INFERRED); informed CF-01 wording (autonomy and governance are not intrinsically contradictory). No requirement was created or changed in status.
- **EV-02** (All-Hands-AI/OpenHands, DeepWiki `ask_wiki_question`): showed self-hosting variants (local, Docker, VM, company infrastructure), "any LLM" provider support and an optional confirmation mode. Changes: candidate options (EXTERNAL) for Q-007, Q-006, Q-002. No requirement created.
- Neither EV informs "sovereign", "India's first", "enterprise-grade" or "privacy-preserving" beyond a weak hint; Q-003, Q-005, Q-009 are stakeholder/legal decisions, no tool consulted for them.
