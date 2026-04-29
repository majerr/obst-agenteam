---
agent: Synthesis
document_reviewed: docs/project-outline.md
date: 2026-04-28
reviewers:
  - AI Engineer
  - Cyber Specialist
  - DevOps Lead
  - Lead Architect
---

## Summary

Four specialist reviewers — AI Engineer, Cyber Specialist, DevOps Lead, and Lead Architect —
assessed the v2 outline independently. This is a second-round review; the first round (2026-04-27,
ID 001) covered an earlier version of the document. The primary new material in v2 is the
introduction of **agenteam**, a proposed CLI tool wrapping Claude Code subagents, and a
significantly expanded description of the OBST framework. The document was also edited before
this review to add a third candidate solution, the **AI Playbook-aligned Prompt and Template
Library**, which was identified as a missing genuine alternative in the v1 review but not included
in v2 by the author.

The document now has three genuine candidates: OBST+agenteam (bespoke framework + CLI tool),
the Prompt and Template Library (no custom software, uses existing assured tools), and Hamster
(US SaaS, included as a market reference). The v1 decision-blocker questions — data
classification ceiling and bottleneck characterisation — remain unanswered and are still the most
important outstanding facts.

---

## Additional Solutions Considered

The following alternatives were evaluated before confirming the third candidate added to the
outline.

| Candidate | Reason not added |
|---|---|
| **OBST framework only (without agenteam)** | Already implied in the document: the outline states OBST "could be followed without AI, given sufficient resource." Making it an explicit fourth candidate adds no new information; the trade-offs are captured by comparing OBST+agenteam with the Prompt Library (which has lower complexity). |
| **Microsoft Copilot Studio / Power Automate workflow** | Could orchestrate governance artefact generation across already-procured tooling. However, its availability and approval status for AAAI are unknown, and it would require a configuration-heavy build effort comparable in risk to agenteam without agenteam's open-source, version-controlled advantage. Not enough basis to add without knowing AAAI's M365 licensing position. |
| **Self-hosted open-source planning tool** | Raised by the Lead Architect as a potential Hamster analogue that avoids procurement risk. No specific candidate was identified that covers the governance artefact scope of OBST with the UK Government regulatory context. This class of tool is absent from the market at present; worth monitoring but not a current candidate. |
| **Dedicated delivery capability / centre of excellence** | Identified in the v1 review as a complementary intervention, not a framework alternative. Unchanged assessment. |

**Why the Prompt and Template Library was added to the outline:** It is the only candidate that
demonstrably satisfies the security and procurement constraint as stated without requiring new
procurement, development, or AI assurance decisions. Two v1 reviewers independently identified
it as a significant missing option. The v1 synthesis explicitly recommended adding it; v2 did
not. It was added before this review began.

**Why no additional candidate was added to the outline:** The three candidates now span the full
investment/risk spectrum: bespoke build (high investment, highly tailored) → lightweight artefact
library (low investment, immediate) → market reference (not procurable). No identified alternative
sits outside this spectrum while also satisfying the UK Government security constraint.

---

## Advantages and Disadvantages by Solution

### OBST+agenteam

**Advantages**
- The stage-gate model (Outline → Brief → Spec → Test) with defined inputs, outputs, and
  sign-off is structurally sound for AI-assisted governance: it creates natural forcing functions
  for human review of AI outputs and emits its own audit trail. *(AI Engineer)*
- Claude Code's Agent SDK provides all necessary orchestration primitives (subagent spawning,
  tool permission scoping, hooks, session management) for the described use case; the engineering
  work is primarily authoring OBST-specific agents, not building infrastructure. *(Lead Architect)*
- The Agent Skills open standard (agentskills.io), supported by Claude Code, Mistral Vibe,
  Gemini CLI, GitHub Copilot, and ~30 other tools, is a concrete and existing mechanism for
  achieving the stated portability goal for skill definitions — and is not mentioned in the
  document. *(Lead Architect)*
- Technology-agnosticism at the framework level is correct: the governance value survives any
  technology change. *(Lead Architect, AI Engineer)*
- Scoping agenteam to OBST processes limits the attack surface and makes the tool easier to
  threat-model and assure. *(Cyber Specialist)*

**Disadvantages**
- **Unverified assurance claim — potential blocker.** "Claude Code is assured for UK government
  use" has no cited basis in the document. Web research found no publicly verifiable UK
  Government assurance for Claude Code; only US-facing FedRAMP High and IL5 documentation was
  found. If this claim is incorrect, the entire agenteam deployment route is unsupported.
  *(Cyber Specialist, AI Engineer, Lead Architect)*
- **Distribution on managed devices — potential blocker.** Claude Code is distributed via
  `curl | bash`, Homebrew, WinGet, or npm — none of which are standard user-initiated paths on
  NCSC-hardened managed Windows endpoints. No IT approval pathway is described. If civil servants
  cannot install Claude Code, agenteam is unreachable. *(DevOps Lead)*
- **Individual ownership is a supply chain risk.** A tool used by UK Civil Servants for
  governance-critical processes, personally owned by an individual developer, is inconsistent
  with UK Government supply chain security expectations. Risks include credential compromise,
  malicious dependency introduction, and project abandonment with no formal handover. The xz-utils
  backdoor (2024) is the current reference event for this class of risk. *(Cyber Specialist,
  DevOps Lead, Lead Architect)*
- **Architecture undefined.** "Thin wrapper" is not an architectural specification. Three
  meaningfully different implementation options exist (SKILL.md collection, Agent SDK CLI, or
  standalone runtime), each with different dependency surfaces, installation stories, and
  portability implications. The one-month timeline is only plausible for option (b), Agent SDK
  CLI; the portability claim is only achievable with a designed abstraction layer. *(Lead Architect)*
- **No grounding strategy for specialist agents.** Agents providing cyber, infrastructure, or
  product ownership perspectives will produce stale or hallucinated policy references without
  RAG over a policy corpus, embedded policy excerpts, or equivalent. No mechanism is described.
  *(AI Engineer)*
- **Mistral Vibe is a named future platform with no UK assurance.** Naming it creates a de facto
  roadmap that development decisions may accommodate, including for a platform that may never
  achieve assurance. Before treating Mistral Vibe as a portability target, two things must be
  confirmed: its UK assurance status, and whether its subagent orchestration model (parent agent
  spawning and coordinating child agents) is architecturally equivalent to Claude Code's. The
  Agent Skills / SKILL.md standard can make individual *skill definitions* portable across tools,
  but it does not provide subagent *orchestration* — that is a runtime capability. If agenteam's
  core value is orchestrating a team of specialist subagents, the choice of future platform is
  constrained to runtimes that support that pattern, not merely tools that can read SKILL.md
  files. *(Cyber Specialist, Lead Architect)*
- **Artefact data model undefined.** No format, schema, or storage convention is described for
  the outputs at each OBST stage. This is the core data flow of the system; agents cannot be
  authored until it is defined. *(Lead Architect)*
- **Internal contradiction.** The framework is described as "technology-agnostic" while agenteam
  v1 will be authored against Claude Code-specific primitives. Both claims cannot be simultaneously
  true for the reference implementation. *(Lead Architect)*
- **No evaluation framework.** Carried forward from v1. First real users become first evaluators
  of governance-critical tooling. *(AI Engineer)*
- **No cost estimate.** A multi-agent parallel review run on a 2,000-word document could cost
  £2–5 per invocation at scale; no estimate or budget cap mechanism is described. *(AI Engineer)*

---

### Hamster

**Advantages**
- If procurable, it is the only option requiring zero build effort and could be trialled
  immediately; the CLI-to-repository model is technically well-matched to the described team.
- Its existence validates market demand for AI-assisted product planning tooling.
- Provides a concrete benchmark for feature completeness and UX when evaluating OBST+agenteam.

**Disadvantages**
- **Binary procurement blocker.** US-domiciled SaaS under the US CLOUD Act, no data residency
  commitment, no ISO 27001 or Cyber Essentials Plus certification, not on G-Cloud. The document's
  own constraint ("security assured and procurable") disqualifies it. This remains true. *(All
  four reviewers)*
- The level of description in the outline creates false equivalence with candidates that can
  actually be delivered within the stated constraints. *(AI Engineer, Cyber Specialist)*

**Recommendation from review:** Hamster should be explicitly removed from the candidate list.
The document has already partially done this (labelling it as a "market reference point") but
retains it in the "Suggested solutions" section, creating ambiguity. Either remove it from that
section or add a prominent exclusion statement at the top of its entry.

---

### AI Playbook-aligned Prompt and Template Library

**Advantages**
- **Lowest risk on procurement and security.** No custom software, no new procurement, no new
  AI assurance decisions — security baseline is inherited from existing approved tool assessments.
  No software supply chain risk. *(Cyber Specialist, DevOps Lead, Lead Architect, AI Engineer)*
- Immediately deployable; does not depend on IT approval or any new infrastructure.
- Explicit mapping to AI Playbook stages 0–5 and GDS Service Standard directly targets the
  governance artefact gap.
- Version-controlled and open-sourced; auditable and modifiable by any team member.
- **Architecturally complementary to OBST+agenteam, not strictly competing.** Templates define
  what each OBST stage must produce; those definitions are what agenteam's agents would be
  authored against. Building the library first de-risks the agenteam build. *(Lead Architect,
  AI Engineer, DevOps Lead)*

**Disadvantages**
- **Maintenance/staleness — key risk.** The UK AI Playbook, GDS Service Standard, and related
  guidance evolve. No review cadence, named owner, or update trigger is defined. A stale template
  is more dangerous than no template: it produces the appearance of compliance rather than
  compliance. *(All four reviewers)*
- **Prompt portability.** Copilot and Claude for Government have different context window sizes,
  system prompt capabilities, and output behaviours. A prompt optimised for one may produce poor
  results in the other. No guidance on tool-specific variants is provided. *(AI Engineer)*
- **Prompt content as policy leakage.** Open-sourcing governance review prompts exposes the
  division's methodology to adversaries — specifically, which questions are asked (and which are
  not). Applies equally to agenteam's agent definitions. *(Cyber Specialist)*
- **"Already procured" is unverified for AAAI.** The document assumes Microsoft Copilot and
  Claude for Government are available to AAAI users, but does not confirm which tools are actually
  on contract, at what classification level, and for which use cases. *(Cyber Specialist)*
- **Usage is opaque.** The principal success metric is usage, but a library has no mechanism to
  track whether teams use the templates or whether they produce better artefacts. A lightweight
  feedback mechanism is needed. *(DevOps Lead)*
- **"Deploy immediately" is still overstated.** A library covering AI Playbook stages 0–5 across
  the concern areas in the problem statement requires authoring by someone with cross-domain
  knowledge in security, governance, and delivery — the same domains identified as scarce.
  *(v1 finding, unchanged)*

---

## Questions Humans Must Answer Before a Decision Is Possible

Questions marked **[Blocker]** must be answered before any solution can be selected. Others are
required before a brief for the selected solution can be written.

### Carried forward from v1 — still open

1. **[Blocker] Is the bottleneck a process gap or a resource gap?** The v2 problem statement now
   acknowledges both ("This is both a process- and a resource-gap"), but does not describe what
   evidence supports this or what proportion of the delay is attributable to each. If teams are
   primarily waiting for decisions or specialist bandwidth rather than struggling to produce
   documents, no framework resolves this. Even lightweight user research (3–5 interviews) would
   answer this before significant investment is made.

2. **[Blocker] What is the data classification ceiling of AAAI projects?** Still the most
   important missing fact. If any AAAI project artefacts are OFFICIAL-SENSITIVE, the entire data
   path through any AI tool must be assessed at that level — and the Claude Code assurance claim
   (if it exists) must be confirmed to cover that tier.

### New from v2 review

3. **[Blocker] What is the specific assurance basis for Claude Code in UK Government?**
   The claim "Claude Code is assured for UK government use" is unverified. Web research found no
   publicly verifiable UK Government assurance; only US-facing FedRAMP High and IL5 documentation.
   Possible bases: NCSC CPA assessment, G-Cloud listing, Crown Commercial Service framework,
   or departmental security review. This must be confirmed before agenteam proceeds to Brief
   stage. If the assurance is informal or limited in scope, this is a blocker for OBST+agenteam.

4. **[Blocker] Has AAAI IT confirmed that Claude Code can be installed on managed devices?**
   If civil servants are on NCSC-hardened managed Windows devices, no standard Claude Code
   installation path (curl, brew, npm, WinGet) is available to a non-administrator user. This
   is a binary blocker for agenteam regardless of assurance status, and it gates the one-month
   timeline. Options: IT approval for standard path, dedicated developer device exemption, or
   containerised workspace deployment.

5. **What is the intended architectural layer for agenteam?**
   The Lead Architect identified three materially different options: (a) a collection of SKILL.md
   files dropped into a user's Claude Code installation; (b) a Python/TypeScript CLI built on the
   Claude Agent SDK; (c) a standalone orchestration runtime. The one-month estimate and the
   portability claim have different validity depending on the choice. Option (b) is recommended
   and is the only one compatible with both the timeline and a designed extension model.

6. **Should ownership be AAAI/Crown from day one, or individual with a planned handover?**
   Individual ownership of a tool used on government processes is a supply chain risk regardless
   of its current scope. The lowest-risk option at this stage is an AAAI-owned GitHub organisation
   repository with the individual as lead maintainer. Formally personal ownership with an informal
   future handover is inconsistent with NCSC supply chain guidance.

7. **What is the portability target for agenteam, and does Mistral Vibe's subagent model support it?**
   The outline names Mistral Vibe as the only alternative to Claude Code for powering agenteam.
   The key capability is not just reading skill definitions (the Agent Skills / SKILL.md standard
   covers that and is supported widely) but subagent *orchestration* — a parent agent spawning,
   invoking, and collecting results from specialist child agents. Whether Mistral Vibe supports
   this orchestration pattern in a way that is architecturally equivalent to Claude Code's Agent
   tool is not established in the document. Before treating Mistral Vibe as a viable future
   platform, this should be confirmed. If it does support equivalent orchestration, retain it as
   the named alternative (with a caveat that UK assurance is still required). If it does not,
   the portability claim should be reframed: SKILL.md skill *definitions* are portable via the
   Agent Skills standard, but the *orchestration runtime* may be Claude Code-only for the
   foreseeable future, and the outline should say so explicitly.

8. **Is the Prompt and Template Library intended as a standalone solution or as Phase 1 of
   OBST+agenteam?**
   Three of four reviewers independently noted these are architecturally complementary, not
   competing. Templates define what each OBST stage produces; agenteam agents automate
   production of those outputs. Building the library first (low risk, immediate value) could
   de-risk and inform the agenteam build. The document currently frames all three as independent
   alternatives; this framing should be reconsidered.

9. **Are Microsoft Copilot and Claude for Government actually available to AAAI users?**
   The Prompt Library option assumes these tools are procured and available. This should be
   confirmed before the option is recommended, along with the classification level at which each
   is approved.

10. **Is there an existing government prompt/template library (CDDO, GDS, another department)
    that should be adopted or contributed to rather than built from scratch?**
    Researched. No UK Government department, arm's-length body, or public sector organisation
    (CDDO, GDS, DSIT, BEIS, NHS, i.AI, Alan Turing Institute, MoJ) has published a prompt
    library or AI-assisted prompt templates for producing governance artefacts. There is nothing
    to duplicate; the gap is genuine.

    However, three official government-issued form templates exist that cover artefact types the
    library would target:

    | Template | Publisher | Coverage |
    |---|---|---|
    | [Algorithmic Transparency Recording Standard v4.0](https://www.gov.uk/government/publications/algorithmic-transparency-template) | DSIT | Mandatory two-tier disclosure of how and why an algorithmic tool is used |
    | [Ethics, Transparency and Accountability Risk Potential Assessment Form](https://assets.publishing.service.gov.uk/media/60869d85e90e076aaf0a323e/Ethics__Transparency_and_Accountability_Risk_Potential_Assessment_Form_.odt) | Cabinet Office | Risk potential assessment for automated/algorithmic decision-making |
    | [Data and AI Ethics Self-Assessment Tool](https://assets.publishing.service.gov.uk/media/6942d87afdbd8404f9e1f27e/Data_and_AI_Ethics_Self-Assessment_Tool.odt) | DSIT | Ethics posture and challenges across a data/AI project |

    These are static forms (Excel, ODT), not prompts. The prompt library should be designed so
    its outputs are compatible with these official forms — not parallel to them. The ATRS in
    particular is a mandatory reporting requirement for central government use of algorithmic
    tools; a prompt that produces an artefact misaligned with the ATRS structure would create
    extra work, not reduce it.

---

## Recommended Edits to the Project Outline

### Edits requiring human input

- **Hamster:** Add an explicit exclusion statement at the top of the Hamster section, or move it
  to a "Market context" or appendix section outside "Suggested solutions." The current framing
  as a market reference in the body text is insufficient to prevent it consuming review effort
  in future rounds.

- **Claude Code assurance claim:** Replace "Claude Code is assured for UK government use" with
  either the specific assurance basis (framework, classification level, acceptable use scope) or
  a placeholder such as "[assurance basis to be confirmed — see Question 3 above]." The claim as
  written is unverified and potentially false.

- **Mistral Vibe:** Retain the reference but add a qualification. The portability claim rests on
  whether Mistral Vibe supports subagent *orchestration* (parent agent spawning child agents) in
  a way that is architecturally equivalent to Claude Code's Agent tool — not merely whether it
  can read SKILL.md skill definitions (which the Agent Skills standard handles broadly). Confirm
  this before treating Mistral Vibe as a design target, and add a note that UK assurance is
  still required regardless.

- **Data classification ceiling:** Add a statement to the Context section specifying the maximum
  classification of documents that will be processed by any of the proposed tools. If unknown,
  mark as "to be confirmed — required before solution selection."

- **Bottleneck characterisation:** The v2 problem statement now acknowledges both process and
  resource gaps but provides no supporting evidence. Add a brief statement of what evidence
  supports the characterisation, or note that user research is needed before solution selection.

### Edits the author can make immediately

- **Architectural layer decision for agenteam:** Add a short paragraph specifying whether
  agenteam is (a) a SKILL.md library, (b) an Agent SDK CLI, or (c) a standalone runtime. Remove
  the phrase "thin wrapper" or redefine it in terms of one of these options.

- **Portability claim clarification:** Add a sentence distinguishing between three levels:
  (1) framework portability — OBST documents have no technology dependency; (2) skill definition
  portability — SKILL.md files are portable across tools via the Agent Skills standard; (3)
  orchestration portability — the ability to switch the runtime that orchestrates subagents is
  a separate, harder problem that the Agent Skills standard does not solve. State clearly which
  level of portability is in scope for v1 and which is a future goal.

- **Relationship between OBST+agenteam and the Prompt Library:** Add a paragraph clarifying
  whether these are independent alternatives or sequential phases. If phased (Library first,
  agenteam second), state the phase boundary and the handoff point.

- **Artefact data model:** Add a sentence describing the intended format and storage convention
  for OBST stage outputs (e.g. "YAML-frontmatter markdown files committed to the project
  repository"). This is the primary integration point between OBST stages and agenteam agents.

- **Success criteria:** Add measurable criteria for each candidate solution. The current
  statement ("principle success metric would be usage") is necessary but not sufficient. Add at
  minimum: what usage level at what time point would indicate success, and what observable
  quality signal (e.g. reduction in governance-related delay, artefacts accepted first time by
  reviewers) would be tracked alongside it.

- **AI Playbook stage mapping:** Confirm whether OBST stages (Outline, Brief, Spec, Test) map
  to AI Playbook stages 0–5. If yes, the mapping should be included or referenced. A mismatch
  would mean teams using OBST remain non-compliant with departmental AI governance policy.

- **Human sign-off requirement:** Add a constraint in the Aims section that all AI-generated
  governance artefacts must be reviewed and signed off by a named human before use. This is a
  non-negotiable AI Playbook obligation and applies equally to all three candidates.
