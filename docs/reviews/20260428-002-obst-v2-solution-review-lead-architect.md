---
agent: Lead Architect
document_reviewed: docs/project-outline.md
date: 2026-04-28
---

### Summary

The document proposes three candidates to address a process and resource gap in the AAAI division: a bespoke framework-plus-CLI (OBST+agenteam), a US SaaS reference point (Hamster), and a no-code prompt/template library. Version 2's primary addition is agenteam, a CLI tool described as a thin wrapper around Claude Code subagents. This review evaluates all three candidates on architectural soundness, with particular scrutiny of agenteam's design claims, the technology-switching ambition, the extension model, and the framework/tool coupling risk.

---

## Solution 1: OBST + agenteam

### Strengths

- **Separation of framework and tooling is architecturally sound in principle.** Defining OBST as a document-first process framework means the governance value survives any future technology change, and gives a stable interface against which tooling can evolve independently.
- **Claude Code is a well-matched foundation.** The Claude Agent SDK (Python/TypeScript) provides programmatic subagent orchestration, tool permission scoping, hooks for lifecycle events, and session management — all necessary primitives for the described use case. Skills (`.claude/skills/*/SKILL.md`) are the natural unit for packaging per-OBST-stage agent behaviours.
- **The Agent Skills open standard (agentskills.io) is an important asset that the document does not mention.** The SKILL.md format is now supported by Claude Code, Gemini CLI, GitHub Copilot, OpenAI Codex, Mistral Vibe, Cursor, VS Code, and ~30 other tools. Building agenteam's agent library on this standard is a concrete mechanism for achieving the stated platform-switching goal and for community reuse.
- **The one-month single-engineer estimate is plausible for a first release** if agenteam is genuinely a thin orchestration wrapper rather than a novel runtime. The existing SDK handles tool execution, context management, and subagent spawning; the engineering work is primarily authoring the OBST-specific agents and skills.

---

### Issues

#### Architectural Decisions Required

1. **What** — The document names "thin wrapper" as the architectural pattern but does not specify what layer agenteam actually occupies. There are three meaningfully different options: (a) a collection of SKILL.md files and agent definitions dropped into a user's Claude Code installation; (b) a Python/TypeScript CLI built on the Claude Agent SDK that programmatically composes and invokes agents; (c) a standalone orchestration runtime that calls AI provider APIs directly.

   **Why it matters** — Each option has a different dependency surface, installation story, extensibility model, and upgrade coupling. Option (a) requires no custom code but cannot enforce OBST workflow sequencing or emit a structured audit trail. Option (b) is the most tractable and aligns with the SDK's intended use, but the tool is then a Claude Code dependency, not just "powered by Claude Code." Option (c) is the only option that makes platform-switching truly independent, but it is not a thin wrapper and the one-month estimate would be unrealistic.

   **Decision required** — Choose and document the architectural layer. Recommend option (b) with the Agent SDK. The "thin wrapper" framing should be retired or redefined; agenteam is an opinionated orchestration CLI, not a pass-through wrapper.

2. **What** — The document states agenteam "could be powered by any AI service that supports subagents" and names Claude Code and Mistral Vibe as the current options.

   **Why it matters** — Claude Code and Mistral Vibe are architecturally dissimilar. Claude Code is a full agentic runtime with its own tool execution loop, SKILL.md loading, and multi-agent orchestration. Mistral Vibe is a terminal coding assistant; its "subagent" support is a handoff mechanism between agents via the Mistral API, without the tool execution primitives that Claude Code provides. Abstracting across them is non-trivial and not a "thin wrapper" concern. Mistral Vibe does support the Agent Skills (SKILL.md) format via agentskills.io, which gives partial portability for skill definitions, but not for the orchestration layer.

   **Decision required** — Define what "platform-switchable" means in practice: is it (a) skill definitions that are portable via the Agent Skills standard, (b) the full agent loop swappable, or (c) only the underlying model? Each has a different abstraction layer requirement. Clarify this before architecture is designed. If (b) or (c), an adapter/backend interface must be designed and the one-month estimate revisited.

#### Assumptions

3. **What** — The document assumes that Claude Code is "assured for UK Government use." This is stated as a current fact, not a conditional.

   **Why it matters** — If this assurance is limited in scope (e.g. covers specific data classifications, specific deployment contexts, or specific model versions), agenteam's viability for AAAI teams may be narrower than assumed. The assurance status of the Claude Agent SDK (as distinct from the Claude Code CLI) is not addressed.

   **Decision required** — Confirm the scope of the existing assurance: does it cover programmatic API usage via the Agent SDK? If not, what is the assurance pathway and timeline?

4. **What** — The document assumes a single AI engineer can deliver both OBST (a document framework) and agenteam (a CLI tool) in one month.

   **Why it matters** — The estimate is only plausible if the architecture is option (b) above (Agent SDK wrapper) and if the OBST framework stages are already sufficiently defined to author the agents against. If OBST stages are being discovered in parallel with agenteam development, the coupling between them becomes a sequencing risk, not a productivity gain. Parallel co-development only works if there is a shared interface contract (e.g. "what inputs and outputs does each stage produce?") that is stable enough to code against.

   **Decision required** — Define the OBST stage interface contract as a prerequisite to agenteam development. This contract is also what enables future technology substitution.

#### Integration and Dependencies

5. **What** — The extension model is described only as enabling "users to add their own agents, skills and prompts as needed," with no further specification.

   **Why it matters** — Without a defined extension contract, there is no way to evaluate whether user-authored agents will be compatible with the OBST workflow, whether they will be isolated from core agents, or how conflicts and versioning will be managed. The Agent Skills standard provides a partial answer for skill portability, but not for workflow-level integration.

   **Decision required** — Specify the extension model: at minimum, define the directory convention, the metadata schema (frontmatter fields), the tool permissions available to user-authored agents, and whether user agents can override or extend built-in agents. Adopting the Agent Skills standard as the extension format is recommended.

6. **What** — The document does not address how agenteam handles the structured artefact outputs (outlines, briefs, specs, tests) required at each OBST stage: where they are stored, what their format is, and how they flow between stages.

   **Why it matters** — This is the core data flow of the system. If artefacts are unstructured markdown files in an arbitrary directory, the "structured record keeping" and "audit trail" claims are aspirational rather than designed. If artefacts are structured (e.g. YAML frontmatter + markdown, or a defined schema), this must be designed before agents can be authored.

   **Decision required** — Define the artefact data model and storage convention before agent authoring begins. This is also the primary integration point for any downstream tooling (e.g. governance review pipelines, CI checks).

#### Irreversible Decisions

7. **What** — Building agenteam as a named open-source project owned by an individual developer, with AAAI potentially forming a steering group if successful, creates an ambiguous governance structure from the start.

   **Why it matters** — If AAAI teams adopt agenteam as a production dependency and the individual owner's priorities change, there is no governance mechanism to ensure continuity. Conversely, if AAAI asserts formal ownership too early, the open-source community benefit is undermined. The ownership and maintenance model should be agreed before the first public release.

   **Decision required** — Define the governance model before public release: minimum viable options are (a) AAAI-owned repository with the individual as primary maintainer, (b) individual-owned with a documented AAAI fork policy, or (c) foundation-hosted from the outset.

#### Internal Contradictions

8. **What** — The document claims the OBST framework is "agnostic about the AI technology used" while simultaneously stating that agenteam is designed specifically to support OBST and is "powered by" Claude Code as the near-term implementation.

   **Why it matters** — If agenteam's agents are authored against Claude Code-specific primitives (SKILL.md frontmatter fields like `context: fork`, `allowed-tools`, the Claude Agent SDK's `AgentDefinition` API), they are not technology-agnostic. The framework may be agnostic, but the reference implementation is not. This is acceptable if stated explicitly, but the current framing implies a portability that does not exist without an abstraction layer.

   **Decision required** — Distinguish clearly between the portability claim of the OBST framework (documents, stage definitions) and the portability claim of agenteam (the CLI and agents). State explicitly that the initial release is Claude Code-only and that portability is a future goal conditional on the abstraction layer decision in issue 2.

---

### Open Questions

- The document says OBST would "emit its own audit trail." What is the intended format and storage location of this trail? Is it a git-committed artefact, a log file, or a structured record queryable by governance tooling?
- The document references the UK Government AI Playbook but does not map OBST stages to Playbook stages. Is there a one-to-one mapping, or is OBST a subset/superset?
- How does agenteam handle the human review and sign-off steps between OBST stages? Is this modelled as a CLI prompt, a file-based gate, or left to the user?
- What is the expected skill library at first release? A working v1 needs at minimum one agent per specialist role (cyber, infrastructure, product ownership, etc.). Who authors these agents and on what domain knowledge are they based?

---

## Solution 2: Hamster

### Strengths

- **Provides a concrete product reference.** Including Hamster gives the outline a benchmark for the kinds of structured brief and plan generation, tool integrations (GitHub, Jira, Linear), and human-AI collaboration patterns that a mature product in this space requires.
- **Demonstrates demand.** The existence of a funded, productised offering in this space validates that the problem is real and that teams will use tooling for it.

---

### Issues

#### Architectural Decisions Required

1. **What** — The document presents Hamster as a "market reference point" but does not use it to test or bound the design of OBST+agenteam.

   **Why it matters** — If Hamster's feature set and integration model are well understood, they can directly inform what agenteam must do to be competitive or complementary, and what it can defer.

   **Decision required** — Before dismissing Hamster entirely, document which of its capabilities OBST+agenteam must replicate at v1, which can be deferred, and which are out of scope by design. This shapes the v1 scope decision.

#### Integration and Dependencies

2. **What** — The sole stated objection to Hamster is procurement difficulty (US-domiciled SaaS, months of assurance work). The document does not address whether a self-hosted or on-premises deployment of a comparable open-source product exists.

   **Why it matters** — If the procurement blocker is data residency and assurance, an open-source product with equivalent functionality deployed on UK government infrastructure might satisfy both the functional and compliance requirements. This option is not considered.

   **Decision required** — Confirm whether there are open-source alternatives (e.g. Plane, Linear self-hosted, or purpose-built planning tools) that could be deployed within existing infrastructure and would close the gap. If none exist, document this as confirmed.

---

### Open Questions

- Is Hamster included solely for comparison, or is there a scenario in which a procurement pathway could open (e.g. a UK-domiciled subsidiary, a G-Cloud listing)?

---

## Solution 3: AI Playbook-aligned Prompt and Template Library

### Strengths

- **No procurement risk.** The only candidate that demonstrably satisfies the security and procurement constraint on day one, using tools already in the approved estate (Microsoft Copilot, Claude for Government).
- **Directly addresses the governance artefact gap.** The explicit mapping to GDS Service Standard artefacts (business case, AI risk assessment, privacy impact assessment, model card) directly targets the stated problem: teams not knowing what artefacts to produce or how to produce them.
- **Lowest delivery risk.** No software to build, no dependencies to manage, no infrastructure to provision. A single content editor can deliver a first version.
- **Open-sourced and version-controlled.** This means it can evolve with policy changes and can attract community contributions, matching the stated aim.
- **Complementary rather than competing.** A well-designed template library is a prerequisite for OBST: the templates define what a "brief" or "spec" should contain, and those definitions are what agenteam's agents would be authored against. Building Option 3 first de-risks Option 1.

---

### Issues

#### Architectural Decisions Required

1. **What** — The document does not specify whether the template library and OBST are designed to be compatible with each other, or whether they are genuinely independent alternatives.

   **Why it matters** — If a template library is built first (low risk, immediate value), it should be designed with the OBST stage model in mind so that it can serve as the content layer for agenteam agents later. Treating them as fully independent alternatives wastes the sequencing opportunity.

   **Decision required** — Decide whether Option 3 is a standalone solution or a Phase 1 of Option 1. If the latter, define the compatibility requirements now.

#### Assumptions

2. **What** — The document states "governance alignment and maintenance would depend on regular review against current policy" without specifying a review cadence, ownership, or trigger mechanism.

   **Why it matters** — UK Government AI Playbook and data ethics guidance evolves. A library that is not actively maintained becomes a compliance risk rather than a compliance aid. Without a named maintainer and scheduled review, the library will drift.

   **Decision required** — Define a maintenance commitment: who reviews the library, how often, and what triggers an unscheduled review (e.g. a new version of the AI Playbook)?

#### Extensibility

3. **What** — The document does not address how teams customise templates for their specific project type, data classification, or departmental requirements.

   **Why it matters** — A single set of templates will require adaptation. Without a documented customisation convention (e.g. a base template plus project-type overlays), teams will fork the library and diverge, eliminating the standardisation benefit.

   **Decision required** — Define a layering model: base templates plus project-type or classification overlays. This also enables the library to grow without becoming unmanageable.

---

### Open Questions

- Is there an existing government-maintained prompt/template library (e.g. from CDDO, GDS, or another department) that this would duplicate? If so, contributing to an existing effort may be preferable to creating a new one.
- What is the adoption mechanism? How do teams discover and use the library? (A published URL is necessary but not sufficient; integration with existing tooling, onboarding, and training are needed.)
- Would the library include example completed artefacts as well as blank templates? Worked examples are cited as a planned output of OBST; the same logic applies here.
