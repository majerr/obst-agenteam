---
agent: Synthesis
document_reviewed: docs/project-outline.md
date: 2026-04-27
reviewers:
  - Lead Product Owner
  - AI Engineer
  - Cyber Specialist
  - Tech Lead
---

## Summary

The project outline proposes three solutions to a well-recognised problem: UK Civil Service data science teams in AAAI spending disproportionate time on non-technical concerns (governance, user research, DevOps, delivery) before they can write code. The three candidates are OBST (a custom AI-assisted delivery framework), Hamster (a commercial US-based SaaS), and an AI Playbook-aligned prompt-and-template library.

Four specialist reviewers — Lead Product Owner, AI Engineer, Cyber Specialist, and Tech Lead — independently assessed the document from their respective perspectives. This document synthesises their findings, identifies additional solutions considered, and sets out the questions and edits needed to support a solution decision.

---

## Additional Solutions Considered

The following alternatives were evaluated before finalising the three candidates in the outline. None were added. The reasoning is recorded here.

| Candidate | Reason not added |
|---|---|
| **CRISP-DM / Microsoft TDSP** | Established data-science process methodologies, but primarily address the technical ML lifecycle (data understanding, feature engineering, model training, deployment). They do not address governance, user research, or delivery overhead — the specific problem in the outline. |
| **Shape Up (Basecamp)** | A structured product development cycle with pitch-and-betting model that addresses pre-implementation scoping. Well-suited to commercial product teams; does not carry UK Government governance artefact requirements and would need significant adaptation. No pre-existing assurance. |
| **Design Sprints** | Time-boxed, facilitated problem-definition and prototyping method. Could address the coordination and alignment component of the problem but requires trained facilitators and significant team time. A resourcing intervention, not a framework. |
| **Enterprise MLOps platforms** (Dataiku, Domino Data Lab) | Address the operational ML lifecycle (model governance, deployment, monitoring) not pre-implementation planning. The problem is upstream of where these tools operate. |
| **Dedicated delivery capability / centre of excellence** | Embedding delivery managers, governance specialists, or user researchers in data science teams. Addresses a resource gap rather than a process gap; warranted as a complementary intervention but not a framework alternative. |

**Why the existing three are sufficient:** OBST, Hamster, and the prompt library form a complete spectrum — custom framework (high investment, highly tailored) → commercial off-the-shelf tool (medium investment, less tailored) → lightweight template library (low investment, immediate). No identified alternative sits outside this spectrum without either failing to address the problem or requiring resourcing changes that are out of scope.

**One emergent option identified in review:** The Tech Lead and Lead Product Owner independently noted that **OBST and the prompt library are not mutually exclusive** — OBST provides the stage-gate process skeleton; the prompt library provides AI-assisted artefact generation within each gate. This combined approach was not discussed in the outline. It should be considered before a decision is made; see Recommended Edits below.

---

## Advantages and Disadvantages by Solution

### OBST

**Advantages**
- The stage-gate model (Outline → Brief → Spec → Test) is structurally well-suited to the problem: each stage produces a reviewable artefact, the process emits its own audit trail, and sign-off gates create natural forcing functions for human oversight of AI outputs.
- Technology-agnosticism is the right design for a multi-team government environment with variable tooling and slow procurement cycles. The framework's value does not depend on any specific model or vendor.
- The principle that OBST can be followed without AI (given sufficient resource) is pragmatically important: it avoids hard coupling to tooling that may not be available to all teams.
- The four-stage model maps plausibly to an AI-assisted pipeline: each stage has defined inputs and outputs, which constrains what the model is asked to do and makes output quality easier to evaluate.

**Disadvantages**
- **Ambiguous delivery form.** "Define and develop" spans a spectrum from a one-person-week markdown document standard to a six-month software programme. The gap between these interpretations is large enough to derail the project if unresolved. *(Tech Lead)*
- **Unvalidated root-cause assumption.** The document assumes the bottleneck is a process gap. If teams are spending weeks on governance because they lack specialist resource (user researchers, security architects), OBST will structure the wait, not resolve it. *(Lead Product Owner)*
- **No AI design.** The four named AI capabilities (challenging ideas, reviewing suggestions, generating artefacts, managing issues) span at least three distinct engineering paradigms — prompted chat, RAG, and agentic loops — and are presented as a single undifferentiated "AI-supported" layer. Artefact generation for governance documents specifically requires policy grounding; without retrieval or constraint, outputs will hallucinate policy requirements. *(AI Engineer)*
- **No threat model.** There is no classification ceiling for artefacts, no data handling policy, and no access control model for the repository. Artefacts describing government systems could be OFFICIAL-SENSITIVE; allowing AI tools to process them before classification decisions are made is an irreversible risk. *(Cyber Specialist)*
- **"AI-first" / "AI-agnostic" / "AI-optional" are mutually inconsistent.** This contradiction must be resolved before the framework can be coherently designed or evaluated. The most defensible formulation is "AI-assisted." *(AI Engineer, Cyber Specialist)*
- **No OBST owner.** No named product owner, maintenance responsibility, or governance body is identified in the document. An unowned framework diverges or stagnates after initial enthusiasm. *(Lead Product Owner, Tech Lead)*
- **No success criteria.** No baseline or target metrics are defined. Without these, the division cannot determine whether OBST is working or justify continued investment. *(Lead Product Owner)*
- **Untested stage structure.** The four stages and their artefact types have not been validated against any real AAAI project. This is the most costly assumption to carry into production. *(Lead Product Owner, Tech Lead)*

---

### Hamster

**Advantages**
- If it could be procured, Hamster is the only option requiring zero build effort and could be trialled immediately.
- The CLI-to-repository model (syncing full brief, reasoning, and decisions into the repo so AI coding agents read them directly) is technically well-matched to the team that delivered a working prototype in a fortnight.
- Two-way sync with GitHub, Jira, and Linear means teams would not need to abandon existing issue trackers.
- The product states it does not train on customer data — a necessary (though not sufficient) starting point for a government procurement conversation.

**Disadvantages**
- **Almost certainly cannot be procured without significant effort that may not succeed.** Hamster is US-domiciled SaaS. Under the US CLOUD Act, US authorities can compel disclosure of data regardless of where it is stored. Hamster discloses no data residency, holds no published ISO 27001 or Cyber Essentials Plus certification, and does not appear on G-Cloud or any Crown Commercial Service framework. The document's own constraint ("security assured and procurable") makes this a binary blocker. *(Cyber Specialist, Tech Lead)*
- **Solves a different problem.** Hamster is designed for commercial product teams shipping software iteratively to end users. Its core value — brief-to-agent execution — depends on integration with Claude Code, Cursor, or similar coding agents, which are likely unavailable on government-managed devices. Its planning capability is untested against UK Government governance artefacts. *(Lead Product Owner, AI Engineer, Tech Lead)*
- **Opaque AI pipeline.** Hamster is primarily an AI coordination layer that routes to third-party model APIs (Anthropic commercial, OpenAI, Google). Procuring Hamster does not procure the underlying AI. Each backend model has its own data handling terms, residency constraints, and UK Government accreditation status. *(AI Engineer, Cyber Specialist)*
- **OAuth integration blast radius.** Integration with GitHub, Jira, Linear, and Notion via broad OAuth scopes means Hamster would ingest existing content in those systems, not just new project briefs. If those repositories contain OFFICIAL-SENSITIVE material, this is an immediate data security incident. *(Cyber Specialist)*
- **Vendor lock-in with no exit strategy.** Artefacts originate in Hamster and are synced into repos; canonical ownership is ambiguous. No data portability guarantee or migration path is described. *(Tech Lead)*
- **No hands-on evaluation.** The document does not record whether anyone on the team has used Hamster. The description is essentially vendor marketing. *(Lead Product Owner, Tech Lead)*

**Conclusion from review:** Hamster should not advance to the brief stage as a primary candidate. The procurement and data sovereignty barriers are severe and resolving them is not within AAAI's control. If the team wishes to keep it as a comparator, it should be reframed as a market reference point (what a mature AI planning tool looks like) rather than a candidate solution, and the description in the outline should be updated to reflect the procurement constraints.

---

### AI Playbook-aligned Prompt and Template Library

**Advantages**
- **The only option that demonstrably satisfies the security and procurement constraint** as stated in the outline. Uses tools already approved (Microsoft Copilot, Claude for Government); no new procurement, no software development, no data transfer to unapproved third-party systems. *(Lead Product Owner, Cyber Specialist, Tech Lead)*
- Explicit alignment to AI Playbook stages 0–5 and named artefact types (business case, AI risk assessment, privacy impact assessment, model card, test reports) directly targets the governance component of the problem.
- Prompt and template libraries are version-controllable, auditable, and replaceable. A prompt is readable by a non-engineer, reviewable by a policy expert, and testable with minimal infrastructure. This transparency is an advantage over black-box SaaS pipelines. *(AI Engineer)*
- Can be delivered in a defined scope by a small team without external dependencies.

**Disadvantages**
- **"Deploy immediately" conflates no software with low effort.** A library covering AI Playbook stages 0–5 across the concern areas in the problem statement is realistically 30–60 prompt-template pairs, requiring authors with cross-domain knowledge in security, governance, and delivery — the same domains identified in the problem statement as outside the team's core skills. *(Tech Lead, Lead Product Owner)*
- **May solve the wrong problem.** The problem statement says teams spend weeks *discussing* non-data-science concerns, not that they fail to produce documents. If the delay is caused by organisational ambiguity, unclear sign-off chains, or contested scope, faster document generation will not reduce discussion time. *(Lead Product Owner)*
- **"Already procured" ≠ "approved for all use cases".** Each approved AI tool will have a classification ceiling, personal data processing restrictions, and acceptable use constraints in its contract. Using these tools to draft security architectures, infrastructure specifications, or AI risk assessments may be outside the approved use case. *(Cyber Specialist)*
- **Policy-currency risk.** Prompts cannot prevent models from hallucinating policy references or citing superseded guidance if the model's training data does not reflect the current version of the AI Playbook, GDS Service Standard, or NCSC guidance. Cheap mitigation: include current policy excerpts as context in each prompt (in-context grounding without retrieval infrastructure). *(AI Engineer)*
- **No ownership, maintenance cadence, or evaluation gate proposed.** The library will become stale as policy evolves. Prompts deployed without evaluation against the relevant standard may produce plausible-looking but non-compliant governance documents. *(All four reviewers)*
- **Policy leakage risk.** Prompts that elicit descriptions of government systems, security controls, or data flows are themselves potentially sensitive. The prompt reveals intent and architecture to the AI provider's logging infrastructure. A review process for prompt content is required before deployment. *(Cyber Specialist)*
- **Quality risk.** AI-generated governance documents (PIAs, AI risk assessments) treated as completed artefacts without expert review create false assurance. This must be explicitly addressed in the library documentation. *(Lead Product Owner, AI Engineer, Cyber Specialist)*

---

## Questions Humans Must Answer Before a Decision Is Possible

These questions are sequenced: earlier ones gate later ones. Questions marked **[Blocker]** must be answered before any solution can be selected; others are needed before a brief can be written.

### About the problem

1. **[Blocker] Is the bottleneck a process gap or a resource gap?**
   Teams spending weeks on non-data-science concerns may be waiting for decisions, sign-off, or specialists — not struggling to produce documents. If the bottleneck is resource, no framework resolves it. Even a lightweight user research exercise (interviews with 3–5 team leads across AAAI) would answer this before significant investment is made in any solution. *(Lead Product Owner)*

2. **[Blocker] What is the data classification ceiling of AAAI projects?**
   If any AAAI projects routinely handle OFFICIAL-SENSITIVE data, or design systems that process it, the security requirements for all three solutions are materially higher than the document implies. This is the most important missing fact for any security decision. *(Cyber Specialist)*

### About OBST

3. **What delivery form is intended?**
   (A) A markdown document standard in a git repo — days of effort; (B) a git workflow with branch conventions — a week or two; (C) a CLI tool — months; (D) a hosted web application — six months or more. The document cannot be redrafted as a brief until this is decided. *(Tech Lead)*

4. **Who owns OBST after delivery?** Is there a named product owner with authority to iterate the framework and mandate or recommend adoption across AAAI? *(Lead Product Owner, Tech Lead)*

5. **Do the four OBST stages map to the AI Playbook staged model (stages 0–5)?** If not, teams using OBST may produce a complete OBST artefact trail but remain non-compliant with departmental AI governance policy — which would undermine OBST's primary governance benefit. *(Lead Product Owner)*

6. **Has the stage structure been validated against a real AAAI project?** A single retrospective test — would the OBST stages have produced useful artefacts for the team that "delivered a prototype in a fortnight"? — would surface calibration errors before the framework is released to other teams. *(Lead Product Owner, Tech Lead)*

7. **What is the AI integration model?** Manual (teams paste prompts into approved tools), semi-automated (prompts embedded in template files), or fully automated (programmatic API calls)? Each has different security assurance and procurement implications. *(Tech Lead)*

8. **What is the minimum AI capability required for OBST to be "AI-first"?** For each of the four named capabilities — challenging ideas, reviewing suggestions, generating artefacts, managing issues — which AI architecture is intended? Prompted chat, RAG over a policy corpus, or agentic loop with state? *(AI Engineer)*

### About Hamster

9. **Is Hamster a procurement candidate or a market reference point?** These require different next steps. If candidate: initiate a formal procurement assessment (likely 3+ months, outcome uncertain). If market reference: update the outline to reflect this and remove it from the shortlist. *(Lead Product Owner, Cyber Specialist)*

### About the prompt library

10. **Who authors and quality-assures the prompts?** Does AAAI have (or can it access) subject-matter expertise in security, governance, delivery, and infrastructure to write and validate prompts that produce artefacts of the required standard? The library's quality ceiling is set by this answer. *(Tech Lead, Lead Product Owner)*

11. **What is the approved classification ceiling and permitted use cases for Microsoft Copilot and Claude for Government in this department?** "Already procured" is the starting point, not the conclusion. Using these tools to draft security architectures or AI risk assessments may be outside their approved scope. *(Cyber Specialist)*

12. **Is there an existing prompt library in HMG (CDDO, DSIT, another department) that could be adopted rather than built from scratch?** *(Lead Product Owner)*

### About both OBST and the prompt library

13. **Should OBST and the prompt library be evaluated as a combined option?** OBST provides the stage-gate process skeleton; the prompt library provides AI-assisted content generation within each gate. Two reviewers independently identified this as potentially the strongest option. The document currently presents all three as alternatives and does not consider a combined approach. *(Tech Lead, Lead Product Owner)*

---

## Recommended Edits to the Project Outline

### Edits requiring human input before the document can be redrafted

- **Problem statement:** Add whether the bottleneck is believed to be a process gap, a resource gap, or both, and what evidence supports this. If unknown, note that lightweight user research is needed before solution selection.
- **Hamster:** Either: (a) add a sentence acknowledging the procurement and data sovereignty barriers and positioning Hamster as a market comparator rather than a candidate; or (b) remove Hamster from the candidate list until a formal procurement assessment has been completed. As currently written, Hamster is presented as a parallel candidate to solutions that can be delivered within existing constraints — this creates a false equivalence.
- **OBST — Aims section:** Replace "define and develop" with a specific statement of the intended delivery form (document standard, git workflow, CLI, or web application).
- **OBST — AI design:** Replace "AI-first" with "AI-assisted" (or an equivalent that is consistent with "technology-agnostic" and "could be followed without AI"). Note which AI capabilities are in scope for v1.
- **Mapping to AI Playbook stages:** Add whether the OBST stages are intended to map to AI Playbook stages 0–5. If yes, the mapping must be designed before the framework is released.
- **Context section:** Add the data classification ceiling for AAAI's work and the systems OBST will be used to govern.

### Edits the document authors can make immediately

- **Add success criteria** for each solution: what would be observed or measured at six months to confirm the solution is working? This is needed to compare options and to justify continued investment.
- **Add ownership and maintenance** for each candidate: who owns it, and what is the maintenance cadence?
- **Add a combined option** (OBST + prompt library) as a fourth candidate for evaluation, with a brief characterisation of what it would involve and how it differs from OBST alone.
- **Qualify "deploy immediately"** in the prompt library section: note that 30–60 prompts covering the stated scope require authoring effort by someone with cross-domain knowledge, and that prompts must be evaluated before deployment.
- **Aims section:** Add a constraint that any governance artefacts produced through AI must be reviewed and signed off by a named human before use — this applies to all three solutions and is a non-negotiable AI Playbook obligation.
