# Assumptions, Open Questions, Conflicts and Evidence Register

Run was non-interactive: no question was asked in chat; all are recorded as open. Suggested defaults are SUGGESTIONS, not applied.

## Assumptions

| ID | Statement | Why assumed | Risk if wrong | What would confirm it |
|---|---|---|---|---|
| ASM-01 | "Agentic harness" means a runtime that runs LLM-based agents with tools. | Common usage; consistent with EV-01 and EV-02. Not used to write any requirement. | The system may be something else (e.g., an agent test/evaluation harness); FR-001 meaning changes entirely. | Q-001 |
| ASM-02 | The idea is intended to become a real product built by a team, not only a pitch phrase. | "Let's make ..." phrasing. | Whole spec moot. | Q-001, Q-011 |
| ASM-03 | Varthik is the person who can decide the conflicts and open questions. | Input title names him as owner of the idea. | Decisions made by the wrong authority. | Q-011 |
| ASM-04 | "local-first" and "self-hosted" are meant as distinct attributes (not synonyms). | Both listed separately. | Duplicate or contradictory requirements (NFR-002 vs CON-01). | Q-004, Q-007 |

## Open questions

Format: options are 2 to 4 plus "other"; if left unanswered, dependants stay BLOCKED / NEEDS CLARIFICATION.

| ID | Question | Why it matters | Blocks | Who can answer | Suggested default (SUGGESTION, not applied) | Status |
|---|---|---|---|---|---|---|
| Q-001 | What does the "agentic harness" do and harness: (a) runtime orchestrating LLM agents with tools, (b) agent evaluation/test harness, (c) enterprise workflow platform, (d) other? Who are its users? | Decides scope of the entire spec. | FR-001, all | Varthik | (a), because EV-01/EV-02 products use the term this way | OPEN |
| Q-002 | What does "fully autonomous" mean: (a) never any human approval, (b) autonomous within a policy sandbox, escalating on risky actions, (c) autonomous for read-only actions, approval for writes, (d) other? | Autonomy level is a safety/security posture; also drives CF-01 and CF-03. | FR-002, NFR-003 (via CF-01), NFR-004 (via CF-03) | Varthik | (b) (EXTERNAL hint from EV-01 where autonomy and sandboxing are separate settings) | OPEN |
| Q-003 | What does "sovereign" mean: (a) data stored/processed in India, (b) Indian ownership and control of code and operation, (c) no dependency on foreign vendors, clouds or models, (d) other / combination? | Defines legal and architectural boundary; may be a constraint, not an NFR. | NFR-001 | Varthik, legal counsel | none, no basis | OPEN |
| Q-004 | What does "local-first" mean: (a) fully offline-capable, (b) local data primary with optional sync, (c) local model inference by default, (d) other? How does it differ from self-hosted? | Defines data/compute placement. | NFR-002 (also ASM-04) | Varthik | none | OPEN |
| Q-005 | Which "enterprise-grade" attributes are required (SSO/RBAC, audit log, availability target, compliance certification, support SLA) and with what thresholds? | Each attribute is a separate testable requirement. | NFR-003 | Varthik, target customers | none (no invented numbers) | OPEN |
| Q-006 | Which models must be supported: (a) any hosted provider, (b) local open-weight models only, (c) both hosted and local, (d) other? Are foreign-hosted models acceptable (see CF-02)? | Core of CF-02. | NFR-005, NFR-001, NFR-002, NFR-004 | Varthik | (c) with hosted optional (EXTERNAL hint from EV-01, EV-02) | OPEN |
| Q-007 | Who hosts and where: (a) customer on-prem, (b) customer cloud/VPC, (c) developer laptop, (d) Indian cloud provider, (e) other? Single or multi-tenant? | Deployment environment is a stakeholder decision. | CON-01 | Varthik | none (EV-02 shows local, Docker and VM variants exist) | OPEN |
| Q-008 | What does "privacy-preserving" mean: which data, protected from whom, under which law; (a) no data leaves the deployment, (b) encryption at rest/in transit, (c) data minimisation/anonymisation, (d) other? | Privacy posture; ties to CF-02 and CF-03. | NFR-004 | Varthik, legal counsel | none | OPEN |
| Q-009 | What does "India's first" mean: does "India's" mean built in India, by an Indian entity, or for India; and is "first" a requirement, a marketing claim, or a goal to be verified? | Possibly not a requirement; unverifiable claim creates RSK-03. | none directly (informs NFR-001) | Varthik | Treat as marketing claim, out of requirements | OPEN |
| Q-010 | What does "let's really push it" mean: (a) top priority, (b) aggressive timeline, (c) maximal ambition/scope, (d) other? | Gives priority and timeline; currently all items unprioritised. | none directly (priorities of all items) | Varthik | none | OPEN |
| Q-011 | Who are the users, operators and decision authority (including who resolves CF-01..CF-03)? | Actors incomplete; no conflict can be closed without an authority. | all (ASM-03) | Varthik | none | OPEN |
| Q-012 | Which regulations or legal obligations apply (e.g., data-protection or sector rules)? | "sovereign" and "privacy-preserving" may carry legal duties this analysis cannot verify. | NFR-001, NFR-004 | Legal counsel, Varthik | none; not assumed | OPEN |
| Q-013 | What are the budget, deadline, team size, expected scale (users, load) and success thresholds? | Constraints and feasibility check cannot be judged. | feasibility of all; CON category | Varthik | none | OPEN |

## Conflicts

All are "potential": they exist only because the terms are undefined. No winner is picked.

| ID | Competing claims | Sources | Inference | Dependants | Who decides |
|---|---|---|---|---|---|
| CF-01 (potential) | "fully autonomous" vs "enterprise-grade" | L3 both | Enterprise-grade commonly implies governance, approvals, audit; full autonomy implies no human gate. EV-01 shows autonomy and governance can coexist if defined as separate settings, so this depends on definitions. | FR-002, NFR-003 | Varthik (authority unconfirmed, Q-011) |
| CF-02 (potential) | "local-first", "sovereign", "privacy-preserving", "self-hosted" vs "model-agnostic" | L3 all | If model-agnostic includes third-party hosted models, prompts/data leave the local deployment, straining the other four. If it means local models only, it is a narrower reading. | NFR-001, NFR-002, NFR-004, NFR-005 (CON-01 not dependent) | Varthik |
| CF-03 (potential) | "fully autonomous" vs "privacy-preserving" / "sovereign" | L3 | An agent acting without approval and able to use tools or network could move data out of the deployment. Depends on sandbox/permission definition. | FR-002, NFR-001, NFR-004 | Varthik |

## External evidence

| ID | Server / tool | Arguments | Date | Repo | Summary | Raw file | Reliability |
|---|---|---|---|---|---|---|---|
| EV-01 | DeepWiki MCP `https://mcp.deepwiki.com/mcp`, `ask_wiki_question` | repoName `openai/codex`; question: how the harness defines autonomy levels (approval modes), sandboxing/permissions and model/provider selection | 2026-10-18 | openai/codex (judgment call: well-known public agent harness, chosen by agent since the input names no repository) | Autonomy is an approval policy (`untrusted`, `on-request`, `never`, plus granular) with an optional reviewer; sandbox/permission profiles are separate; model provider is a configurable abstraction supporting local or self-hosted models. | `ev_01.txt` (request, headers, raw SSE; also `ev_01_request.txt`, `ev_01_response.txt`) | Relevant. Answer is wiki-derived; inline citation markers appear stripped in raw text, so treated as background only. EXTERNAL, needs stakeholder confirmation. |
| EV-02 | same, `ask_wiki_question` | repoName `All-Hands-AI/OpenHands`; question: self-hosting, any/local LLM, data privacy, autonomy/confirmation modes | 2026-10-18 | All-Hands-AI/OpenHands (judgment call, same reason) | Self-hosting via local, Docker, VM or company infrastructure; works with any LLM; data stays in the deployment unless configured otherwise; optional confirmation mode. | `ev_02.txt` (also `ev_02_request.txt`, `ev_02_response.txt`) | Relevant but partially unverified: answer describes the product as "Agent Canvas", not checked against the repo. EXTERNAL; may motivate questions only. |

Discovery step: `initialize` (200 OK), `notifications/initialized` (202), `tools/list` (200) returned `ask_wiki_question`, `read_wiki_contents`, `read_wiki_structure` (raw: `init_resp.txt`, `initialized_resp.txt`, `tools_list_resp.txt`). No tool failures, no retries, no conflicting evidence, no instructions found in tool output.
