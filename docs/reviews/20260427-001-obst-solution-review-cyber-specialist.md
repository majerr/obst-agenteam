---
agent: Cyber Specialist
document_reviewed: docs/project-outline.md
date: 2026-04-27
---

### Summary

The AAAI division is proposing a framework (OBST) and two supporting solutions to accelerate AI product delivery by UK Civil Servants, reducing time spent on non-data-science concerns such as governance, infrastructure, and DevOps. The three candidate solutions are: a custom-built OBST framework backed by a repository of governance artefacts; Hamster, a US-based commercial SaaS for AI-native product planning; and a prompt and template library for use with already-procured AI tools. The document acknowledges that any tooling must be security-assured and procurable but provides no evidence that this constraint has been evaluated for any of the three options. Several solutions introduce material risks — particularly around data classification, third-party data processing, and unverified assurance claims — that must be resolved before a solution is selected.

---

## Solutions

---

### OBST

#### Strengths

- The stage-gate structure with defined inputs, outputs, and sign-off before progression is consistent with secure-by-design delivery and with NCSC's principle of building security in rather than retrofitting it.
- Explicit production of an audit trail and decision records is a positive security property. If correctly implemented with integrity controls, it creates non-repudiation for governance decisions — a control frequently absent in data science delivery.
- Technology-agnosticism is a genuine strength: it avoids locking the framework's security posture to any single AI vendor, and means the framework can be operated at different sensitivity levels by choosing appropriate underlying tooling.

#### Issues

**Threat Model Gaps**

1. **What** — No threat model exists for OBST. The document does not identify threat actors, assets at risk, or trust boundaries between the framework, its tooling, and the teams using it.
   **Why it matters** — Without a threat model there is no basis for evaluating whether proposed controls are sufficient. Key threat actors not addressed include: malicious insiders, compromised AI tool providers, supply chain attackers targeting tooling dependencies, and external actors targeting government system design information. The audit trail that is a primary stated benefit is itself a high-value target.
   **Question / Decision required** — A threat model must be produced before OBST is deployed on real projects, covering at minimum: asset inventory (what data OBST holds), trust boundaries (where data crosses organisational or system boundaries), and principal threat actors.

2. **What** — The document states AI tools will "challenge ideas, review suggestions, generate artefacts, and manage issues" but does not specify which AI tools, what data they will receive, or what retention and logging applies to prompts and outputs.
   **Why it matters** — When a team member passes a project spec or security decision record to an AI system, that content leaves team control. If the AI tool is not correctly configured (e.g., data used for model training, or processed outside UK/EEA jurisdiction), this constitutes a data breach under UK GDPR and potentially a security incident. The act of asking an AI tool to generate a security design also reveals the existence and shape of the system being designed — a policy leakage risk.
   **Question / Decision required** — Per AI tool used within OBST: (a) Are data processing terms compatible with the classification of artefacts being processed? (b) Is UK data residency required and provided? (c) Is "no training on customer data" contractually guaranteed and independently verified? This must be documented before any pilot with real project content.

**Data Classification & Handling Risks**

3. **What** — No data classification has been assigned to OBST artefacts, and no classification ceiling has been defined for the framework. Briefs, specs, and decision records describing government AI systems may be OFFICIAL-SENSITIVE or higher.
   **Why it matters** — Classification determines where artefacts can be stored, which tools can process them, and what access controls apply. Designing and piloting OBST without resolving this means the repository may ingest content above its approved handling level before anyone notices.
   **Question / Decision required** — The team must decide:
   - Option A: OBST is limited to OFFICIAL (non-sensitive) artefacts — simpler to assure, restricts use cases.
   - Option B: OBST supports OFFICIAL-SENSITIVE — requires restricted toolchain, approved storage, and potentially additional DPA controls; significantly more complex to assure.
   - Option C: Classification ceiling left to each team — creates inconsistent risk exposure across the division and is not recommended.

4. **What** — The document does not address audit record storage location, retention period, or access controls. Audit records that reveal which systems were being designed, what security decisions were taken, or what vulnerabilities were identified are high-value targets.
   **Why it matters** — Storing audit records in an inadequately controlled repository creates a secondary attack surface. The audit trail cited as a primary framework benefit becomes a liability if it can be accessed, modified, or exfiltrated.
   **Question / Decision required** — Where will audit records be stored? Options: (a) departmentally-managed access-controlled repository (e.g., Azure DevOps within the government tenancy) with defined access control policy — acceptable; (b) third-party SaaS without explicit government approval — not acceptable for OFFICIAL-SENSITIVE content.

**Identity/Access Risks**

5. **What** — There is no access control model for the OBST repository. The framework's value depends on sign-off gates, but sign-off is meaningless if any team member can modify artefacts after sign-off.
   **Why it matters** — Governance artefacts that can be retroactively altered undermine the audit trail the framework is designed to produce. This is a tampering risk and a compliance risk (misleading governance records).
   **Question / Decision required** — The team must define: (a) whether repositories will enforce branch protection and signed commits; (b) who holds approval rights at each stage gate and whether these are enforced technically (e.g., required reviewers in the VCS) or only by process. Process-only controls are insufficient for a governance audit trail.

**Compliance & Regulatory Gaps**

6. **What** — The document references no specific assurance frameworks or compliance requirements that OBST must satisfy. For a UK Government tool handling governance artefacts and interacting with AI systems, the following are likely to apply: GovAssure, NCSC AI Security Principles (2023), UK GDPR (where artefacts include personal data, e.g., user research outputs), and the CDDO AI Assurance Framework.
   **Why it matters** — Deploying OBST without aligning to these standards means the division may be required to remediate or withdraw it later, potentially after sensitive artefacts have already been created and stored.
   **Question / Decision required** — Confirm which assurance frameworks apply and document a compliance mapping before OBST is deployed at scale. NCSC's guidance on securing AI systems should be reviewed and applied to any AI-integrated configuration.

**Irreversible or Expensive Security Decisions**

7. **What** — If OBST artefacts are committed to a repository without classification controls from the outset, it will be extremely difficult to subsequently identify and remove sensitive content. AI-generated artefacts may contain inferred sensitive information not obviously classifiable at creation time.
   **Why it matters** — Retroactive classification review of a repository containing months of AI-assisted project artefacts is expensive, unreliable, and may miss sensitive content. This is an irreversible decision if not designed correctly from the start.
   **Question / Decision required** — Define data handling and classification policy for OBST artefacts before any pilot on real projects, and build classification labelling into artefact templates from day one.

#### Open Questions

- If AI is used to "manage issues" (as stated in the document), does this mean the AI system has write access to issue trackers? What prevents it from creating, modifying, or closing issues in ways that compromise the governance record?
- What is the process for reviewing and approving the AI tools used within OBST? Who holds approval authority — the team, the division, or the departmental SIRO?
- Has any consideration been given to insider threat — specifically, a team member using OBST to exfiltrate a structured, coherent description of a sensitive government system to an external AI service?
- Is there a defined retention and disposal policy for OBST artefacts consistent with HMG records management requirements (Public Records Act, departmental retention schedules)?

---

### Hamster

#### Strengths

- The product homepage states Hamster does not train models on customer data — a necessary (though not sufficient) condition for government use.
- Claims of third-party penetration testing and auditing, if substantiated by documentation, are positive signals that would merit further investigation rather than immediate rejection.

#### Issues

**Supply Chain & Third-Party Risks**

1. **What** — Hamster is a US-based commercial SaaS. The document does not address data residency, data sovereignty, or whether data processing is subject to US law, including CLOUD Act compelled disclosure. AAAI teams using Hamster would be sending project briefs and plans — potentially describing sensitive government AI systems — to infrastructure under US jurisdiction.
   **Why it matters** — Under the US CLOUD Act, US authorities can compel US-domiciled providers to produce data stored anywhere in the world, without notifying the data subject or the foreign government. This applies even if Hamster operates UK data centres, because the risk attaches to the company's US domicile, not the server location. This is a significant concern for OFFICIAL or OFFICIAL-SENSITIVE government project data.
   **Question / Decision required** — The team must determine:
   - Option A: Hamster is assessed and found unsuitable for government use — do not proceed.
   - Option B: Hamster is used only for OFFICIAL (non-sensitive) project planning with no classified content — requires a documented policy decision and technical controls to enforce the boundary.
   - Option C: Hamster obtains UK Government assurance (NCSC Cloud Security Principles assessment or G-Cloud listing) — this requires Hamster to undertake significant compliance work and is outside AAAI's control.

2. **What** — Hamster integrates with GitHub, Jira, Linear, Notion, Slack, and AI coding agents (the homepage lists Claude Code, Cursor, Gemini, Copilot, Windsurf, Roo Code, and Cline). Each integration is a trust extension: Hamster receives whatever data those systems expose through the granted scopes.
   **Why it matters** — If the government GitHub organisation or Jira instance contains OFFICIAL-SENSITIVE material (code, issues, comments referencing sensitive data sources), Hamster's integration would ingest that material into a US SaaS platform. The blast radius is not limited to the new project being planned — it extends to all existing content in integrated tooling. This is also a UK GDPR Article 28 sub-processor risk if any personal data appears in those systems.
   **Question / Decision required** — If Hamster is evaluated further, the team must document and constrain the OAuth scopes it requires, and assess whether those scopes are compatible with the sensitivity of existing content in integrated tools. Broad read access to a government GitHub organisation by an unassured US SaaS is not acceptable.

3. **What** — Hamster itself uses multiple AI backends (homepage references integration with Anthropic, Google, Microsoft, and others). It is unclear which AI model generates briefs and plans. This creates a second-tier data processing relationship — government project content → Hamster → AI sub-processor — that is not covered by any primary contract.
   **Why it matters** — This is a UK GDPR Article 28 sub-processor risk. The department would need to know which AI providers Hamster uses, confirm they are named sub-processors, and verify they are contractually bound. A US SaaS routing government data through a further unspecified AI backend compounds the data sovereignty concern above.
   **Question / Decision required** — Which AI models does Hamster use as backends? Are those sub-processors named in a data processing agreement the department's DPO would accept?

**Data Classification & Handling Risks**

4. **What** — The Hamster homepage makes security claims ("architected with security at its core," "3rd-party audited and pen tested") without specifying scope, standard, or certifications. No SOC 2 Type II, ISO 27001, or Cyber Essentials Plus is cited.
   **Why it matters** — "Pen tested" without qualification is a marketing claim that does not correspond to the 14-domain assessment required under NCSC Cloud Security Principles. HMG procurement requires evidence-based assurance, not vendor self-attestation. Using Hamster on the basis of homepage claims would not satisfy GovAssure or departmental security approval requirements.
   **Question / Decision required** — Before Hamster can be considered for government use, the team must obtain: (a) Hamster's Data Processing Agreement and sub-processor list; (b) evidence of SOC 2 Type II or ISO 27001 certification; (c) data residency guarantees for UK-based data; (d) a vendor response to the NCSC Cloud Security Principles. None of this was available on the public homepage.

5. **What** — The document does not address what data Hamster retains after a project closes or a subscription ends.
   **Why it matters** — Residual data held by a US SaaS provider after contract termination is a persistent exposure risk. Without a contractual right to deletion with evidence of secure disposal, government project briefs may remain accessible to Hamster staff or subject to US legal process indefinitely.
   **Question / Decision required** — What is Hamster's data retention and deletion policy? Can the department obtain contractual deletion guarantees with evidence of secure disposal on termination?

**Compliance & Regulatory Gaps**

6. **What** — Hamster does not appear on G-Cloud or any known HMG procurement framework. Procuring it would require a new commercial arrangement, likely subject to CDDO spend controls for AI tools.
   **Why it matters** — The document's own context section states "any use of AI or other technology must be security assured and procurable." Hamster satisfies neither criterion as currently documented. Procuring an unassured US SaaS outside spend controls creates both financial governance risk and security risk.
   **Question / Decision required** — Options: (a) confirm Hamster is procurable under an existing framework — if not, the procurement path is likely months of effort with no guarantee of success; (b) descope Hamster and rely on OBST or the prompt library instead.

**Identity/Access Risks**

7. **What** — The document does not address how Hamster accounts would be provisioned, managed, or deprovisioned for Civil Servants. SaaS tools not integrated with the department's identity provider create orphaned accounts and persistent access risk.
   **Why it matters** — Access that persists after a staff member moves role or leaves creates an ongoing exposure. The two-way sync feature (Hamster can write back to issue trackers, not just read) means a compromised or orphaned Hamster account could modify government records.
   **Question / Decision required** — Can Hamster be integrated with the department's SSO/IdP (e.g., Azure AD/Entra ID)? If not, this is a material access management risk that would need to be mitigated by manual deprovisioning processes — which are unreliable.

#### Open Questions

- Has Hamster been used in a UK public sector or regulated government context subject to NCSC guidance? The homepage references enterprise customers (Lenovo, SAP, Salesforce, Dell) but no UK public sector references.
- Does Hamster have a UK entity or UK data residency option? US-only data residency materially increases the international transfer burden under UK GDPR Chapter V.
- The homepage describes "two-way sync" closing tickets automatically as tasks complete. What prevents Hamster from modifying or deleting government records in issue trackers, and is there any audit log of Hamster-initiated changes?
- If any Civil Servant names, roles, or contact details appear in project briefs processed by Hamster, a UK GDPR DPIA is required before use. Has this been considered?

---

### AI Playbook-aligned Prompt and Template Library

#### Strengths

- Reuse of already-procured tools (Microsoft Copilot, Claude for Government) avoids introducing new attack surface, new procurement risk, and new assurance obligations — the strongest security property of this option relative to the others.
- Explicit alignment to UK Government AI Playbook stages 0–5 and required artefacts (business case, AI risk assessment, DPIA, model card, test reports) is well-considered. These are exactly the governance documents AAAI teams are struggling to produce.
- No software development means no software supply chain risk, no dependency management, no vulnerability management, and no patching obligation.

#### Issues

**Data Classification & Handling Risks**

1. **What** — The document asserts that Copilot and Claude for Government are "security-assured" but does not specify the scope of that assurance or the classification ceiling under which each has been approved. "Security-assured" is not binary — a tool may be approved for OFFICIAL but not OFFICIAL-SENSITIVE, or for general use but not for security architecture content.
   **Why it matters** — If teams use these tools to draft AI risk assessments or infrastructure specifications, the prompts will contain content about government systems, data sources, and potentially security controls. If the tool's approval does not cover this content type or classification level, using it this way creates an uncontrolled risk — one that would not be visible until a breach or audit.
   **Question / Decision required** — For each tool in the approved set: (a) what is the approved classification ceiling? (b) is the tool approved for use with personal data (relevant to DPIA drafting)? (c) are there use-case restrictions that exclude any of the prompt library's intended uses? "Already procured" is the starting point, not the conclusion.

2. **What** — Prompts that elicit detailed descriptions of government systems, AI model design, data flows, and security controls are themselves potentially sensitive documents. A prompt that asks "describe your model's training data, including data sources excluded due to sensitivity" could, when answered, reveal which data sources are considered sensitive by the department.
   **Why it matters** — This is a policy leakage risk. Sensitive portions of governance artefacts (threat models, vulnerability assessments, security architecture decisions) should not be drafted using AI tools unless the tool is approved for that content at the appropriate classification level. The prompt itself reveals intent and architecture to the AI provider's logging infrastructure.
   **Question / Decision required** — The prompt library must be reviewed for policy leakage risk before deployment. Prompts should be designed to elicit necessary governance information without requiring users to document sensitive security controls or vulnerabilities in AI-processed inputs. A clear list of content types not suitable for AI-assisted drafting should accompany the library.

**Compliance & Regulatory Gaps**

3. **What** — The document says prompts will cover "security" and "infrastructure." Using AI tools to draft security architecture documents or infrastructure specifications may be outside the approved use case for Copilot or Claude for Government, even if those tools are approved for other government use. Departmental AI acceptable use policies typically restrict or require specific approval for security-sensitive content.
   **Why it matters** — If a team uses the prompt library to generate a security architecture document and that use is outside the tool's approved scope, the department may have a compliance violation it is unaware of. Discovery during a security audit or incident investigation would be reputationally and legally damaging.
   **Question / Decision required** — Review the departmental acceptable use policy for each approved AI tool and confirm that using it to draft security and infrastructure artefacts is permitted. If not, those prompt categories must be excluded or handled through a separately-approved workflow.

4. **What** — The prompt library proposes generating artefacts including "AI risk assessment" and "privacy impact assessment." These are formal documents with defined owners, approval processes, and legal significance under UK GDPR and CDDO guidance. AI-generated drafts are not equivalent to completed assessments.
   **Why it matters** — If teams treat AI-generated drafts as completed assessments without human expert review and formal sign-off, this creates false assurance. A DPIA that was AI-generated without expert review and signed off without scrutiny provides no real GDPR protection and could expose the department to ICO enforcement action. The AI system generating the DPIA for an AI system also raises obvious independence concerns that a regulator would scrutinise.
   **Question / Decision required** — The prompt library must include mandatory human review and formal sign-off requirements for all formal governance artefacts. Who is responsible for defining and enforcing that requirement? This must be stated in the framework documentation, not left to individual teams.

**Threat Model Gaps**

5. **What** — The prompt library has no described version control, access control, or integrity protection. Anyone with access could modify a prompt to remove security guidance, introduce biased outputs, or steer governance artefacts in a particular direction without detection.
   **Why it matters** — If the library becomes the de facto governance standard for AI delivery in AAAI, its integrity is a governance integrity issue. A subtly modified security review prompt could cause multiple teams to omit critical security controls from their artefacts without realising it. This is both an insider threat vector and a supply chain integrity risk.
   **Question / Decision required** — Where will the prompt library be hosted and who has write access? Options: (a) read-only, access-controlled repository with a change management and approval process — treat prompt updates as equivalent to policy document changes; (b) shared document with no controls — not acceptable for a library that governs security artefact generation. The team must commit to option (a) before deployment.

**Supply Chain & Third-Party Risks**

6. **What** — The library depends on two AI tools. The document does not address what happens if either tool changes its model behaviour, data handling policies, or is withdrawn from the government framework.
   **Why it matters** — Prompts are optimised for specific model behaviours. Model updates can change output quality, introduce new risk (less accurate security risk assessments), or produce outputs that are no longer compliant with the Playbook format. Tool withdrawal could leave the framework inoperable at a critical delivery point.
   **Question / Decision required** — Should the library be tool-agnostic with variants per tool? Is there a contingency if one or both tools becomes unavailable or changes significantly?

**Missing Context**

7. **What** — The document does not address how the prompt library will be kept current with evolving UK Government policy (AI Playbook updates, new NCSC guidance, ICO regulatory changes, CDDO assurance requirement changes).
   **Why it matters** — A prompt that correctly reflects policy at launch will drift out of date. Governance artefacts generated from an out-of-date prompt could fail assurance review or, worse, be accepted without scrutiny and used to justify deployment decisions that would not survive external review.
   **Question / Decision required** — Who owns the prompt library and is responsible for keeping it current? What is the review cycle? This is an ongoing operational commitment, not a one-time build — it must be resourced accordingly.

#### Open Questions

- Will the prompt library be accessible to all Civil Servants in AAAI, or restricted by role or project classification? Unrestricted access increases the risk of prompts being used with content above their intended classification ceiling.
- Is there a review process for AI-generated governance artefacts before they are submitted to a departmental AI assurance board or used to justify deployment decisions?
- Has the department's SIRO or Data Protection Officer been consulted on use of AI tools to draft documents that may include personal data (e.g., user research outputs used in Playbook Stage 2 artefacts)?
- "Claude for Government" is cited as already procured. Has the specific Anthropic Claude for Government offering been reviewed against NCSC Cloud Security Principles, and is data residency confirmed as UK or EEA?

---

## Cross-Cutting Issues

**Internal Contradictions**

The document states in the Context section that "any use of AI or other technology must be security assured and procurable." None of the three solutions includes any analysis of how it satisfies this requirement. Hamster is a US-based SaaS with no documented government assurance; OBST's AI toolchain is unspecified; and the prompt library relies on the unverified claim that procurement equates to assurance for all use cases. The document's own governing constraint is not applied to any of its solutions.

**Missing Context Applicable to All Solutions**

The document does not state the classification level of AAAI's work or the systems that OBST would be used to govern. This is the most important missing fact for a security review. If AAAI projects routinely handle OFFICIAL-SENSITIVE data or design systems that process OFFICIAL-SENSITIVE data, the security requirements for all three solutions are materially higher than the document implies.

There is no discussion of personal data flows. AAAI is likely to undertake user research (referenced in the problem statement) and to build systems that process personal data. Artefacts describing these systems will likely contain personal data or references to personal data processing. UK GDPR applies to those artefacts and to any AI tool that processes them. No DPIA consideration appears anywhere in the document.
