---
agent: AI Engineer
document_reviewed: docs/project-outline.md
date: 2026-04-28
---

### Summary

The v2 outline introduces agenteam, a proposed CLI tool acting as a thin wrapper around a library of AI subagents, skills and prompts, using Claude Code as the immediate implementation platform. It also adds a third candidate solution — an AI Playbook-aligned Prompt and Template Library — that was identified as missing in a prior review. This review focuses on new AI engineering issues introduced by agenteam and on the degree to which v1 gaps have been addressed; it does not restate resolved findings. The document remains an outline rather than a design, which is appropriate at this stage, but several AI architecture and risk questions must be resolved before a solution decision can be made.

---

## Solution 1: OBST+agenteam

### Strengths

- The "thin wrapper" framing is appropriate for a one-month, single-engineer effort. Delegating model execution to Claude Code avoids building or operating inference infrastructure, keeping scope realistic.
- Grounding agenteam's scope in OBST process stages is a sound constraint: it makes the agent library finite and the prompt surface area manageable, which directly limits hallucination risk compared to an open-ended assistant.
- Open-sourcing under the developer's ownership while targeting AAAI adoption is a pragmatic ownership model that avoids the procurement risk that disqualifies Hamster.
- Parallel development of OBST and agenteam — each scaffolding the other — is a reasonable bootstrapping strategy that should surface usability gaps early.

### Issues

#### AI Architecture

1. **What** — The document describes agenteam as "a thin wrapper around a library of AI subagents, skills and prompts" but does not specify the orchestration model: whether agents run sequentially, in parallel, or via a supervisor pattern; how inter-agent context is managed; or whether results are synthesised by a further model call.
   **Why it matters** — The orchestration model determines token cost per OBST stage invocation, latency, and the risk of compounding errors across agent hops. A supervisor-synthesis pattern is expensive; pure parallel without synthesis produces unrelated outputs that burden the user; neither is obviously right for a review-style task.
   **Question / Decision required** — Which orchestration pattern will agenteam use for a multi-agent review run? Options: (a) sequential with accumulated context passed between agents (cheap but slow, context window pressure); (b) parallel with a final synthesis call (faster but higher cost, needs a merge prompt); (c) parallel with raw output surfaced to user (cheapest, lowest quality). Decision affects cost modelling and UX design.

2. **What** — Claude Code is stated as the implementation platform, but "Claude Code" is an agentic coding assistant, not a general-purpose multi-agent orchestration runtime. The claim that agenteam can be "powered by any AI service that supports subagents" with Mistral Vibe as an alternative implies an abstraction layer that is not described.
   **Why it matters** — If agenteam is tightly coupled to Claude Code's specific subagent invocation mechanism (e.g. the `@agent` syntax in `.claude/agents/`), porting to another backend is a non-trivial rewrite, not a configuration switch. This is an irreversible early decision if the abstraction is not designed in from the start.
   **Question / Decision required** — Will v1 design an explicit provider abstraction layer, or will Mistral Vibe compatibility be deferred entirely to a later version? If deferred, the document should not imply interoperability as a near-term property.

3. **What** — No retrieval or grounding mechanism is described for the specialist agents (cyber security, infrastructure, product ownership, etc.). The outline implies they will provide "specialist perspective" but does not state whether this is based on baked-in prompt content, RAG over policy documents, or the model's parametric knowledge alone.
   **Why it matters** — UK Government policy and standards (e.g. NCSC guidance, GDS Service Standard, Cabinet Office spend controls) change. Parametric knowledge will be stale within months; prompt-baked summaries require manual maintenance; RAG requires a retrieval pipeline. The choice has significant cost and maintenance implications and directly affects the reliability of governance outputs.
   **Question / Decision required** — What is the knowledge grounding strategy for specialist agents? Options: (a) prompt-embedded static summaries (low cost, maintenance burden, staleness risk); (b) RAG over a curated policy document corpus (higher build cost, ongoing index maintenance); (c) model parametric knowledge with explicit disclaimers (lowest cost, highest hallucination risk for policy-specific claims). A hybrid of (a) and (c) with explicit staleness warnings may be the most tractable for a one-month build.

#### Hallucination Risks

4. **What** — Agents specialising in governance areas (cyber, infrastructure, data ethics) will produce review comments that users may act on as authoritative specialist advice. No mitigation for hallucinated or outdated policy references is described.
   **Why it matters** — A data scientist receiving an AI-generated "cyber review" may present it to stakeholders as substantive assurance. If the review cites outdated or fabricated policy requirements, this could cause harm: projects either advance without genuine security review, or are blocked by phantom requirements.
   **Question / Decision required** — What disclosure mechanism will agenteam surface to users to prevent AI review output being treated as specialist sign-off? Options: (a) mandatory output header on all agent responses stating they are AI-generated drafts requiring human specialist review; (b) structural separation in OBST between "AI pre-review" and "human sign-off" stages, with tooling that cannot mark the latter complete without an explicit human action; (c) no mitigation (not recommended). This must be resolved before the first OBST stage is published.

5. **What** — The v1 gap around artefact generation for governance documents (hallucination without policy grounding) was identified but is not explicitly addressed in v2. The addition of agenteam increases the surface area of this risk: agents will now be generating draft artefacts, not just reviewing them.
   **Why it matters** — If agenteam's "drafting artefacts" capability produces, for example, a model card or AI risk assessment containing plausible but incorrect policy citations, the artefact may pass through review without challenge, particularly in a team without specialist AI governance knowledge (as described in the problem statement).
   **Question / Decision required** — Will artefact-generating prompts include explicit grounding instructions (e.g. "do not cite specific policy documents unless provided in context") and output-level disclaimers? This is a prompt design decision that should be made at the library design stage, not discovered during use.

#### Evaluation

6. **What** — No evaluation framework is described for agenteam's agent library. There is no stated approach to measuring whether agent outputs are accurate, useful, or safe before the tool is used on real AAAI projects.
   **Why it matters** — The v1 review identified absence of an evaluation framework as a key gap. v2 does not address this. A one-month build with no evaluation plan means the first real users are also the first evaluators, on live governance-critical work.
   **Question / Decision required** — What is the minimum viable evaluation approach for the v1 agent library? Options: (a) manual review of outputs against a set of known-good reference documents for each agent type (low tooling cost, high analyst time); (b) LLM-as-judge evaluation on a synthetic test set (automatable, but introduces a second model's biases); (c) no formal evaluation, rely on user feedback post-launch (not recommended for governance-adjacent tooling). A test set of 5–10 representative OBST documents with expected agent output themes should be achievable within the one-month timeline.

#### Cost and Scaling

7. **What** — No token cost estimate is provided for a typical agenteam invocation. The outline envisions "a team of AI agents" reviewing a project outline simultaneously, but the number of agents, average input length, and output verbosity are not bounded.
   **Why it matters** — Claude Code usage for agentic tasks can be expensive at scale. If a six-agent parallel review of a 2,000-word outline costs £2–5 per run, and AAAI teams run this multiple times per project across dozens of projects, costs could reach hundreds of pounds per month before any formal procurement decision is made.
   **Question / Decision required** — What is the per-invocation cost estimate for a representative agenteam run, and is there a budget cap mechanism? This should be estimated before v1 release and communicated to users.

#### Security and Assurance

8. **What** — The document states Claude Code is "assured for UK government use" but does not specify the assurance basis (which framework, at what data classification, under what acceptable use policy). Mistral Vibe is correctly identified as requiring assurance and procurement.
   **Why it matters** — "Assured for UK government use" can mean anything from "on the G-Cloud framework" to "accredited at OFFICIAL-SENSITIVE". The actual classification of AAAI project documents being reviewed by agenteam determines whether Claude Code's current assurance status is sufficient.
   **Question / Decision required** — At what data classification will agenteam inputs be processed? This determines whether Claude Code's existing assurance is adequate or whether additional controls (e.g. data residency, DPA agreements, OFFICIAL-SENSITIVE handling) are required. This is a pre-development decision, not a post-launch one.

### Open Questions

- The document states agenteam would "enable users to add their own agents, skills and prompts." How will user-contributed agents be reviewed for quality and safety before use on governance-critical tasks? A badly written user prompt could produce outputs indistinguishable in format from vetted agent outputs.
- Is there a mechanism for versioning the agent and prompt library independently of the CLI tool? Policy-aligned prompts will need to be updated when policy changes; tight coupling to the CLI release cycle would create maintenance friction.
- What happens when a Claude Code subagent invocation fails mid-run (rate limit, context overflow, model error)? Is there a partial-output handling strategy, or does the whole run fail?

---

## Solution 2: Hamster (tryhamster.com)

### Strengths

- Hamster is a mature, working product with existing integrations (GitHub, Linear, Jira, Notion, coding agent CLI) that would not need to be built. For the problem as stated, it likely covers the brief-and-plan generation use case adequately.
- As a market reference, its existence validates that there is demand for AI-assisted product planning tooling, which supports the case for OBST+agenteam as a UK-government-specific alternative.

### Issues

#### Security and Assurance

1. **What** — The document correctly identifies Hamster as US-domiciled SaaS that is "highly unlikely ever to be assured for UK government use." However, it does not state whether any investigation has been done (e.g. checking G-Cloud, reviewing Hamster's data processing agreements or SOC 2 status).
   **Why it matters** — If Hamster is being included as a genuine option rather than a foil, the basis for disqualification should be documented. If it is only included as a reference point, that should be stated more explicitly to avoid wasting review effort.
   **Question / Decision required** — Is Hamster a real candidate for evaluation, or is it included solely as a market reference? If the latter, the document should remove it from the "Suggested solutions" section to avoid confusion.

2. **What** — No AI architecture analysis of Hamster is provided. It is unknown whether its outputs are grounded, hallucination-mitigated, or auditable in a way compatible with UK Government governance requirements.
   **Why it matters** — Even if Hamster were procurable, its AI quality characteristics are unknown from this document. Evaluating it as a solution requires understanding what it actually produces.
   **Question / Decision required** — If Hamster is to be taken seriously as a candidate, a brief assessment of its AI output quality and auditability is needed.

### Open Questions

- Does Hamster support the specific OBST stage structure (outlines, briefs, specs, tests), or would it require significant workflow customisation that partially recreates the build effort?

---

## Solution 3: AI Playbook-aligned Prompt and Template Library

### Strengths

- This is the only option that requires no new software development, no new procurement, and no new AI assurance decisions. It is immediately deployable using tools already approved for AAAI use.
- Grounding prompts and templates in the UK Government AI Playbook (stages 0–5) and GDS Service Standard directly addresses the v1 gap around governance alignment without requiring a new framework layer.
- Version-controlled and open-sourced, it remains auditable and modifiable by any team member, not only those with CLI or developer skills. This lowers the adoption barrier significantly.
- It directly addresses the core problem (data scientists lacking the skills to produce governance artefacts) with the minimum viable AI intervention: a well-designed prompt does not hallucinate a framework it is given to follow.

### Issues

#### AI Architecture Assumptions

1. **What** — The library is described as designed for use with "AI tools that are already procured and assured" (e.g. Microsoft Copilot, Claude for Government) but no guidance is given on how prompt templates should differ between these tools. Copilot and Claude have different context window sizes, different system prompt capabilities, and different behaviours for structured output.
   **Why it matters** — A prompt template optimised for Claude for Government may produce poor results in Copilot, and vice versa. If the library does not specify target tool(s) or provide tool-specific variants, users will get inconsistent results across the AAAI team.
   **Question / Decision required** — Will the library provide tool-specific prompt variants, or a single tool-agnostic format? If tool-agnostic, what constraints will be applied to ensure portability (e.g. no system-prompt reliance, bounded output length)?

2. **What** — "Governance alignment and maintenance would depend on regular review against current policy" is noted but no governance model for the library itself is described: who owns it, on what cadence it is reviewed, and what happens when a template becomes misaligned with current policy.
   **Why it matters** — A stale template is potentially more dangerous than no template, because it gives users false confidence that their artefact is policy-compliant. The UK AI Playbook and related standards (DSIT guidance, ICO guidance) are actively evolving.
   **Question / Decision required** — What is the minimum governance model for the library? Options: (a) quarterly review by a named owner against published policy updates; (b) community-contributed updates via pull request with an AAAI maintainer; (c) no formal review cycle (not recommended). A named maintainer and a review trigger (e.g. any new DSIT or ICO publication) is the minimum viable model.

#### Hallucination Risks

3. **What** — Templates prompt AI tools to draft governance artefacts. If a user provides minimal context, the AI will fill gaps with plausible-sounding content. The document does not describe how templates will be designed to prevent this.
   **Why it matters** — A model card or privacy impact assessment with AI-generated placeholder content that was not removed before submission creates audit and compliance risk.
   **Question / Decision required** — Will templates include explicit instructions to the AI to flag where it is inferring rather than reporting (e.g. "if you do not have evidence for this field, output [REQUIRES INPUT] rather than a plausible estimate")? This is a prompt design decision that should be standardised across the library.

#### Evaluation

4. **What** — No quality criteria are defined for what a "good" template or prompt in this library looks like, and no process for validating new additions is described.
   **Why it matters** — Without quality criteria, the library will accumulate prompts of varying quality. A prompt that produces a plausible but structurally incomplete model card is worse than no prompt, because it creates the appearance of compliance.
   **Question / Decision required** — What are the acceptance criteria for a template entering the library? A minimum bar might be: (a) output reviewed against the target policy document by a subject-matter expert; (b) tested against at least two different AI tools in the approved set; (c) output reviewed for hallucination risk by a second reviewer.

### Open Questions

- The document does not address how the Prompt and Template Library relates to agenteam. If both are built, the library could become the knowledge base that grounds agenteam's specialist agents, resolving the grounding gap identified under OBST+agenteam. Is this integration intended?
- Who is the intended maintainer of the library — the individual developer, an AAAI team, or the open-source community? This affects whether the governance model described above is realistic.
- Does the library scope include prompts for the "challenging ideas" and "providing specialist perspective" use cases described in the problem statement, or only artefact drafting? The former is harder to template reliably.
