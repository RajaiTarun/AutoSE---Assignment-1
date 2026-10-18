# Assumptions, Open Questions, Conflicts and Evidence

Date 2026-10-18 | Version 0.2 | Companion to `01_requirements_spec.md`

## Assumptions

| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | "Admin" is a single role that is distinct from ordinary users. | A1-Q001 and A1-Extra use "admin" without tiers. | Several admin tiers would change FR-004, FR-006 and FR-009. | Varthik's answer to Q-015. |
| ASM-02 | "Agents" means LLM-driven software agents. | S0 and A1-Q002. | The scope of FR-001 would change. | Varthik's answer to Q-017. |
| ASM-03 | "DPDP Act" means India's Digital Personal Data Protection Act. | A1-Q003 (c) gives only the abbreviation. | The wrong legal instrument would be treated as the target. | Varthik or the legal owner (Q-008). |
| ASM-04 | One deployment serves one organisation (single-tenant). | "across an organisation" (A1-Q002) and on-premises hosting. | Multi-tenancy would change the security, access and data-separation requirements. | Varthik's answer to Q-017. |

## Open questions

Status key: ANSWERED = closed by a stakeholder answer. PARTIAL = answered, with residual questions listed. OPEN = not yet asked. The "who" column names the likely owner.

| ID | Question | Why it matters | Requirements blocked | Who can answer | Suggested default (NOT applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | What does "fully autonomous" mean? | Autonomy level and security posture. | none now | Varthik | n/a | ANSWERED: (c), A1-Q001. Residual question in Q-014. |
| Q-002 | What is the "agentic harness", and what is it for? | Scope. | none now | Varthik | n/a | PARTIAL: (c), A1-Q002. Residual questions in Q-006 and Q-017. |
| Q-003 | What does "sovereign" mean, and does "India's first" need to be demonstrable? | Compliance, dependencies and licensing. | none now | Varthik | n/a | ANSWERED: (a) and (c), "first" is marketing (A1-Q003). Residual question in Q-008. |
| Q-004 | Where does it run, and may it send data to outside models? | Deployment, privacy posture, and the model-agnostic conflict. | NFR-001, FR-008, CON-02 | Varthik | n/a | ANSWERED, but it created CF-02 and CF-03. Residual questions in Q-007 and Q-009. |
| Q-005 | What does "enterprise-grade" require? | Many NFRs. | none now | Varthik | n/a | PARTIAL: (a) and (b) answered, availability and scale "don't know yet". Residual questions in Q-012 and Q-013. |
| Q-006 | What does "govern" cover for agents? Options: (a) registry and lifecycle, (b) policy enforcement, (c) monitoring and audit, (d) quotas and cost control, (e) other. | The meaning of the central requirement. | FR-001 | Varthik | none | OPEN (round-2 priority) |
| Q-007 | CF-02: how should "only local models, no data leaves the premises" be reconciled with "an admin may opt in to hosted models such as GPT"? Options: (a) opt-in is a deliberate, documented exception to NFR-001, (b) hosted models allowed only if hosted in India, (c) drop the opt-in, (d) other. | Decides what is built, and whether the legal posture holds. | FR-008, FR-009, NFR-001, CON-03, RSK-01 | Varthik, plus the legal or compliance owner | none. Resolving this is not the agent's call. | OPEN (round-2 priority) |
| Q-008 | Which DPDP obligations apply, and who is accountable for confirming them? | Legal obligation this agent cannot verify. | CON-04, NFR-002 | A named legal or compliance owner (unnamed) | none | OPEN (round-2 priority, escalation) |
| Q-009 | CF-03: what is the "premises" boundary, and does a private cloud in India count as inside it? | A private cloud and "no data leaves the premises" may be incompatible. | CON-02, NFR-001, AC-008 | Varthik | none | OPEN (round-2 priority) |
| Q-010 | Which local models, runtimes and hardware must be supported? Do agents need configurable model profiles? | Defines "model-agnostic" and the deployment environment. | FR-010, CON-01, DEP-02 | Varthik, or whoever owns infrastructure | none | OPEN |
| Q-011 | Must the platform work fully offline or air-gapped? | The meaning of "local-first". | NFR-003 | Varthik | none | OPEN |
| Q-012 | What are the availability and scale targets (users, agents, concurrent tasks, uptime)? | Placeholders for NFR-004 and NFR-005. | NFR-004, NFR-005 | Varthik said "don't know yet". Needs a sizing owner or IT operations. | none, to avoid inventing numbers | OPEN, stakeholder cannot answer yet |
| Q-013 | Are compliance standards or certifications needed (for example ISO 27001 or SOC 2)? | Option (d) of Q-005 was not selected, which is not the same as excluded. | Possible NFR / CON | Varthik | none | OPEN |
| Q-014 | What can an admin policy restrict (tools, files, network, data classes)? What happens on an out-of-policy action: block, log, or escalate to a human? | Defines the autonomy boundary. | FR-002, FR-003, FR-004, FR-011 | Varthik | none | OPEN (round-2 priority) |
| Q-015 | Which SSO protocol and IdP, and which roles exist? | Makes FR-005 and FR-006 testable. | FR-005, FR-006 | Varthik, or IT | none | OPEN |
| Q-016 | What must audit logs contain, how long are they kept, must they be tamper-evident, and who reads them? | Makes FR-007 testable. | FR-007, RSK-07 | Varthik, plus the compliance owner | none | OPEN |
| Q-017 | Who are the intended users, what do the agents do (the domain), and is it single-tenant? | Scope and intended users. | FR-001, ASM-02, ASM-04 | Varthik | none | OPEN |
| Q-018 | What are the priorities, timeline and budget? What counts as success for "really push it"? | Prioritisation, and a success measure. | All priorities | Varthik | none | OPEN |
| Q-019 | What more does "privacy-preserving" need beyond no egress and DPDP compliance (for example encryption, PII handling, retention)? | Defines NFR-002. | NFR-002 | Varthik, plus the compliance owner | none | OPEN |

**Round-2 priority set (at most 5):** Q-007, Q-009, Q-008, Q-014, Q-006.

## Conflicts

| ID | Competing claims | Sources | Type | Dependents | Who decides |
|---|---|---|---|---|---|
| CF-01 | "Fully autonomous" against "enterprise-grade" and "privacy-preserving". | S0 | Potential. RESOLVED by A1-Q001 (c): autonomy is bounded by an admin-set policy. The residual question is in Q-014. | none now | Varthik (decided) |
| CF-02 | "Only local models, no data leaves the premises" against "an admin can opt in to hosted models such as GPT". Hosted models may also break "data and processing stay in India" (CON-03). | A1-Q004 against A1-Extra (both Varthik). CON-03 comes from A1-Q003 (a). | Actual, inside one stakeholder's answers. No winner is picked. | FR-008, FR-009, NFR-001, CON-03 (all BLOCKED), RSK-01 | Varthik, plus the legal or compliance owner for the DPDP effect (Q-007) |
| CF-03 | "Runs on a private cloud in India" against "no data leaves the premises". | A1-Q004 (both parts) | Potential, arising from the undefined term "premises". Inference: a private cloud may count as a third party's infrastructure. | CON-02, NFR-001 (both BLOCKED) | Varthik (Q-009) |

## External evidence

**EV-01**
- **Server and tool:** DeepWiki MCP (`https://mcp.deepwiki.com/mcp`, server v2.14.3), tool `ask_wiki_question`.
- **Discovery:** `initialize` and `tools/list` were run and returned `ask_wiki_question`, `read_wiki_contents` and `read_wiki_structure`. Raw files: `init_raw.txt` and `tools_list_raw.txt`.
- **Arguments:** `repoName`: `All-Hands-AI/OpenHands`. `question`: how an agent harness defines and controls autonomy, how it supports model-agnostic providers, and what self-hosted deployment and sandboxing it offers.
- **Date:** 2026-10-18.
- **Repo choice:** the input names no repository, so this was the agent's judgment call. OpenHands is a well-known open-source agent platform.
- **Summary of the result:**
  - Autonomy is controlled by a confirmation mode and a security analyzer (LLM-based, pattern-based or policy).
  - Model choice is via provider profiles (model, base URL, key) and external agents.
  - Deployment options are a local service, a Docker sandbox and a self-hosted VM.
  - There is no granular permission system, only enabled and disabled skills.
- **Caveat:** the answer calls the project "Agent Canvas" while citing OpenHands wiki pages, so repo identity is uncertain. It is marked partially unverified: it may motivate questions and candidate options, never a requirement.
- **Raw files:** `ev_01_request.txt` and `ev_01_response.txt`.
- **Effect on the spec:** see `01` section 12.
