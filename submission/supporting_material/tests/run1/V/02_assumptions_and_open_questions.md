# Assumptions, Open Questions, Conflicts and Evidence
Date 2026-10-16. The user was not reachable (non-interactive), so no question was asked; all are recorded as open. Suggested defaults are suggestions only and are NOT applied.

## Assumptions
| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | The deliverable is a software product that others deploy, not a one-off internal script | "enterprise-grade", "self-hosted" | Wrong scale and audience | Q-010 answer |
| ASM-02 | "Model-agnostic" means AI/LLM models | "agentic" context | Misread; "model" could mean data or process model | Q-007 answer |
| ASM-03 | "Sovereign" relates to India (data/jurisdiction/control) | "India's first sovereign" | Could mean technical independence only | Q-002 answer |
| ASM-04 | This spec stays at requirements level; no design or code | Skill boundary | n/a | n/a |

## Open questions
| ID | Question | Why it matters | Requirements blocked | Who can answer | Suggested default (NOT applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | What is the "harness"? Options: (a) runtime that executes LLM agents with tools; (b) orchestration framework for multi-agent workflows; (c) test/evaluation harness for agents; (d) SDK/library; (e) other. What tasks/domains will agents perform? | Defines the entire functional scope | FR-001, all future FRs | Varthik / product owner | (a), SUGGESTED, unconfirmed | OPEN |
| Q-002 | What does "sovereign" mean? Options: (a) data and processing stay in India; (b) Indian-owned and controlled code and operations; (c) no dependency on foreign services; (d) compliance with specific Indian policy; (e) other. | Constraints on hosting, models, supply chain | NFR-001, FR-002, FR-003 | Varthik, legal/compliance | none | OPEN |
| Q-003 | What does "local-first" mean? Options: (a) works fully offline; (b) data stored on user device first, sync optional; (c) inference runs on local hardware; (d) other. | Conflicts with hosted models if (a)/(c) | FR-004, FR-002 | Varthik | none | OPEN |
| Q-004 | Which "enterprise-grade" attributes, with thresholds? Options: availability %, SSO/RBAC, audit logging, scalability (users/agents), support/SLA, certifications, other. | Largest source of NFR scope and cost | NFR-002 | Varthik, target enterprise customers | none | OPEN |
| Q-005 | What does "privacy-preserving" mean? What data is sensitive, who must not see it, and what is the threat model? Options: no data leaves the host; encryption at rest/in transit; PII redaction before model calls; compliance with a law; other. | Sets security/privacy posture | NFR-003 | Varthik, privacy/legal | none | OPEN |
| Q-006 | What does "fully autonomous" mean? Options: (a) no human approval ever; (b) runs unattended but with audit log and kill switch; (c) approval required only for high-risk actions (EV-01 shows such a mode); (d) other. What actions must never be autonomous? | Autonomy level is a mandatory-ask item; safety and privacy impact | FR-005, NFR-002, NFR-003 | Varthik, risk owner | none (EV-01 option (c) is only an example) | OPEN |
| Q-007 | Which models must be supported? Options: local open-weight only; local plus hosted APIs; specific named models/providers; other. | Drives sovereignty/privacy tension (RSK-03) | FR-002 | Varthik | none | OPEN |
| Q-008 | What is the target deployment environment? Options: single container/server; on-premise cluster; private cloud in India; air-gapped; desktop; other. | Defines constraints and feasibility | FR-003, FR-004 | Varthik, operations | none | OPEN |
| Q-009 | What do "India's first" and "really push it" mean in measurable terms (first by what criterion; ambition vs deadline vs feature breadth)? | Possibly a success measure or schedule driver; currently not a requirement | none yet (would shape priorities) | Varthik | treat as non-requirement | OPEN |
| Q-010 | Who are the users and operators, how many, and what is Varthik's role? | Intended users is a mandatory-ask item | ACT-02, ACT-03, ASM-01, all NFRs | Varthik | none | OPEN |
| Q-011 | Deadline, budget, team, and existing system/prior art? | Feasibility cannot be checked without them | feasibility of all | Varthik / sponsor | none | OPEN |
| Q-012 | Which regulations apply (e.g. Indian data protection law, sector rules)? | Compliance NFRs | NFR-001, NFR-003 | Legal/compliance | none (INFERRED that Indian data-protection rules are likely relevant; unverified) | OPEN |
| Q-013 | If local/sovereign/private conflicts with model-agnostic or autonomy, which wins? | Resolves RSK-02, RSK-03 | FR-002, FR-004, FR-005, NFR-001 to NFR-003 | Varthik | none | OPEN |
| Q-014 | Is any use safety-critical, legal, medical or financial (agents acting on real systems)? | Escalation trigger | all | Varthik | none | OPEN |

Top 5 for a first round: Q-001, Q-006, Q-002, Q-005, Q-007. If unanswered, scope stays unknown and the spec stays not implementation-ready.

## Conflicts
No source-versus-source conflict exists (one source). Internal tensions are logged as risks, not resolved:

| Competing claims | Sources | Who decides |
|---|---|---|
| "fully autonomous" vs "enterprise-grade" / "privacy-preserving" (oversight and audit expectations) | same sentence in `V_varthik.md` (tension, not yet a proven conflict) | Varthik (Q-013) |
| "local-first / sovereign / self-hosted" vs "model-agnostic" (hosted foreign models) | same sentence (tension, not yet a proven conflict) | Varthik (Q-013) |

## External evidence
| ID | Server / tool | Arguments | Date | Result summary | Repo | Status / effect |
|---|---|---|---|---|---|---|
| EV-01 | DeepWiki MCP (`https://mcp.deepwiki.com/mcp`) / `ask_wiki_question` | repoName `All-Hands-AI/OpenHands`; question: how the project defines and configures agent autonomy levels, permissions/confirmation of actions, and self-hosted deployment | 2026-10-16 | Wiki answer: autonomy is governed by a boolean `confirmation_mode` (pause for confirmation before high-risk actions) plus a `security_analyzer` setting (LLM-based or disabled); deployment modes include all-in-one Docker, headless self-hosted VM with API-key header auth, and Electron desktop; recommends firewall restrictions and encrypted secrets. Raw output: `ev_ask_openhands.txt`. | All-Hands-AI/OpenHands | EXTERNAL. Not independently verified beyond the wiki. Used only to shape Q-006, Q-008, RSK-05. Note: the repo was chosen by the agent because the input names none; the wiki also calls the product "Agent Canvas". |
