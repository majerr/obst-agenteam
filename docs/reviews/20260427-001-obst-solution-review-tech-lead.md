---
agent: Tech Lead
document_reviewed: docs/project-outline.md
date: 2026-04-27
---

### Summary

The OBST project outline proposes three solutions to a documented problem: UK Civil Service data science teams (AAAI division) are losing weeks to non-core concerns — user research, governance, infrastructure, delivery — before they can write code. The three candidate solutions are OBST (a bespoke AI-first delivery framework), Hamster (a commercial SaaS planning tool), and an AI Playbook-aligned prompt and template library. The document is early-stage and intentionally light, but several decisions with significant delivery and compliance consequences are left unaddressed. The review below surfaces the issues that must be resolved before the outline is redrafted as a brief.

---

## Solutions

---

### OBST

#### Strengths

- The stage-gate model (Outlines, Briefs, Specs, Tests) is a credible structural response to the problem. It maps naturally to the audit and governance requirements of UK Gov delivery.
- Framing OBST as AI-agnostic is the correct call for a multi-team, security-constrained environment where approved tooling varies and changes over time.
- The claim that the process "emits its own audit trail" is well-matched to the problem: teams spending weeks on governance could be served by artefacts that are a byproduct of normal workflow rather than a separate overhead.

#### Issues

**Missing Context**

1. **What** — The Aims section says the project will "define and develop" OBST with "definitions, tutorial, examples." The word "develop" is ambiguous: it could mean writing documentation and templates, or it could mean building software tooling (CLI, repo scaffolding, agent integrations).
   **Why it matters** — These are categorically different in scope, team capability required, procurement implications, and delivery timeline. Four materially different interpretations exist: (A) a document standard only — markdown templates in a git repo, deliverable in days; (B) a git-based workflow with branch conventions and a GitHub Actions check, deliverable in a week or two; (C) a CLI tool that scaffolds stages and validates artefacts, requiring ongoing software maintenance; (D) a hosted web application, requiring Azure infrastructure, security assurance, and a six-month-plus programme. Choosing D when A would suffice is an expensive mistake; choosing A when the team expects D produces disappointment and low adoption.
   **Decision required** — The team must agree on the delivery form of OBST before proceeding. The recommendation is to start with A or B, validate adoption, and only consider C if there is demonstrated need. D should not be pursued unless evidence requires it.

2. **What** — The document states OBST should support "multiple AAAI teams" but gives no account of how adoption happens: whether it is mandated, recommended, or opt-in; who owns the framework after initial delivery; or how it evolves.
   **Why it matters** — A framework nobody adopts delivers nothing. Adoption strategy in a Civil Service context typically requires sponsor buy-in and a named owner. Without these, the framework will be used by the team that built it and ignored by others.
   **Question required** — Who is the framework owner post-delivery? Is there a named sponsor with authority to mandate or recommend adoption across AAAI?

**Technical Stack Assumptions**

3. **What** — The document refers to "a repository" as the home for OBST artefacts without specifying git, SharePoint, Confluence, or a document management system. UK Civil Service teams using M365 may have SharePoint as their default document store, not GitHub or Azure DevOps.
   **Why it matters** — If OBST artefacts are markdown files in a git repository, the workflow depends on teams being comfortable using git for non-code documents. That is a different habit from using git for code, and an unevidenced assumption for governance documents such as business cases and privacy impact assessments.
   **Question required** — Is there a git platform (GitHub, Azure DevOps) available to all AAAI teams that is approved for the classification level of these artefacts, or would documents be stored in SharePoint/OneDrive?

4. **What** — The document states OBST is "supported by AI" but names no specific integration points. It is unclear whether AI support means teams manually paste prompts into Claude for Government or Copilot, whether structured prompts are embedded in template files, or whether a CLI or application makes programmatic API calls.
   **Why it matters** — The integration model determines the security and procurement surface. Manual paste-in requires no integration work and no new approvals. Programmatic API calls require API keys, network routing within the government network, and potentially new procurement. It is also unclear whether "Claude for Government" and "Microsoft Copilot" expose accessible APIs (as distinct from chat interfaces) for programmatic use.
   **Decision required** — Is AI integration manual, semi-automated (prompts embedded in template files), or fully automated (programmatic API calls)? Each has a different security assurance burden.

**Irreversible or Expensive Decisions**

5. **What** — The stage definitions and required artefact formats at each stage are the central design decision of OBST. The document names the four stages but provides no detail on what the artefacts look like, how heavyweight they are, or how they map to the UK Gov AI Playbook stages 0–5 referenced in the Prompt Library section.
   **Why it matters** — Getting this wrong and correcting it after teams have started using the framework is expensive: all template materials must be revised, teams must be re-briefed, and artefacts already produced may be invalidated. The team that delivered a working prototype in a fortnight is the obvious candidate for a retrospective test of whether the OBST stages would have produced useful artefacts for that project.
   **Question required** — Have the stage definitions and artefact formats been validated against at least one real AAAI project? If not, is there a plan to do so before the framework is released?

**Team & Capability Gaps**

6. **What** — The document does not identify who is building OBST. The non-data-science concerns in the problem statement (governance, infrastructure, DevOps, security) require input from people with those specialisms to produce credible stage artefacts. Data scientists designing governance templates without domain input from security or delivery specialists risks producing well-structured but substantively weak documents.
   **Why it matters** — If OBST ships with inadequate governance templates, teams will either ignore them or submit poor-quality artefacts for sign-off, creating compliance risk.
   **Question required** — Is there access to security, infrastructure, and delivery expertise to validate the framework's coverage of those domains during authoring?

#### Open Questions

- The document says the output of one stage "licenses the beginning of the next." Who performs the review and grants that licence — a peer, a line manager, or a formal governance gate? The answer has significant implications for how lightweight or heavyweight the framework feels in practice and for whether it reduces or simply formalises the weeks of discussion the problem statement describes.
- OBST and the Prompt Library (Solution 3) are not mutually exclusive. OBST defines the stage-gate process; a prompt library could provide the AI-assisted content generation within each gate. The document treats them as competing alternatives without discussing a combined approach.

---

### Hamster

#### Strengths

- Hamster is the only option that requires zero build effort and could be trialled immediately, assuming it passes security assurance.
- The CLI-to-repository model — syncing the full brief, reasoning, and decisions into the repo so AI coding agents read them directly — is technically well-matched to a team that already uses agents for code generation.
- Bidirectional sync with GitHub Issues, Jira, and Linear means teams do not need to abandon existing issue trackers.

#### Issues

**Integration & Interface Risks**

1. **What** — Hamster is a US-based commercial SaaS. The document's Context section states that "any use of AI or other technology must be security assured and procurable." Hamster's website states data is not used for training and that infrastructure is security-audited, but discloses no data residency, hosting region, or compliance certifications.
   **Why it matters** — UK Civil Service procurement and security assurance typically requires a supplier to confirm UK or EEA data residency and hold relevant certifications (ISO 27001, Cyber Essentials Plus). If Hamster cannot satisfy these requirements it cannot be used regardless of its technical merits. This is a binary blocker that should be confirmed before any further evaluation time is invested.
   **Question required** — Can Hamster provide: (a) confirmation of data residency (UK or EEA), (b) relevant certifications (ISO 27001, Cyber Essentials Plus), and (c) a route to procurement (G-Cloud or equivalent)? If not, Hamster should be descoped from the shortlist before the brief stage.

2. **What** — Hamster's primary value for AI execution depends on integration with coding agents — Claude Code, Cursor, Copilot, Gemini, and others. The AAAI team has "Claude for Government" and "Microsoft Copilot" procured. These are distinct products with different API surfaces and security profiles from Claude Code (an agentic CLI tool) and standard Cursor.
   **Why it matters** — If AAAI teams cannot use Claude Code or Cursor on government-managed devices, the core "brief-to-agent execution" feature of Hamster is unavailable, leaving only brief generation and project tracking — a much weaker value proposition relative to OBST or the Prompt Library.
   **Question required** — Which AI execution tools are approved on AAAI developer machines? Are Claude Code or Cursor available, or is the team limited to Claude for Government (browser/API) and Microsoft Copilot?

3. **What** — The Hamster CLI "syncs briefs, tasks, and plans directly into your repository" but the document does not address whether the CLI requires a live outbound internet connection to Hamster's cloud API for all operations, or whether synced files can be read and edited locally once downloaded.
   **Why it matters** — Government developer machines may not have unrestricted outbound internet access. A CLI that requires a live connection to a US-hosted API to function at all is a network security concern and a single point of failure: if Hamster is unreachable or decommissioned, the workflow stops.
   **Question required** — Does the Hamster CLI function offline once files are synced, or does it require a live connection for all operations? What are the network egress requirements on a UK government-managed device?

**Irreversible or Expensive Decisions**

4. **What** — Adopting Hamster as a division-wide planning standard creates a vendor dependency on a third-party SaaS. If Hamster is discontinued, changes pricing significantly, or is acquired and pivots, teams are left with artefacts in a proprietary format inside an external system.
   **Why it matters** — If AAAI teams embed Hamster into their delivery workflow across 10+ projects over 12 months, migrating off it becomes a significant effort. The OBST framework is explicitly designed to produce a repository-resident audit trail; Hamster's artefacts originate in Hamster and are synced into repos, meaning canonical ownership is ambiguous.
   **Alternatives and consequences:**
   - Accept the dependency and negotiate contractual data export guarantees and a defined migration path before signing any agreement.
   - Adopt Hamster only for a time-boxed experimental cohort, with OBST as the documented successor.
   - Reject Hamster in favour of solutions where all artefacts remain in AAAI-controlled storage from day one.
   **Decision required** — Is AAAI willing to accept a third-party SaaS dependency for a division-wide delivery standard? If yes, what contractual protections are required before procurement?

**Missing Context**

5. **What** — The document does not state whether Hamster has been evaluated by anyone on the team, or whether this is a desk-research inclusion. There is no account of a trial or hands-on assessment.
   **Why it matters** — Including an unevaluated commercial product alongside a bespoke framework and a no-build prompt library creates an uneven comparison. The document cannot recommend between solutions without at least a basic assessment of Hamster against the specific AAAI workflow and governance requirements.
   **Question required** — Has Hamster been trialled against a real AAAI brief? If not, is there a plan to run a time-boxed trial (contingent on initial security checks passing) before the solution decision is made?

#### Open Questions

- Hamster's free tier raises a procurement question: is a free-tier SaaS acceptable under AAAI procurement rules, or does all tooling require a formal procurement route regardless of cost? If Hamster's paid tier is required for team-level use, what is the expected annual cost and is it within direct award thresholds?
- Hamster targets "founder-to-enterprise teams." The artefacts it generates are optimised for that context. Is there evidence that Hamster's brief and plan formats meet the content and structure requirements of UK Gov governance stages (business case, AI risk assessment, privacy impact assessment)?

---

### AI Playbook-aligned Prompt and Template Library

#### Strengths

- This is the lowest-risk option by a significant margin: no procurement, no software development, no new infrastructure, and it uses already security-assured tools (Microsoft Copilot, Claude for Government). The absence of new technical debt is a genuine and undervalued advantage.
- Explicit alignment to the UK Government AI Playbook stages 0–5 and their required artefacts (business case, AI risk assessment, privacy impact assessment, model card, test reports) is precisely targeted at the problem. Teams would not need to know what a privacy impact assessment looks like — the template tells them.
- The scope is well-bounded and deliverable by a small team in a defined timeframe without external dependencies.

#### Issues

**Missing Context**

1. **What** — The document does not specify who authors or quality-assures the prompt and template library. Producing prompts that reliably generate useful, compliant governance artefacts requires substantive domain knowledge across security, DevOps, delivery, and governance — the exact areas identified in the problem statement as outside the team's core skills.
   **Why it matters** — A poorly-written prompt that generates a plausible-looking but substantively inadequate AI risk assessment is potentially worse than no template at all: it creates a false sense of compliance. The quality of this library is entirely dependent on the domain knowledge of its authors.
   **Question required** — Who authors each template type, and what is the review and sign-off process? Is there a route to involve security, legal, or governance SMEs in validating that the prompts produce artefacts of the required standard?

2. **What** — The document does not specify the number of templates, the depth of coverage per artefact type, or how the library is maintained as the UK Gov AI Playbook evolves. The Playbook is a living document; procurement rules and governance requirements change.
   **Why it matters** — Without a maintenance model, the library degrades over time. A library that teams cannot trust is not used; a library that produces outdated governance artefacts creates compliance risk.
   **Question required** — What is the planned scope (number and type of templates)? Who owns maintenance, and what triggers a review of existing templates — for example, a new Playbook version, a model update, or user feedback indicating poor output quality?

**Team & Capability Gaps**

3. **What** — The document describes data scientists as the target users but does not address whether data scientists have sufficient familiarity with governance artefacts to evaluate whether AI-generated output is adequate before submitting it for sign-off.
   **Why it matters** — The value of the library depends on users being able to critically assess and edit the generated output, not just accept it. If teams treat AI-generated governance documents as finished artefacts, poor-quality documents will be submitted for sign-off, creating risk for the broader AAAI programme.
   **Question required** — Is there a plan to pair the template library with guidance on how to evaluate the AI-generated output — i.e., what "good" looks like for each artefact type, and what a reviewer will look for?

**Integration & Interface Risks**

4. **What** — The solution relies on Microsoft Copilot and Claude for Government but does not address the practical usage workflow: how teams access the prompts, how they pass project-specific context into them, and how they store or version the resulting artefacts.
   **Why it matters** — Without a defined workflow, teams will each use the library differently, producing inconsistent artefacts and defeating the standardisation goal. The prompt library is only as useful as the workflow that embeds it in daily practice.
   **Question required** — What is the intended usage workflow? For example: are prompts stored in a shared repo and pasted manually into Copilot or Claude for Government, or is there a more structured access model? How are the resulting documents stored and versioned?

**Internal Contradictions**

5. **What** — The document states the library "requires no additional procurement, no software development, and can be deployed immediately." This framing conflates the absence of software development with low effort overall. The problem statement identifies six non-data-science concern areas and the Playbook has six stages, each with multiple required artefacts. A conservative estimate is 30–60 prompt-template combinations requiring authoring, testing, and iteration — likely two to four weeks of effort for a single author with the necessary cross-domain knowledge.
   **Why it matters** — If the library is under-resourced on the assumption that it is trivial to produce, it will be shipped with untested prompts and abandoned after first use, undermining confidence in the broader OBST initiative.
   **Point** — "No software development" is accurate; "can be deployed immediately" is only true if the prompts already exist. The document should acknowledge the authoring effort required.

#### Open Questions

- The Prompt Library and OBST are not mutually exclusive. OBST defines the stages; the Prompt Library could provide the AI support at each stage. A combined approach — OBST as the process skeleton, the Prompt Library as the content generation layer within each gate — may be the strongest option with no commercial dependency and no new software. This is not discussed in the document.
- "Aligned to the UK Government AI Playbook" requires knowing which version is the reference. Is there a canonical, version-controlled source the library should track, and who monitors it for updates?
