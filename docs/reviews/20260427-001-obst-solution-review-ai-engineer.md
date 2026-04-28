---
agent: AI Engineer
document_reviewed: docs/project-outline.md
date: 2026-04-27
---

### Summary

The document addresses a pre-implementation bottleneck in the AAAI division: data science teams are losing weeks to governance, user research, DevOps, and delivery work that falls outside their core skills. Three candidate solutions are proposed — OBST, a bespoke AI-first delivery framework built around four artefact-stage gates; Hamster, a commercial AI-native product planning SaaS; and an AI Playbook-aligned Prompt and Template Library using already-procured tools. The document is directional rather than evaluative: it introduces options without comparing them, selecting one, or specifying the AI design of any of them. From an AI engineering perspective, each solution carries a materially different risk profile around prompt fragility, hallucination in governance artefacts, vendor opacity, and UK Gov security assurance.

---

## Solutions

---

### OBST

#### Strengths

- The stage-gate structure (outline → brief → spec → test), where each stage's output licenses the next and is punctuated by sign-off, is a sound structural mitigation against AI-generated artefacts being used without human review. It matches how AI-assisted workflows should be governed.
- Technology-agnosticism is the right instinct given UK Government procurement cycles and the pace of model change. It protects the framework from becoming obsolete when models or licences change.
- The audit trail and decision-record outputs directly address UK Gov accountability obligations. Because each stage must produce a sign-off artefact, there is a natural forcing function for human review at each AI-generated output — a critical safeguard for governance-critical documents.

#### Issues

##### AI Approach Assumptions

1. **What** — OBST is described as "AI-first" with four AI capabilities named (challenging ideas, reviewing suggestions, generating artefacts, managing issues), but no AI design is provided for any of them.
   **Why it matters** — These four capabilities have entirely different engineering complexity. "Challenging ideas" is a structured critique task suited to a strong system prompt with no retrieval. "Generating governance artefacts" (AI risk assessments, PIAs) against UK Government standards requires grounding in current policy documents; without retrieval or in-context policy excerpts the model will hallucinate requirements. "Managing issues" implies stateful tracking across sessions, an agentic capability requiring significantly more infrastructure than a prompt. Conflating them as a single "AI-supported" capability conceals the engineering work required.
   **Question / Decision required** — For each of the four named AI capabilities, which architecture is intended? Options: (A) stateless prompted chat — lowest cost, appropriate for idea challenge, insufficient for policy-grounded artefact generation; (B) RAG over a curated policy corpus — substantially reduces hallucination on governance artefacts but requires corpus curation and maintenance; (C) agentic loop with state — highest capability, highest failure-mode surface area, not compatible with the technology-agnostic claim. Not deciding this now means "OBST" will mean different things to different implementers and the framework will produce inconsistent outputs.

2. **What** — The document calls OBST "AI-first" and simultaneously states it is "agnostic about the AI technology used" and "could be followed without AI, given sufficient resource." These three characterisations are mutually inconsistent.
   **Why it matters** — An AI-first design optimises stage inputs and outputs for AI consumption (structured, machine-readable, consistently formatted). An AI-optional design prioritises human readability and manual editability. These produce different artefact schemas, different prompt designs, and different review workloads. Building the framework without resolving this means the design will serve neither goal well.
   **Question / Decision required** — Which characterisation governs design decisions: AI-first (AI drives, humans review), AI-assisted (humans drive, AI supports), or AI-optional (process defined independently, AI plugged in)? The most defensible framing for a governance context is AI-assisted, but this should be an explicit choice. See also: Internal Contradictions below.

##### Hallucination & Trust Risks

3. **What** — Artefact generation is listed as a core AI capability and is the highest hallucination risk. UK Government governance artefacts (AI risk assessments, PIAs, business cases) have specific mandatory content referencing current policy (DSIT AI Playbook, ICO guidance, GDS Service Standard). A model generating these documents without grounding in current versions of those policies will produce plausible-sounding but incorrect or outdated content.
   **Why it matters** — A civil servant who signs off a hallucinated AI risk assessment because it looked authoritative carries real professional and legal risk. The audit trail OBST promises to create would then document a governance failure rather than prevent one. The credibility of the entire framework depends on the accuracy of its generated artefacts.
   **Question / Decision required** — What mechanism prevents AI-generated governance artefacts from fabricating policy references or omitting mandatory content? Options: (A) RAG against a maintained, versioned corpus of UK Government policy documents — most effective, requires ongoing curation; (B) in-context inclusion of current policy excerpts in the prompt — feasible within current context windows, lower infrastructure cost; (C) template-locked output format where AI populates named fields within a fixed structure — reduces hallucination surface but is insufficient alone for complex artefacts. Not deciding this means artefact generation cannot be trusted for production governance use.

4. **What** — "Reviews, redrafts, and signoff" are listed as stage controls but are not designed. The document does not specify what a review entails, who is qualified to conduct it, or what criteria they apply.
   **Why it matters** — The problem statement says teams lack governance, user research, and delivery skills. If reviewers lack the domain expertise to spot hallucinated policy references, review is not an effective safeguard. Rubber-stamping AI output is worse than no review because it creates a signed record of a flawed artefact.
   **Question / Decision required** — What is the minimum qualification required to sign off an AI-generated artefact at each OBST stage? This must be specified in the framework. If the required expertise does not exist in the team, the review step needs a different design (e.g. structured checklist against a canonical source, external SME review).

##### Evaluation & Monitoring Gaps

5. **What** — No evaluation framework is described. The document does not specify how the quality of AI-generated outputs will be measured during framework development or in production.
   **Why it matters** — Without evaluation, there is no basis for trusting AI outputs and no way to detect quality degradation as models are updated by providers. Model updates from Microsoft and Anthropic can silently change output characteristics with no external signal. This is not optional for a system producing governance artefacts in a regulated environment.
   **Question / Decision required** — What evaluation infrastructure will be built before launch? A proportionate minimum: (A) a benchmark set of representative project contexts with known-good artefacts against which generated outputs are scored; (B) factual accuracy checks for policy references in governance documents; (C) a production spot-check cadence to detect drift. Launching without (A) means the framework's AI quality claims are asserted, not evidenced.

##### Cost & Scaling Risks

6. **What** — The cost of running OBST's AI components at division scale is unquantified. If governance artefact generation involves long-context prompts with policy excerpts or retrieval, token costs per project could be material.
   **Why it matters** — UK Government teams operate under budget constraints. If per-project AI costs are significant and unbudgeted, adoption will stall or teams will reduce context to cut costs in ways that degrade output quality without visibility.
   **Question / Decision required** — Has a cost-per-project estimate been produced? This requires knowing the model, the average token budget per stage, and the number of projects per year. Without this, the business case cannot be assessed.

##### Irreversible Decisions

7. **What** — The OBST artefact schemas (what a "brief" contains, what a "spec" contains) will become load-bearing once teams start producing them. If prompts are also tightly coupled to a specific model's behaviour (e.g. Claude's XML conventions, GPT-4o's instruction style), changing either the schema or the model later requires retroactively updating all existing artefacts and re-testing every prompt.
   **Why it matters** — This is low-cost to get right early and expensive to retrofit. UK Government procurement cycles mean available models may change with little notice.
   **Question / Decision required** — Should artefact schemas be versioned from day one (e.g. `brief/v1`, `spec/v1`) and prompts be written against model-neutral output contracts (structured schemas, format specifications) rather than against a specific model's idiomatic behaviour? The alternative is faster initial development with higher retrofit cost if schemas evolve or models change.

#### Open Questions

- The document says OBST "could be followed without AI, given sufficient resource." Is there a plan to build and validate the non-AI version of the framework first, then layer in AI? This would separate framework quality from AI quality and reduce the risk of both being evaluated together.
- OBST is designed to help teams navigate UK Government delivery stages. Has consideration been given to which governance gates OBST itself must pass before it can be officially adopted in AAAI? The framework is an AI-enabled tool that will need an AI risk assessment and potentially a DPIA.
- What happens when AI-generated artefacts from different stages are inconsistent with each other (e.g. a risk assessment generated at brief stage is contradicted by a test plan generated at spec stage)? Is there a mechanism for detecting and resolving cross-artefact inconsistency?

---

### Hamster

#### Strengths

- Hamster's AI philosophy — "you decide what to build; AI handles how it gets built" — is honest about the division of responsibility and does not claim to replace human judgment on strategic or policy questions. This framing is appropriate for governance-critical planning work.
- Two-way sync with GitHub, Linear, Jira, and Notion means planning artefacts are not siloed in a proprietary system, which reduces lock-in risk at the data layer.
- The product is evidently mature and used by substantial enterprise teams (NVIDIA, Google, Vercel, Lenovo, SAP per the website), providing more confidence in its stability than an early-stage startup would.

#### Issues

##### AI Approach Assumptions

1. **What** — The document describes Hamster as generating "structured briefs and task plans collaboratively" but does not assess whether Hamster's outputs align with UK Government artefact requirements. Based on the Hamster website, it targets "founder-to-enterprise" commercial product teams and generates briefs, acceptance criteria, and task breakdowns. It shows no awareness of GDS Service Standard, DSIT AI Playbook stages 0–5, ICO guidance, or HMG spending controls.
   **Why it matters** — The specific artefacts causing friction in AAAI — AI risk assessments, PIAs, business cases, model cards — have mandatory content requirements defined by UK Government policy. A commercial planning tool calibrated for startup and enterprise software teams will not produce these artefacts without substantial customisation, if at all.
   **Question / Decision required** — Can Hamster's templates be configured to produce UK Government-specific governance artefacts, or is it scoped to product delivery planning? If the latter, Hamster solves only a subset of the stated problem and must be supplemented by the Prompt Library option regardless, making it a partial solution at best.

2. **What** — The Hamster website positions its primary value in coordinating AI coding agents (Claude Code, Cursor, Windsurf, Gemini, Copilot) at implementation time. The document describes a pre-implementation planning problem, not a coding-agent coordination problem.
   **Why it matters** — If Hamster's brief-generation capability is a supporting feature rather than its core product, its maturity and development priority for that use case are uncertain. Adopting a tool for a non-primary use case creates risk if the vendor prioritises its core market.
   **Question / Decision required** — Is there evidence that Hamster's planning capability (brief generation, alignment, governance documentation) is mature and actively developed as a standalone function, independent of its coding-agent integration?

##### Hallucination & Trust Risks

3. **What** — Hamster's AI pipeline is opaque. The website does not describe how briefs are generated, what models are used, whether retrieval is involved, or how outputs are constrained. Generated plans and briefs are not grounded in UK Government policy documents.
   **Why it matters** — If a Hamster-generated artefact is challenged during a governance review, the team must explain how it was produced. "Hamster generated it" is not an acceptable answer in a Civil Service accountability context. Additionally, mandatory UK Gov content (DPIA requirements, AI risk register format, spending control thresholds) will not appear in Hamster's outputs unprompted, and a model without current UK Government policy in its training data will likely get these wrong.
   **Question / Decision required** — Before further evaluation, should a structured spike be run: generate five representative AAAI planning artefacts using Hamster, then review each against the relevant GDS/DSIT checklist? This would either establish a baseline confidence or quickly demonstrate the gap, without committing to procurement.

##### Cost & Scaling Risks

4. **What** — Hamster's pricing model is not assessed in the document. The website lists a free tier and enterprise tiers, but no cost analysis is provided.
   **Why it matters** — Government use on a free tier almost certainly violates data handling obligations. Inputs to Hamster will include unpublished project details, policy intent, and potentially OFFICIAL or OFFICIAL-SENSITIVE context. Enterprise tiers require procurement. Neither path is trivial or quick.
   **Question / Decision required** — Can team project inputs be entered into Hamster without violating HMG data classification requirements? This must be assessed against Hamster's data processing terms before any piloting, even exploratory use.

##### Irreversible Decisions

5. **What** — Adopting Hamster as the primary planning platform creates a dependency on a commercial vendor's AI roadmap, pricing continuity, and continued operation. Planning artefacts would live in Hamster's data model.
   **Why it matters** — UK Government projects have long lifespans. AI planning tools in this market segment have a mixed track record of longevity. If Hamster is discontinued, pivots, or changes its pricing, artefact history and workflow continuity are at risk. Unlike a prompt library, Hamster's internal data format and AI pipeline are not portable or auditable.
   **Question / Decision required** — Does Hamster export artefacts in open, portable formats (Markdown, JSON) that are complete and human-readable independent of the platform? This should be confirmed before any adoption decision.

##### Missing Context

6. **What** — Hamster is not security-assured for UK Civil Service use. The document presents it as a candidate solution without acknowledging this gap.
   **Why it matters** — UK Government procurement of a new SaaS AI tool requires a security assessment, a DPIA, and IT procurement approval. Hamster's website references a third-party security audit but does not specify the standard (ISO 27001, SOC 2, Cyber Essentials Plus). UK Government typically requires Cyber Essentials Plus at minimum and often a full technical risk assessment. The Prompt Library option is available immediately; Hamster is not.
   **Question / Decision required** — What is the realistic procurement and assurance timeline for Hamster in the AAAI context? If it exceeds three months, the comparison with the Prompt Library option is not like-for-like and this should be acknowledged explicitly.

#### Open Questions

- The Hamster website claims data is not used to train models and that the product has been third-party security audited. What standard was the audit conducted against, and does it satisfy HMG requirements? Until this is known, the security claim cannot be assessed.
- Hamster's enterprise pricing is not publicly listed. What is the likely per-seat or per-team cost for AAAI use, and is this within the division's budget envelope without a formal procurement exercise?

---

### AI Playbook-aligned Prompt and Template Library

#### Strengths

- This option uses AI within its current reliable capability boundary — structured, long-form text generation with human-defined structural constraints — rather than relying on more complex AI behaviours. This is appropriate for governance-critical outputs where failure modes must be visible and controllable.
- Using already-procured, security-assured tools (Microsoft Copilot, Claude for Government) removes the procurement and assurance barriers that affect both Hamster and a bespoke OBST build. It is the only option deployable immediately under the stated constraints.
- Prompts are readable by a non-engineer, reviewable by a policy expert, version-controllable, and replaceable without touching any infrastructure. This transparency is a structural advantage over black-box SaaS AI pipelines and over prompt sets embedded in a third-party product.
- Explicit scope alignment to UK Government AI Playbook stages 0–5 and named artefact types (business case, AI risk assessment, PIA, model card, test reports) gives the library a clear, evaluable delivery target.

#### Issues

##### AI Approach Assumptions

1. **What** — The document describes "structured prompts" without specifying what structure means in practice. A prompt can range from a two-sentence instruction to a detailed system prompt with role definition, few-shot examples, output schemas, chain-of-thought instructions, and explicit constraints. These produce very different output quality and consistency.
   **Why it matters** — If "structured prompts" means prompts written ad hoc by whoever builds the library, output quality will vary with author, model, and context in ways that are invisible to users. Governance artefacts require consistent, auditable outputs. Inconsistency across the library undermines the core value proposition.
   **Question / Decision required** — What is the specification for a "structured prompt" in this library? Should the library define and enforce a prompt engineering standard (mandatory role, context, output format, constraints, and at least one few-shot example for each artefact type) that all prompts must conform to? Not defining this now means quality will vary with who writes each prompt and there is no basis for comparing or improving prompts over time.

2. **What** — The document does not address how prompts will be maintained as Microsoft Copilot and Claude for Government are updated on vendor timelines outside the team's control.
   **Why it matters** — Model updates can silently change output characteristics — format, verbosity, tendency to hallucinate specific content types — with no external signal. Unlike a codebase, there is no compile step to detect breakage. For governance artefacts, undetected quality degradation is a genuine risk that will surface only when an artefact fails review or audit.
   **Question / Decision required** — What is the maintenance regime? Options: (A) periodic scheduled re-evaluation (e.g. quarterly smoke tests against benchmark outputs from when the prompt was written) — catches gradual drift at low cost; (B) change-triggered re-evaluation when a model provider announces an update — requires monitoring vendor release notes; (C) no maintenance plan, acceptable only if every output is reviewed by a domain expert before use. Option (C) must be a conscious design choice, not an omission.

3. **What** — The document does not address how prompts will handle context-dependent artefacts. A PIA requires project-specific facts: data types, data subjects, retention periods, third-party processors, legal bases. A generic single-shot prompt cannot produce a compliant PIA without this information; it will produce a plausible-looking template with invented specifics.
   **Why it matters** — When an AI model receives incomplete input for a complex artefact, it fills gaps with plausible inventions rather than flagging uncertainty. A PIA generated without the project's actual data processing details will appear complete while containing fabricated content.
   **Question / Decision required** — For context-dependent artefacts (PIA, AI risk assessment, business case), should prompts be structured as guided elicitation sequences — multi-turn prompts that gather project-specific facts before generating the document — rather than single-shot generation prompts? This is a fundamental prompt architecture decision that affects the design of every complex artefact in the library.

##### Hallucination & Trust Risks

4. **What** — Even well-designed prompts cannot prevent a model from generating plausible but outdated policy references. Claude for Government and Microsoft Copilot have training data cutoffs; UK Government policy is updated regularly. A prompt generating a GDS-compliant service assessment will be accurate only to the extent the model's training data reflects current GDS requirements.
   **Why it matters** — Governance artefacts containing superseded policy references or invented policy citations will pass casual review and fail formal audit. The Playbook alignment that is the library's core value proposition depends on the model knowing what the current Playbook says, which cannot be guaranteed.
   **Question / Decision required** — How will prompts handle policy references that may be at or beyond training cutoffs? Options: (A) include current policy excerpts as context directly in the prompt (in-context policy grounding, no retrieval infrastructure required) — most effective, feasible within current context windows; (B) require the user to supply the current policy reference as a named input — shifts verification responsibility to the user; (C) include explicit output disclaimers requiring users to verify all policy references against current sources — lowest cost, weakest mitigation. Option (A) is the most robust and practical for the policy documents in scope; not addressing this at all is the most common failure mode for governance prompt libraries.

5. **What** — The document implies that generating a governance artefact via a prompt produces a document suitable for governance submission. It does not. A model-generated AI risk assessment is a structured draft requiring expert validation, not a signed-off document.
   **Why it matters** — Teams under delivery pressure will treat AI-generated artefacts as complete. If the library ships without explicit guidance that outputs are drafts requiring expert review before sign-off, it will accelerate the production of formally submitted but non-compliant governance documents. This is worse than no library because it creates documented false assurance.
   **Question / Decision required** — Should every prompt in the library produce output that includes a prominent draft disclaimer with a specific checklist of items that must be human-verified before submission? This is a cheap, one-time prompt engineering decision that should be a design requirement from the start, not an optional addition.

##### Evaluation & Monitoring Gaps

6. **What** — No evaluation gate is described before prompts enter the library or before the library is deployed to teams.
   **Why it matters** — Publishing untested prompts for governance artefacts distributes tooling of unknown quality to civil servants. A prompt that produces confident-sounding but incorrect output for a PIA or AI risk assessment is actively harmful. Even a small benchmark — five representative prompts, outputs reviewed by a governance expert against GDS/DSIT checklists — would catch the most common failure modes before they reach production.
   **Question / Decision required** — What is the minimum evaluation gate before a prompt is published? A proportionate minimum: each prompt tested against at least one real project context; output reviewed by a subject-matter expert (not the prompt author) against the relevant government standard; failures recorded and resolved before publication. This requires no tooling beyond a shared document.

7. **What** — There is no described mechanism for collecting quality feedback from teams using the library in production.
   **Why it matters** — Without feedback loops, the library will silently diverge from user needs. Problems will surface as delivery failures or audit findings rather than as library issues. This is a common failure mode for centrally maintained prompt libraries.
   **Question / Decision required** — How will quality feedback be collected? Minimum viable options: (A) a shared channel where teams flag poor outputs; (B) a structured quarterly review of a sample of artefacts produced using the library; (C) a lightweight rating mechanism attached to each prompt. Without one of these, the library has no improvement mechanism.

##### Missing Context

8. **What** — The document does not specify who will own, maintain, and version-control the prompt library, or what the governance model for updating it is.
   **Why it matters** — A prompt library without a named owner becomes stale and contradictory. The UK Government AI Playbook is actively updated; prompts aligned to its artefact requirements will need updating when guidance changes. Without a clear owner and update trigger, the library's alignment to the Playbook will degrade without anyone noticing.
   **Question / Decision required** — Who owns the library? What triggers a prompt update (Playbook revision, model version change, audit finding)? Is this a product with a named owner and a roadmap, or a static artefact? This must be decided before launch.

9. **What** — The document does not define the scope of the library. "Covering the non-data-science concerns identified in the problem statement" is not a specification.
   **Why it matters** — Without a scope definition, the library has no completion criteria. Effort estimates, quality bar, and maintenance burden are all unknown. It could be three prompts or three hundred.
   **Question / Decision required** — Should version 1 of the library be scoped to a specific, bounded set of artefacts — for example, the six artefacts required for a GDS stage 2 assessment — to create a deliverable v1 that can be evaluated before scope is expanded? This would make the project tractable and the evaluation meaningful.

#### Open Questions

- Microsoft Copilot and Claude for Government have different context windows, system prompt capabilities, and instruction-following characteristics. Should the library maintain separate prompt variants per tool, or will prompts be written to be tool-agnostic? Tool-agnostic prompts may produce mediocre results on both; tool-specific variants double the maintenance burden. This is a consequential design choice that should be made explicitly.
- Who in AAAI has the policy domain expertise to write and validate prompts for governance artefacts (AI risk assessments, PIAs, business cases, model cards)? If that expertise does not exist in-house, it is the binding constraint on library quality — not the prompt engineering.
- Is there a mechanism to receive feedback from teams who use the library, so that poor outputs are surfaced and prompts are improved iteratively? Without this, the library will drift out of alignment with real-world needs.

---

## Cross-cutting Issues

### Internal Contradictions

The document describes OBST as "AI-first," "agnostic about the AI technology used," and capable of being "followed without AI, given sufficient resource." These three characterisations cannot simultaneously be true. An AI-first design shapes artefact schemas and review processes around AI interaction patterns. An AI-agnostic design abstracts away from any specific AI capability. An AI-optional design makes AI a performance enhancement rather than a design constraint. These produce different frameworks. The document should resolve which characterisation governs OBST's design before any implementation work begins.

### Solution Relationships

The three solutions are presented as alternatives but are not mutually exclusive. OBST could be the framework, the Prompt Library could provide the AI support layer within it, and Hamster could optionally be evaluated as tooling if it clears procurement. The document should state whether these are alternatives or a potential stack, and if the latter, how they interact. Leaving this unresolved means development work on any one solution may duplicate or contradict another.

### The Binding Constraint

Under the stated context — "any use of AI or other technology must be security assured and procurable" — the Prompt and Template Library is the only solution currently deployable. OBST as designed requires an AI platform to deliver its AI-first capabilities; Hamster requires procurement and security assurance that does not currently exist. The document should acknowledge this constraint when comparing options, as it materially affects the viable sequencing of any implementation. The Prompt Library could be a deployable v1 while OBST's AI design is resolved and Hamster is assessed for procurement.
