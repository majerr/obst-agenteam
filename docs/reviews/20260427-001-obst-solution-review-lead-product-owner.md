---
agent: Lead Product Owner
document_reviewed: docs/project-outline.md
date: 2026-04-27
---

### Summary

The document describes a problem affecting data science teams in AAAI: capable engineers are losing disproportionate time to non-data-science concerns (user research, governance, DevOps, delivery) rather than building. Three candidate solutions are proposed — OBST, a bespoke AI-first delivery framework; Hamster, a commercial US-based SaaS product planning platform; and a prompt and template library aligned to the UK Government AI Playbook. The stated aim is to "define and develop a framework and supporting materials for use in AAAI." The document is at an early exploratory stage: it articulates the problem clearly but lacks the specificity needed to select a solution, establish success criteria, or assess delivery feasibility. Significant decisions, assumptions, and risks must be resolved before any solution is progressed.

---

## Solutions

---

### OBST

#### Strengths

- Framing OBST as a framework rather than a technology is strategically sound. It avoids premature tooling lock-in, reduces procurement risk, and is consistent with GDS's emphasis on meeting user needs rather than adopting technology for its own sake.
- The stage-gate model (Outlines > Briefs > Specs > Tests, with review and sign-off at each stage) directly addresses the stated problem of teams lacking structure between idea and code.
- The built-in audit trail and structured artefacts (decision records, specs, tests) are coherent with HMG governance obligations rather than an addition to them.
- The principle that OBST "could be followed without AI, given sufficient resource" is pragmatically important in a Civil Service context where tool procurement and security assurance timelines are variable.

#### Issues

##### User Need Assumptions

1. **What** — The problem statement attributes delay to "discussing user value, user research, infrastructure, governance, devops and delivery." OBST is presented as a framework that moves teams faster from idea to implementation readiness, but the document does not establish whether the bottleneck is the absence of a structured process or the absence of subject-matter expertise and resource.

   **Why it matters** — If teams are spending weeks on these concerns because they lack skilled practitioners (no user researcher, no DevOps engineer), a framework will not reduce that time — it will make the gap more structured but no smaller. The solution to a resource gap is different from the solution to a process gap.

   **Question / Decision required** — Has any research (even lightweight: interviews, time-diaries, retrospectives) been conducted with AAAI teams to confirm that a structured framework would reduce time spent on non-data-science concerns, rather than simply making that time more organised? This assumption should be tested before investing in OBST development.

2. **What** — The document does not describe who within AAAI would use OBST, at what career stage, or with what level of delivery experience. Data scientists, delivery managers, and product managers have different mental models of process and different tolerances for documentation overhead.

   **Why it matters** — A framework that feels natural to a delivery manager may feel bureaucratic to a senior data scientist. Adoption depends on the experience being sufficiently low-friction for the actual target users. Uniform user behaviour has not been validated.

   **Question / Decision required** — Who are the primary users of OBST, and have they been involved in shaping the framework stages and artefacts? Without this, the stage model and artefact types are assumptions, not validated requirements.

##### GDS & Standards Alignment

3. **What** — If OBST is made available across AAAI teams — which "for use in AAAI" implies — it is effectively an internal product or service. The document does not propose applying a GDS-aligned Discovery/Alpha/Beta approach to OBST's own development.

   **Why it matters** — Without a GDS-aligned delivery approach for OBST itself, the team risks building something that does not meet the needs of AAAI teams and that cannot be iterated responsibly. There is also a risk of reputational harm if OBST is positioned as a standard without an evidence base behind it.

   **Decision required** — Should OBST itself be developed using a lightweight GDS-aligned Discovery/Alpha approach? Alternatives: (a) yes — conduct a Discovery to validate the problem and proposed solution before committing to development; (b) no — treat OBST as an internal practice standard developed by subject-matter experts and iterated informally. Consequence of (b): adoption and effectiveness will not be systematically measured, and the framework may embed assumptions that prove wrong and are later costly to change.

4. **What** — The UK Government AI Playbook (Feb 2025) defines a staged delivery model (stages 0–5) with required artefacts at each stage. OBST stages (Outline, Brief, Spec, Test) do not map to AI Playbook stages or GDS phases (Discovery, Alpha, Beta, Live). The document does not address this relationship.

   **Why it matters** — If OBST stages do not align with AI Playbook and GDS requirements, teams may produce a complete OBST artefact trail while remaining non-compliant with departmental AI governance policy. This would undermine OBST's stated benefit of facilitating governance.

   **Decision required** — Should OBST stages be explicitly mapped to, or designed to satisfy, both the AI Playbook staged model and GDS service phases? If yes, this mapping must be produced before the framework is finalised. If no, the document must state clearly that OBST is a pre-Discovery tool only, and teams must be told how to fulfil AI Playbook and GDS obligations alongside it.

##### Value & Outcome Risks

5. **What** — No measurable success criteria are defined for OBST. The document lists process benefits (audit trail, narrative structure, accelerated development) but gives no baseline or target metric — for example, time from idea to implementation-readiness, number of governance issues caught at review stages, or team satisfaction scores.

   **Why it matters** — Without measurable outcomes, there is no basis for deciding whether OBST is working, needs iteration, or should be discontinued. In a Civil Service context, this also makes it difficult to justify continued investment.

   **Question / Decision required** — What does success look like for OBST at six months? What would be observed or measured to confirm the framework is delivering the stated benefits? At minimum, a baseline of current time-to-implementation-readiness should be established before OBST is introduced.

##### Scope & Prioritisation Risks

6. **What** — The document defines the aim as producing "a framework and supporting materials (definitions, tutorial, examples)" but does not specify which stage of development OBST is in, what constitutes an MVP, or what the delivery timeline is.

   **Why it matters** — "Framework plus supporting materials" could mean a one-page guide or a comprehensive toolchain with AI agents, templates, and worked examples. Without scope boundaries, the team risks indefinite expansion of the artefact set.

   **Decision required** — What is the minimum viable version of OBST — the smallest set of artefacts and guidance that would allow a single AAAI team to run a complete OBST cycle? Scope should be fixed before development begins, with a formal review point after the first live use.

##### Irreversible or Expensive Decisions

7. **What** — The four OBST stages (Outlines, Briefs, Specs, Tests) are named but not defined anywhere in the document. Inputs, outputs, review criteria, and sign-off processes for each stage are absent.

   **Why it matters** — Once teams begin building artefact trails, review processes, and tooling around OBST, the stage model becomes embedded in practice and costly to change. If the stage definitions turn out to be poorly calibrated, unwinding them after adoption requires significant change management effort.

   **Question / Decision required** — Has the stage model been tested end-to-end with any AAAI team, even informally? A single dry run with a real project would surface calibration issues before the framework is promoted more widely. If not, this should be a prerequisite before wider rollout.

#### Open Questions

- The document notes "one team delivered a prototype multi-layer, working, end-to-end prototype in a fortnight." Was this team using any precursor to OBST, or working without a framework? Understanding whether the fast team succeeded despite the absence of a framework — or because of informal practices that OBST seeks to formalise — would materially affect the design.
- Who owns OBST? The document does not name a product owner, a team responsible for maintenance, or a governance body that would approve changes to the framework. An unowned framework tends to diverge or stagnate once initial enthusiasm passes.
- The document describes AI as "challenging ideas, reviewing suggestions, generating artefacts, managing issues" within OBST. What review and validation process governs AI outputs before they become part of an official record? This is directly relevant to AI Playbook Principle 2 (human oversight) and Principle 7 (transparency about AI use).

---

### Hamster

#### Strengths

- Hamster is a live production service with existing integrations (GitHub, Jira, Linear, Notion, coding agents via CLI). If it were procurable, it would offer a faster route to capability than building OBST from scratch.
- The two-way sync model and CLI-based brief distribution are well-aligned to the kind of AI-assisted development described in the problem statement.
- Including a commercial market comparator is useful for benchmarking what OBST needs to achieve and informing its design.

#### Issues

##### GDS & Standards Alignment

1. **What** — Hamster is a US-based commercial SaaS product. The document's Context section states "any use of AI or other technology must be security assured and procurable." Hamster is not listed as a security-assured or centrally procured tool; it sits entirely outside the set of already-assured tools (Microsoft Copilot, Claude for Government) referenced elsewhere in the document.

   **Why it matters** — UK Civil Service teams cannot use unapproved SaaS tools for work-related data. The procurement and security assurance process for a new SaaS product — involving a DPIA, third-party security review, contract negotiation, and potentially an IT Health Check — can take months and is not guaranteed to succeed. Hamster's marketing claims about data handling do not substitute for a formal departmental security assurance process.

   **Decision required** — Is Hamster a realistic option given current procurement and security assurance constraints? Alternatives: (a) initiate a formal security and procurement assessment — this takes time and resource and may not succeed; (b) treat Hamster as out of scope until assurance is complete and focus evaluation on the two solutions deliverable within existing constraints. Consequence of proceeding without assurance: potential information security incident and a policy violation. This decision needs an answer before Hamster receives further product attention.

2. **What** — Hamster stores team briefs, project context, and task plans. For AAAI, this data may be sensitive or policy-relevant. The document contains no discussion of data handling, information classification, or what categories of data would flow into Hamster.

   **Why it matters** — Sending project context, design decisions, or governance information about government AI products to a US-based SaaS platform may breach information security policy, depending on the classification of the material involved.

   **Question / Decision required** — What is the likely information classification of material AAAI teams would input to Hamster? Has the department's security or data protection officer been consulted? If not, this is a prerequisite before any further evaluation.

##### Decisions Required

3. **What** — The document does not state the basis on which Hamster is included as a candidate solution. It is presented as an "existing service" without evaluation against the problem statement, comparative assessment against OBST, or any cost information.

   **Why it matters** — Without an evaluation framework, Hamster cannot be meaningfully compared with the other solutions. The document risks appearing to endorse it without due diligence.

   **Decision required** — Is Hamster a serious procurement candidate (requiring security and commercial assessment) or is it included as market context to inform OBST's design? These require different next steps and the answer should be made explicit.

##### Value & Outcome Risks

4. **What** — Hamster targets "founders, scale-up teams, and enterprise product organisations" building software products with AI coding agents. The workflow — briefs, acceptance criteria, CLI sync to coding agents — is designed for software product teams, not analytical and AI/ML teams in a regulated government environment.

   **Why it matters** — There is a risk that Hamster solves a related but different problem. The non-data-science concerns cited in the problem statement are predominantly governance and assurance artefacts (AI risk assessment, privacy impact assessment, model card). Hamster does not appear to generate these artefacts, which means teams using it would still need another solution for the governance burden.

   **Question / Decision required** — Can Hamster produce the specific artefact types required in AAAI, or would it need to be supplemented by other tools? If supplementation is required, how does the value proposition compare with the prompt library option, which addresses governance artefacts directly?

##### Irreversible or Expensive Decisions

5. **What** — Adopting Hamster as a platform would create dependency on a third-party commercial SaaS product for core delivery workflow. If Hamster changes pricing, terms, or product direction — or is acquired — the team would need to migrate artefacts, retrain users, and re-establish integrations.

   **Why it matters** — Vendor dependency for a core delivery process is a material risk for a Civil Service team that cannot easily re-procure alternatives at short notice.

   **Decision required** — If Hamster is considered for procurement, is there a defined exit strategy? What data portability does Hamster offer? This must be assessed before any procurement commitment is made.

#### Open Questions

- Has anyone in AAAI piloted Hamster, even informally using the free tier with non-sensitive data? Without hands-on evaluation, the description in the document is essentially a summary of vendor marketing. A time-boxed evaluation spike would provide better evidence before any procurement decision.
- The document does not state what problem Hamster solves that OBST and the prompt library cannot. Without a comparative analysis, it is not clear why Hamster is included as a candidate alongside an internally developed framework that appears to be already in progress.

---

### AI Playbook-aligned Prompt and Template Library

#### Strengths

- This is the only option that demonstrably satisfies the security and procurement constraint stated in the Context section. It uses tools already approved (Microsoft Copilot, Claude for Government), requires no new procurement and no software development, and involves no data transfer to third-party systems. This is a significant practical advantage over both alternatives.
- Explicit alignment to AI Playbook stages 0–5 and the required artefacts at each stage (business case, AI risk assessment, privacy impact assessment, model card, test reports) is the most direct response to the governance component of the stated problem.
- Templates and prompts are low-cost to iterate. If early versions are wrong, they can be updated without architectural consequences. This makes the option well-suited to an incremental, evidence-led approach.
- The option can be deployed without a procurement or development cycle, which is a material benefit in a Civil Service environment where both can be slow.

#### Issues

##### User Need Assumptions

1. **What** — The prompt library assumes the bottleneck is the absence of structured prompts and templates — that teams would produce adequate governance artefacts faster if given the right prompts. The problem statement says teams are "spending weeks discussing" these concerns, not that they are failing to produce documents. Discussion is a human coordination activity; prompts address document generation.

   **Why it matters** — If teams are spending time in discussion because they are genuinely uncertain about requirements, lack expertise, or are navigating organisational ambiguity (unclear sign-off chains, contested scope), pre-written prompts may produce documents faster without resolving the underlying delay. The document will be created but the discussion will continue.

   **Question / Decision required** — Is the delay caused by slow document production, slow discussion, or slow sign-off? The prompt library directly addresses only the first of these. Even lightweight user research (short interviews with two or three data science team leads) would clarify which problem the library is actually solving.

2. **What** — The solution assumes that Microsoft Copilot and Claude for Government are the right tools for generating governance documents. It does not address the quality or reliability of AI-generated governance artefacts, or what validation process ensures outputs meet the required standard before being treated as official records.

   **Why it matters** — AI-generated privacy impact assessments, AI risk assessments, and model cards that are accepted without critical review could create compliance risk. Governance artefacts exist to surface genuine risk; if they become a templated output exercise, the assurance value is undermined. This is a direct concern under AI Playbook Principle 2 (maintain human oversight) and Principle 9 (be transparent about limitations).

   **Decision required** — What is the intended review and quality assurance process for AI-generated governance documents? Who is responsible for ensuring outputs are substantively accurate, not merely formally complete? This must be defined as part of the solution design, not left to individual teams.

##### Scope & Prioritisation Risks

3. **What** — "A curated library of structured prompts and document templates covering user research, security, infrastructure, governance, DevOps, delivery" is a very wide scope. The AI Playbook stages 0–5 and their required artefacts alone could generate a large number of prompts across multiple roles and project types.

   **Why it matters** — A library that is too broad to navigate or too shallow to be trustworthy will not be adopted. Teams will revert to asking colleagues or producing documents from scratch. Curation and maintenance are ongoing costs that are not acknowledged in the document.

   **Decision required** — What is the minimum viable library — the smallest set of prompts and templates covering the highest-frequency, highest-pain artefacts for AAAI teams? The library should be scoped to a defined artefact set before work begins, with a mechanism for adding to it based on evidence of demand. A reasonable starting point would be AI Playbook governance artefacts at stages 0–3 only.

##### Stakeholder & Governance Gaps

4. **What** — The document does not identify who owns, maintains, or quality-assures the library. Prompt libraries decay: AI Playbook updates, changes to departmental policy, new security guidance, and model capability changes will all require the library to be updated.

   **Why it matters** — A prompt library without a named owner and maintenance cadence will become outdated, potentially guiding teams to produce artefacts that no longer meet current standards.

   **Decision required** — Who is the named owner of the prompt library? What is the review and update cadence? Is there a process for teams to flag prompts that produce poor or non-compliant outputs? These should be defined before deployment, not after.

##### GDS & Standards Alignment

5. **What** — The prompt library is described as covering "the AI Playbook's staged delivery model (stages 0–5)" but makes no mention of GDS Service Standard alignment. For AAAI products that are user-facing services, GDS Service Standard compliance is a separate, parallel, and mandatory obligation.

   **Why it matters** — If teams use the prompt library to satisfy AI Playbook obligations but overlook GDS Service Standard requirements, they may produce a set of governance artefacts that is AI Playbook-compliant but not GDS-compliant. These are not the same thing, and failure at a GDS service assessment is a costly and visible outcome.

   **Decision required** — Should the prompt library explicitly cover GDS Service Standard artefacts (user research plans, accessibility statements, service assessment preparation) as well as AI Playbook artefacts? If AAAI teams build services with external users, this is not optional. Scoping this in, however, materially increases the size and complexity of the library.

##### Value & Outcome Risks

6. **What** — The document implies the library "can be deployed immediately." This does not account for the time required to write, test, iterate, and validate prompts for each artefact type to a standard where outputs are reliably usable and acceptable to assurance functions.

   **Why it matters** — If "deployed immediately" is taken to mean days, this will set false expectations. A prompt that reliably generates a usable AI risk assessment requires testing against real projects, iteration based on output quality, and sign-off from the relevant assurance function (e.g., SIRO office, data protection team).

   **Question / Decision required** — What is the realistic timeline for a first usable release, and who has agreed it? Has the relevant assurance team been consulted on what constitutes an acceptable AI-generated artefact?

#### Open Questions

- Is there an existing prompt library anywhere in HMG (CDDO, DSIT, other departments) that could be adopted or adapted rather than built from scratch? Building from existing work would reduce time and improve credibility.
- Are Microsoft Copilot and Claude for Government both actively licensed and accessible to all AAAI teams? If only one tool is universally available, the library should be tool-specific rather than assuming choice.
- How would the library be distributed and discovered? A library stored in a SharePoint folder without navigation or search will not be used. The distribution and access model is part of the minimum viable scope, not a post-launch concern.
- Is there a risk that teams treat AI-generated governance documents as sufficient without engaging the specialist functions (security, data protection, legal) that governance processes are specifically designed to involve? If so, the library should include explicit guidance on when specialist review is mandatory rather than optional.
