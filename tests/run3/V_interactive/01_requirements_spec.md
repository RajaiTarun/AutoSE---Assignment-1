# Agentic Governance Platform ("India-first sovereign agentic harness") Requirements Specification

Version 0.2 (after stakeholder round 1) | Date 2026-10-18 | Author: requirements-spec agent | Stakeholder: Varthik

Source documents:
- **S0**: `tests/inputs/V_varthik.md` (one sentence, "Assignment 1 brief").
- **A1-Q001 .. A1-Q005, A1-Extra**: Varthik's round-1 answers to Q-001 .. Q-005, plus one unsolicited addition. These were relayed by the coordinator and simulated by the test operator. Each is cited as e.g. "A1-Q001 (c)".
- **EV-01**: DeepWiki result (EXTERNAL, partially unverified). Raw files: `ev_01_request.txt`, `ev_01_response.txt`.

Status legend: CONFIRMED / INFERRED / EXTERNAL / UNRESOLVED. Split form `CONFIRMED (term) / UNRESOLVED (meaning)` is used for terms.
Readiness legend: READY / NEEDS CLARIFICATION (NC) / BLOCKED.

---

## 1. Problem restatement and scope

**Restatement.** Varthik wants a software platform, described in S0 as an "agentic harness". Round 1 narrowed it to a platform that governs many autonomous agents across an organisation (A1-Q002 (c)). It must be self-hosted in India, work with local models, and stay inside an admin-set policy (A1-Q001 (c)).

**In scope (CONFIRMED)**
- A platform that governs many agents across an organisation (A1-Q002 (c)).
- Agent autonomy bounded by an admin-set sandbox and policy (A1-Q001 (c)).
- SSO, role-based access control and audit logs (A1-Q005 (a),(b)).
- Local models, with an admin opt-in to hosted models such as GPT (A1-Q004, A1-Extra). These two statements conflict, see CF-02.
- Deployment on an on-premises server, or on a private cloud in India (A1-Q004).
- Data and processing stay in India, and the platform complies with the DPDP Act (A1-Q003 (a),(c)).

**Out of scope**
- "India's first" is marketing, not a requirement (A1-Q003: "just marketing"). It is recorded in the glossary only.
- "Let's really push it" is a slogan and is not a requirement.
- Code, architecture and design are not covered. This skill stops at requirements.

**Unknown scope (UNRESOLVED)**
- What "govern" means: Q-006.
- The use cases and the domain of the agents: Q-017.
- Availability and scale: Q-012.
- Timeline, budget and priorities: Q-018.

## 2. Glossary

| Term | Status | Meaning in this spec |
|---|---|---|
| agentic harness | CONFIRMED (term) / CONFIRMED (working meaning) | "A platform that governs many agents across an organisation" (A1-Q002 (c)). The word "govern" is undefined, see Q-006. |
| fully autonomous | CONFIRMED (term) / CONFIRMED (meaning) | "Autonomous inside an admin-set sandbox and policy" (A1-Q001 (c)). The option text Varthik selected also said "no per-action prompts". What happens on an out-of-policy action is undefined, see Q-014. |
| sovereign | CONFIRMED (term) / CONFIRMED (meaning, with legal detail open) | Data and processing stay in India, and DPDP Act compliance (A1-Q003 (a),(c)). Which DPDP obligations apply is UNRESOLVED, see Q-008. |
| India's first | CONFIRMED as marketing wording, NOT a requirement | A1-Q003. |
| local-first | CONFIRMED (term) / UNRESOLVED (meaning, partly informed) | Partly informed by A1-Q004: local models and on-premises hosting. Whether offline or air-gapped operation is needed is open, see Q-011. |
| self-hosted | CONFIRMED (term) / INFERRED (meaning) | The organisation hosts the platform itself. INFERRED from A1-Q004, which names on-premises or private cloud. |
| privacy-preserving | CONFIRMED (term) / UNRESOLVED (meaning, partly informed) | Working meaning: no data leaves the premises (A1-Q004) plus DPDP compliance. Whether more is needed, such as encryption or PII handling, is open, see Q-019. |
| enterprise-grade | CONFIRMED (term) / partly defined | A1-Q005 gives SSO and role-based access (a) and audit logs (b). Availability and scale are "don't know yet" (Q-012). Compliance standards were not selected (Q-013). |
| model-agnostic | CONFIRMED (term) / partly defined | A1-Extra: an admin can opt in to hosted models such as GPT. A1-Q004: local models only. These conflict, see CF-02. Which models are supported is open, see Q-010. |
| premises | UNRESOLVED | Used in "no data leaves the premises" (A1-Q004). Conflicts with "private cloud", see CF-03 and Q-009. |
| admin | CONFIRMED (role word) / UNRESOLVED (who, how many tiers) | See ACT-01. |
| DPDP Act | CONFIRMED (wording) / INFERRED (identity) | ASM-03: India's Digital Personal Data Protection Act. Obligations are not verified by this agent. |

## 3. Actors

| ID | Actor | Status | Source / rationale |
|---|---|---|---|
| ACT-01 | Administrator: sets sandbox and policy, opts in to hosted models | CONFIRMED (role) / UNRESOLVED (privileges) | A1-Q001 ("admin-set"), A1-Extra ("an admin") |
| ACT-02 | Organisation users who use or run agents on the platform | INFERRED | "across an organisation" (A1-Q002). The users are not described. |
| ACT-03 | Agents (software) governed by the platform | CONFIRMED | "many agents" (A1-Q002) |
| ACT-04 | Auditor or compliance reader of audit logs | INFERRED | Audit logs (A1-Q005 (b)) imply a reader. |
| ACT-05 | Regulator under the DPDP Act | INFERRED | A1-Q003 (c). The regulator is not named. |
| ACT-06 | Agent owner or builder who defines or registers agents | INFERRED | Needed if the platform governs agents. Not stated, see Q-017. |
| ACT-07 | Product sponsor / stakeholder (Varthik) | CONFIRMED | S0 |

## 4. Functional requirements

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| FR-001 | The platform shall govern multiple agents across an organisation. | CONFIRMED (term "govern") / UNRESOLVED (meaning) | A1-Q002 (c) | unprioritised | none yet, see Q-006 | BLOCKED (the meaning of "govern" decides what is built) |
| FR-002 | The platform shall let agents execute actions autonomously, with no per-action human approval, while the actions fall inside the administrator-defined sandbox and policy. | CONFIRMED | A1-Q001 (c). "No per-action prompts" comes from the option text Varthik selected. | unprioritised | AC-001 | NC (policy contents undefined, Q-014) |
| FR-003 | The platform shall prevent an agent action that falls outside the administrator-defined policy from executing without further authorisation. | INFERRED | Implied by "inside" a sandbox and policy (A1-Q001 (c)). Whether it blocks, logs or escalates is not stated. | unprioritised | AC-002 | NC (Q-014) |
| FR-004 | The platform shall allow an administrator to define the sandbox and the policy that bound agent autonomy. | CONFIRMED | A1-Q001 ("admin-set sandbox and policy") | unprioritised | AC-003 | NC (what policy can express, scope per agent / role / organisation, Q-014) |
| FR-005 | The platform shall authenticate users through single sign-on. | CONFIRMED | A1-Q005 (a) | unprioritised | AC-004 | NC (IdP and protocol, Q-015) |
| FR-006 | The platform shall enforce role-based access control on its functions. | CONFIRMED | A1-Q005 (a) | unprioritised | AC-005 | NC (role list undefined, Q-015) |
| FR-007 | The platform shall keep audit logs. | CONFIRMED (term) / UNRESOLVED (contents, retention, integrity, reader) | A1-Q005 (b) | unprioritised | none yet, see Q-016 | NC |
| FR-008 | The platform shall run agent model inference only on models hosted on the organisation's own infrastructure. | CONFIRMED | A1-Q004 ("Only local models, no data leaves the premises") | unprioritised | AC-006 | BLOCKED (CF-02) |
| FR-009 | The platform shall allow an administrator to opt in to hosted models such as GPT. | CONFIRMED | A1-Extra | unprioritised | none yet, see Q-007 | BLOCKED (CF-02) |
| FR-010 | The platform shall be model-agnostic: an administrator shall be able to choose among multiple models without changing agent definitions. | CONFIRMED (term "model-agnostic") / INFERRED (this meaning) / UNRESOLVED (which models) | S0; the meaning is INFERRED from A1-Q004 and A1-Extra | unprioritised | AC-007 (placeholder) | NC (Q-010) |
| FR-011 | CANDIDATE ONLY: the platform may provide an optional risk classifier for agent actions, as seen in comparable projects. | EXTERNAL | EV-01 (partially unverified). Not accepted by the stakeholder. | unprioritised | none yet, see Q-014 | NC |

## 5. Non-functional requirements

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| NFR-001 | The platform shall not transmit organisational data outside the organisation's premises. | CONFIRMED (the meaning of "premises" is UNRESOLVED) | A1-Q004 ("no data leaves the premises") | unprioritised | AC-008 | BLOCKED (CF-02, CF-03) |
| NFR-002 | The platform shall be privacy-preserving. | CONFIRMED (term) / UNRESOLVED (meaning). Partly informed by NFR-001 and CON-04. | S0, A1-Q004, A1-Q003 (c) | unprioritised | none yet, see Q-019 | NC (judgment call: the answers supply a working meaning, so only additions are open) |
| NFR-003 | The platform shall be local-first. | CONFIRMED (term) / UNRESOLVED (meaning). Partly informed by FR-008 and CON-01. | S0, A1-Q004 | unprioritised | none yet, see Q-011 | NC (same judgment call as NFR-002) |
| NFR-004 | The platform shall meet an availability target of [TBD]. | UNRESOLVED | A1-Q005: "I don't know yet" | unprioritised | AC-009 (placeholder) | NC (lacks a number only) |
| NFR-005 | The platform shall support a scale of [TBD users, TBD agents, TBD concurrent tasks]. | UNRESOLVED | A1-Q005: "I don't know yet" | unprioritised | AC-010 (placeholder) | NC (lacks numbers only) |

No numeric value is suggested for NFR-004 or NFR-005, to avoid inventing numbers.

## 6. Constraints

| ID | Statement | Status | Source | Priority | AC | Readiness |
|---|---|---|---|---|---|---|
| CON-01 | The platform shall be deployable on an organisation's on-premises server. | CONFIRMED | A1-Q004 | unprioritised | AC-011 (placeholder for the OS and hardware) | NC (environment details missing, Q-010) |
| CON-02 | The platform shall be deployable on a private cloud located in India. | CONFIRMED | A1-Q004 | unprioritised | none yet, see Q-009 | BLOCKED (CF-03) |
| CON-03 | All data and processing shall stay within India. | CONFIRMED | A1-Q003 (a) | unprioritised | AC-012 | BLOCKED (CF-02: the location of hosted models is unknown) |
| CON-04 | The platform shall comply with the DPDP Act. | CONFIRMED (wording) / UNRESOLVED (which obligations) | A1-Q003 (c) | unprioritised | none yet, see Q-008 | NC. This is a legal obligation this agent cannot verify, so an escalation is recorded in `03`. |

## 7. Dependencies

| ID | Dependency | Status | Source / rationale |
|---|---|---|---|
| DEP-01 | The organisation's identity provider for SSO | INFERRED | FR-005 |
| DEP-02 | On-premises compute for local models, and the model runtimes and weights | INFERRED | FR-008. Hardware is not stated, see Q-010. |
| DEP-03 | Hosted model providers such as GPT, for the opt-in | CONFIRMED (as example) | A1-Extra. It is also a source of conflict, see CF-02. |
| DEP-04 | A legal or compliance owner to confirm DPDP obligations | INFERRED | CON-04. The owner is unnamed, see Q-008. |
| DEP-05 | A decision on CF-02 and CF-03 by Varthik, plus the legal or compliance owner | INFERRED | Section 2 conflict rule |
| DEP-06 | A private cloud provider in India, if that deployment is chosen | INFERRED | CON-02 |

## 8. Risks

| ID | Risk | Status | Likelihood / impact | Rationale |
|---|---|---|---|---|
| RSK-01 | The hosted-model opt-in could send data outside the premises or India and breach NFR-001, CON-03 or the DPDP Act. | INFERRED | High if the opt-in is used / high | CF-02. Cross-border legal effect not verified by this agent. |
| RSK-02 | Autonomy without per-action approval raises the impact of a misconfigured policy or a sandbox escape. | INFERRED. EV-01 (EXTERNAL, partially unverified) shows comparable projects offering confirmation modes and security analyzers; this spec does not adopt them. | Not estimable / high | A1-Q001 (c) |
| RSK-03 | DPDP obligations are misunderstood, because no one with legal authority has confirmed them. | INFERRED | Not estimable / high | CON-04, Q-008 |
| RSK-04 | Availability and scale numbers arrive late, and the later design has to be reworked. | INFERRED | Medium / medium | NFR-004, NFR-005 |
| RSK-05 | Local models are less capable than hosted ones, which pushes users toward the opt-in. | INFERRED | Not estimable / medium | FR-008, FR-009 |
| RSK-06 | "Private cloud in India" and "no data leaves the premises" are read differently, which causes non-compliance. | INFERRED | Not estimable / high | CF-03 |
| RSK-07 | Audit logs may hold personal data and so fall under the DPDP Act. | INFERRED | Not estimable / medium | FR-007, CON-04 |

## 9. Acceptance criteria

| ID | Requirement | Pass/fail test |
|---|---|---|
| AC-001 | FR-002 | Given a policy that permits action X, when an agent attempts X, then X executes and no human-approval prompt is raised. The list of actions X is [TBD, Q-014]. |
| AC-002 | FR-003 | Given a policy that does not permit action Y, when an agent attempts Y, then Y does not execute unless separately authorised. Whether it is blocked or escalated is [TBD, Q-014]. |
| AC-003 | FR-004 | An administrator creates or edits a policy, and the next agent action is evaluated against the new policy. A non-administrator cannot do this. |
| AC-004 | FR-005 | A user with a valid session at the organisation's IdP [TBD, Q-015] reaches the platform with no separate platform password. A user without a session is denied. |
| AC-005 | FR-006 | For each defined role [list TBD, Q-015], permitted operations succeed and non-permitted operations are denied. |
| AC-006 | FR-008 | With no hosted-model opt-in configured, a network capture over a reference agent run shows zero model-inference traffic to hosts outside the organisation's infrastructure. |
| AC-007 | FR-010 | An administrator switches an agent from model A to model B by configuration alone, and the reference task completes. The model list is [TBD, Q-010]. |
| AC-008 | NFR-001 | A network egress capture over a reference workload shows no organisational data leaving the "premises" boundary. The boundary is [TBD, Q-009]. |
| AC-009 | NFR-004 | Measured availability over [period TBD] is at least [target TBD]. |
| AC-010 | NFR-005 | The platform sustains [N users, M agents, K concurrent tasks, all TBD] with [latency TBD]. |
| AC-011 | CON-01 | The platform installs on a reference on-premises server [OS and hardware TBD] and passes a smoke test. |
| AC-012 | CON-03 | Inspection of data stores, compute locations and network egress shows that all data and processing are in India. |

Items with "none yet": FR-001 (Q-006), FR-007 (Q-016), FR-009 (Q-007), FR-011, NFR-002 (Q-019), NFR-003 (Q-011), CON-02 (Q-009), CON-04 (Q-008).

## 10. Traceability matrix

| Source | Requirement | Acceptance criterion |
|---|---|---|
| S0 "agentic harness" + A1-Q002 (c) | FR-001 | none yet |
| A1-Q001 (c) | FR-002, FR-003 (inferred), FR-004 | AC-001, AC-002, AC-003 |
| A1-Q005 (a) | FR-005, FR-006 | AC-004, AC-005 |
| A1-Q005 (b) | FR-007 | none yet |
| A1-Q004 | FR-008, NFR-001, CON-01, CON-02 | AC-006, AC-008, AC-011, none yet |
| A1-Extra | FR-009, FR-010 | none yet, AC-007 |
| S0 "model-agnostic" | FR-010 | AC-007 |
| S0 "privacy-preserving" | NFR-002 | none yet |
| S0 "local-first" | NFR-003 | none yet |
| A1-Q005 ("don't know yet") | NFR-004, NFR-005 | AC-009, AC-010 |
| A1-Q003 (a) | CON-03 | AC-012 |
| A1-Q003 (c) | CON-04 | none yet |
| EV-01 | FR-011 (candidate) | none yet |
| S0 "India's first" + A1-Q003 | none (marketing, not a requirement) | N/A |

## 11. Change log

| Version | Change |
|---|---|
| 0.1 | Draft after steps 1-5. S0 yielded only terms and no requirements. |
| 0.2 | Round-1 answers applied. |

Changes in v0.2:
- **Autonomy:** "fully autonomous" went from UNRESOLVED to CONFIRMED (A1-Q001 (c)). This created FR-002 and FR-004, plus INFERRED FR-003. CF-01 (potential) is resolved by the stakeholder's definition.
- **Agentic harness:** it went from UNRESOLVED to CONFIRMED as a governing platform (A1-Q002 (c)), creating FR-001. The word "govern" is newly open (Q-006).
- **Sovereign:**
  - It is CONFIRMED as data and processing in India (CON-03) plus DPDP compliance (CON-04).
  - "India's first" is CONFIRMED as marketing and dropped as a requirement.
- **Enterprise-grade:** partly defined by SSO and RBAC (FR-005, FR-006) and audit logs (FR-007). Availability and scale (NFR-004, NFR-005) stay UNRESOLVED.
- **Deployment:** CON-01 and CON-02 are created from A1-Q004.
- **Model-agnostic:** FR-008 and FR-009 are created, and they conflict with each other. CF-02 (actual) is logged, and CF-03 (potential) is also new.
- **Blocked by the new conflicts:** FR-008, FR-009, NFR-001, CON-02 and CON-03.

## 12. Effect of external evidence

**EV-01** (DeepWiki, `All-Hands-AI/OpenHands`, 2026-10-18, partially unverified because the answer names the project "Agent Canvas"):
- It shaped the options offered in Q-001 (approval modes) and Q-004 and Q-010 (local and Docker deployment, provider profiles).
- It produced the labelled candidate FR-011 (EXTERNAL) and a note in RSK-02.
- It changed no CONFIRMED requirement. A stakeholder answer outranks it (Section 8).
- **Legal topics:** no external evidence was consulted on the DPDP Act, because DeepWiki covers public code repositories and cannot answer a legal question.
