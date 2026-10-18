# Dev/Design Agency Positioning (Ask HN) - Requirements Specification: DECLINED

Version 0.1 | Date 2026-10-18 | Author: agent (requirements-spec skill) | Source documents: `T5_hn_non-software-scope-test.md` (Ask HN post, https://news.ycombinator.com/item?id=43113083, author SouravInsights)

**Result: decline set. No requirements are written for this input.**

## 1. Problem restatement and scope

**Restatement (2 lines).** A developer asks the Hacker News community how to start and position a small (3-4 person) dev/design agency in the age of AI, mixing client work with own products and open source. The post is a request for business advice and experience reports, not a description of a system to build.

**Why the skill does not apply (skill Section 1, "Borderline inputs: mostly not software").**
- The post's four explicit questions are all about business strategy: positioning ("How would you position an agency today with AI in the picture?"), running client work alongside own products, which work AI will not replace, and growing through open source.
- The "Here's what I'm thinking" and "What I want to avoid" lists describe company strategy and culture (team size, client selection, avoiding the "digital transformation" agency model). They contain no system behaviour, users, data or interfaces.
- It is not a "mixed input": no system to specify is described in the post. The only software artefact is mentioned in passing (see below).

**Software-related part (if any), recorded as an open question, not as a requirement.**
- The author's side project: "an open-source form builder platform like Typeform" (CONFIRMED as wording only; no features, users, scale or constraints are stated). See Q-001.

| Scope class | Content |
|---|---|
| In scope | None (declined). |
| Out of scope | All questions asked in the post: agency positioning, mixing client work with own products, AI-resistant work types, open-source-led growth, and the "Anyone here managed to..." experience requests. These are business questions, not answered here. |
| Unknown scope | The open-source form builder (Q-001); whether the author wants a spec for it at all. |

**What input would make the skill applicable.** A description of a software system to be built, for example the form builder: who fills in and who builds forms, what a form can contain, where responses go, how it is hosted, and what is required beyond "like Typeform". Alternatively, a software need arising from the agency (internal tooling, client portal), stated as a need. See Q-001 to Q-003 in `02_assumptions_and_open_questions.md`.

**Sections 2 to 12 of the standard template are not produced** (declined input; the skill says never write requirements for a declined input). Glossary, actors, FR, NFR, constraints, dependencies, risks, acceptance criteria and traceability are intentionally absent. Section 12 (effect of external evidence): none, no external evidence consulted. Change log: v0.1, 2026-10-18, initial decline set.
