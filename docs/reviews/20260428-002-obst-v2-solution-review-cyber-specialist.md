---
agent: Cyber Specialist
document_reviewed: docs/project-outline.md
date: 2026-04-28
---

### Summary

This document proposes three candidate solutions to an AI-assisted project governance problem within a UK Government division: OBST+agenteam (a bespoke framework and CLI tool wrapping Claude Code), Hamster (a US-domiciled SaaS product), and an AI Playbook-aligned Prompt and Template Library. The primary new material in v2 is the introduction of agenteam as an open-source CLI tool, the identification of Mistral Vibe as a potential future platform, and the addition of the Prompt and Template Library as a third option. This review focuses on the security implications of those additions, with particular attention to supply chain risk, assurance claims, and data handling. The prior v1 gaps — no data classification ceiling, no threat model, OFFICIAL-SENSITIVE artefact risk, OAuth blast radius, and prompt content sensitivity — remain unresolved in v2 and are noted where relevant but not repeated in full.

---

## Solutions

---

## OBST+agenteam

### Strengths

- Restricting agenteam's scope to OBST processes limits attack surface compared to a general-purpose agent platform. A narrow tool is easier to threat-model and assure.
- Using Claude Code as the near-term backend, if its assurance status is confirmed, avoids the need for new procurement and keeps data processing within a known boundary.
- Open-sourcing the codebase means the code can be subject to independent security review and community scrutiny, which is consistent with NCSC guidance on transparency for shared tooling.
- The framework explicitly references the UK Government AI Playbook and GDS Service Standard, indicating awareness of the governance environment.

### Issues

#### Assurance & Procurement

1. **What** — The document states "Claude Code is assured for UK government use" without citation, qualification, or reference to a specific assurance mechanism (e.g. NCSC CPA, DSPT, G-Cloud listing, or departmental security review).
   **Why it matters** — Web research for this review found no publicly verifiable evidence of Claude Code holding a formal UK Government assurance. The US-facing Anthropic page references FedRAMP High and IL5, not UK equivalents. If the claim is incorrect or based on informal approval, the entire agenteam deployment route is unsupported from a procurement and security baseline perspective.
   **Question / Decision required** — What is the specific assurance basis for Claude Code? Options: (a) Formal NCSC or departmental security assessment — confirm and document it; (b) G-Cloud or Crown Commercial Service listing — verify currency and scope; (c) Informal departmental sign-off — this is insufficient; a formal assessment should be commissioned before proceeding. The outline must not proceed to Brief stage with an unverified assurance claim as its foundation.

2. **What** — Mistral Vibe is named as a potential future platform with no assurance or procurement assessment, and the document notes only that "Vibe would require assurance and procurement."
   **Why it matters** — Naming an unassessed platform in a project outline, even prospectively, creates a de facto roadmap that development decisions may start to accommodate. Web research confirms Mistral Vibe publishes no UK government compliance information. If development begins with a "future switch" in mind, design choices (e.g. abstraction layers, data schemas) may inadvertently favour a platform that never achieves assurance.
   **Question / Decision required** — Should Mistral Vibe be removed from the outline entirely until a formal assurance assessment is completed? Options: (a) Remove it and revisit if and when assessed — lowest risk; (b) Retain it with an explicit caveat that no design decisions should accommodate it — acceptable if the caveat is binding; (c) Commission an assurance assessment in parallel — only appropriate if there is genuine intent and resource to do so.

#### Supply Chain & Individual Ownership

3. **What** — Both OBST and agenteam would be "owned by the developer (i.e. me, Matthew Roberts)" as open-source projects, with a possible future steering group from AAAI.
   **Why it matters** — Individual ownership of a tool used by UK Civil Servants for governance-critical processes creates significant supply chain risk. The individual could: change the tool's behaviour, introduce a malicious dependency, abandon the project, be subject to a security incident affecting their personal accounts, or have their GitHub/npm/PyPI credentials compromised. There is no legal agreement, SLA, or formal security responsibility. This mirrors the risk profile of the xz-utils backdoor (2024): a trusted individual maintainer of widely-used infrastructure.
   **Question / Decision required** — Before proceeding, the ownership and governance model must be resolved. Options: (a) Crown ownership — the tool is developed under employment contract and owned by the department, eliminating personal supply chain risk; (b) Formal open-source governance from day one — a recognised foundation or multi-maintainer model with signed commits, dependency review, and a clear security disclosure policy; (c) Accept individual ownership with a documented risk register entry — only appropriate if the tool's blast radius is tightly bounded and it handles no sensitive data. The current description is inconsistent with the tool's intended use on government processes.

4. **What** — agenteam is described as wrapping a "library of AI subagents, skills and prompts." The supply chain of this library — including any third-party prompt libraries, npm/PyPI dependencies, or upstream Claude Code SDK versions — is not described.
   **Why it matters** — Each dependency in the chain is a potential vector for a supply chain attack. A compromised dependency could exfiltrate the content of prompts and project documents passed through the tool, including anything at OFFICIAL-SENSITIVE. NCSC guidance on software supply chain (2021) explicitly flags this risk for tools with network-connected dependencies.
   **Question / Decision required** — What is the dependency policy for agenteam? Decisions required: (a) Will a Software Bill of Materials (SBOM) be maintained? (b) Will dependencies be pinned and reviewed against known vulnerability databases? (c) Will the Claude Code SDK version be tracked and updated under a defined patch management process?

#### Data Classification & Prompt Security

5. **What** — agenteam will process project outline documents to produce governance artefacts. These documents may contain project intent, architecture decisions, threat assessments, and resourcing information — all potentially OFFICIAL-SENSITIVE. The outline does not state a data classification ceiling for the tool.
   **Why it matters** — If the tool is used on OFFICIAL-SENSITIVE material, the entire data path — from the user's terminal, through the Claude Code API, to Anthropic's infrastructure — must be assessed at that classification level. This was raised as a gap in v1 and remains unresolved. The assurance claim for Claude Code (Issue 1 above) is directly relevant here: assurance at OFFICIAL is meaningfully different from assurance at OFFICIAL-SENSITIVE.
   **Question / Decision required** — What is the maximum classification of documents that will be processed by agenteam? This must be decided before the tool is used in any live context. Options: (a) OFFICIAL only — document this as an explicit constraint in the tool's README and governance; (b) OFFICIAL-SENSITIVE — requires a higher assurance bar; verify Claude Code's assurance covers this tier; (c) No ceiling stated — unacceptable; this must be resolved before proceeding.

6. **What** — The prompts and agent definitions used by agenteam are themselves potentially sensitive. They encode the division's review logic, specialist questions, internal thresholds, and governance processes.
   **Why it matters** — Open-sourcing these prompts exposes the division's internal security review methodology to adversaries, who could use them to understand what questions reviewers ask (and therefore what they do not ask), reverse-engineer detection logic, or tailor proposals to pass review. This is a policy leakage risk as defined in UK Government AI guidance.
   **Question / Decision required** — Which components of agenteam will be open-sourced? Options: (a) Framework only (scaffolding, CLI structure) — prompts and agent definitions remain internal; (b) All components including prompts — requires a deliberate decision that the division is comfortable with full methodology disclosure; (c) A tiered model — generic prompts open-sourced, sensitive specialist prompts held internally. This decision must be made before the open-source licensing is applied.

#### Access Control

7. **What** — The document does not describe how agenteam authenticates to Claude Code, how API keys are managed, or what access controls govern who can run agents against which documents.
   **Why it matters** — Misconfigured API key management is a leading cause of cloud data exposure. If API keys are shared, embedded in the repository, or managed informally, they create a single point of compromise for all data processed through the tool. This risk is compounded by individual ownership (Issue 3).
   **Question / Decision required** — How will API credentials be managed? Decisions required: (a) Will credentials be per-user or shared? (b) Will there be a secrets management policy (e.g. prohibition on committing keys, use of a secrets vault)? (c) Will audit logging of API calls be required?

### Open Questions

- Has a Data Protection Impact Assessment (DPIA) been considered for agenteam, given it will process Civil Service project data via a third-party AI API?
- What is the incident response plan if agenteam is compromised or a malicious commit is introduced?
- Is there a plan to conduct a penetration test or code security review of agenteam before departmental use?
- Does the Claude Code assurance (if it exists) extend to the agenteam wrapper, or does wrapping it create a new surface requiring separate assessment?

---

## Hamster

### Strengths

- The document correctly identifies Hamster as unsuitable for UK Government use due to its US-domiciled SaaS nature and the procurement burden this entails. This is an accurate risk assessment and the document is appropriately dismissive of this option.

### Issues

#### Assurance & Procurement

1. **What** — The document describes Hamster's functionality in detail while simultaneously stating it is "highly unlikely ever to be assured for UK government use." The level of description given to an excluded option risks creating a false equivalence in subsequent discussions.
   **Why it matters** — Decision-makers who read only summaries may not register the exclusion. Describing a non-viable option in detail without a clear up-front exclusion marker could lead to scope creep or informal use outside the approved framework.
   **Question / Decision required** — Should Hamster be removed from the outline or relegated to a footnote? Options: (a) Remove it entirely — cleanest, avoids confusion; (b) Retain as a "market reference" with a prominent exclusion statement at the top of its section — acceptable if the exclusion is unambiguous.

#### Supply Chain & Third-Party Risks

2. **What** — No assessment is provided of what data Hamster would process, how it is stored, or under what legal jurisdiction, beyond noting it is US-domiciled.
   **Why it matters** — If Hamster is ever reconsidered (e.g. if it achieves a UK assurance), the absence of a data flow analysis means the risk is invisible rather than managed.
   **Question / Decision required** — For completeness: if Hamster is retained as a reference option, a brief note should be added that no data flow or legal assessment has been conducted, and that any future consideration would require a full DPIA and security assessment from scratch.

### Open Questions

- Is there any UK-domiciled equivalent to Hamster that would have a more tractable procurement path?

---

## AI Playbook-aligned Prompt and Template Library

### Strengths

- This option has the lowest attack surface of the three: no custom software, no new API endpoints, no supply chain dependencies beyond existing assured tools. This is its principal security advantage.
- Using only tools already procured and assured (e.g. Microsoft Copilot, Claude for Government) means the security baseline is inherited from existing assessments.
- Open-sourcing only document templates and prompts — not software — eliminates the software supply chain risk entirely.
- No API key management, no agent orchestration, no dependency tree: the security model is simple and auditable.
- Aligns with the principle of least privilege at the tooling level: no new capabilities are introduced.

### Issues

#### Data Classification & Prompt Security

1. **What** — The same prompt content sensitivity risk identified for agenteam (OBST Issue 6) applies here: open-sourcing prompts aligned to the AI Playbook stages exposes the division's governance review methodology.
   **Why it matters** — While the risk is lower than for agenteam (no automation, no agent logic), the prompts for a privacy impact assessment or AI risk assessment will encode the questions the division deems important — and by implication, those it does not ask.
   **Question / Decision required** — Will the prompt library be fully open-sourced? If yes, has the division accepted the policy leakage risk? Options: (a) Full open-source with awareness of disclosure — defensible if the prompts are deliberately generic; (b) Internal-only with selective sharing — reduces risk but limits community benefit; (c) Tiered: generic templates open, sensitive review logic internal.

#### Compliance & Regulatory Gaps

2. **What** — The option states that "governance alignment and maintenance would depend on regular review against current policy," but no review cadence, owner, or assurance mechanism is specified.
   **Why it matters** — A prompt library that drifts from current policy (e.g. following updates to the AI Playbook, GDPR guidance, or NCSC frameworks) could actively mislead users into believing their artefacts are compliant when they are not. This is a governance risk with direct regulatory consequences.
   **Question / Decision required** — Before adopting this option, a maintenance model must be defined. Decisions required: (a) Who owns the library and is accountable for its currency? (b) What is the review cadence (e.g. quarterly, triggered by policy updates)? (c) How will users be notified of changes or deprecations?

3. **What** — The option references "Microsoft Copilot, Claude for Government" as the target tools, but does not verify that these tools are currently on contract and available to AAAI users.
   **Why it matters** — If a team begins developing artefacts using a tool that is not actually procured for their specific department or security domain, this creates an unmanaged data handling incident rather than a governed process.
   **Question / Decision required** — Before proceeding, confirm: (a) which specific AI tools are currently procured and available to AAAI; (b) at what classification level each tool is approved; (c) whether "Claude for Government" is distinct from Claude Code and if so what its assurance basis is.

### Open Questions

- Does this option require any security assessment at all before deployment, given it relies entirely on existing tooling? If so, what is the minimum assurance bar?
- Who would be responsible for the DPIA for this option, given the templates will guide users in processing potentially sensitive project data through third-party AI tools?
- Could this option serve as a lower-risk interim while OBST+agenteam undergoes formal assurance? The outline does not consider phased adoption.
