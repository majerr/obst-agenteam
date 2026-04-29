---
agent: DevOps Lead
document_reviewed: docs/project-outline.md
date: 2026-04-28
---

### Summary

The outline proposes three candidate solutions for improving delivery governance and AI adoption in the AAAI division of UK Civil Service. Two are buildable (OBST+agenteam and a Prompt/Template Library) and one (Hamster) is included as a market reference the document itself concedes is not procurable. This review covers the infrastructure, distribution, operational, and security risks of each buildable solution, with particular focus on the constraints imposed by UK government managed devices, assured procurement requirements, and the single-developer ownership model.

---

## Solutions

---

## Solution 1: OBST + agenteam

### Strengths

- Building on Claude Code, which already has UK government assurance, eliminates the longest lead-time risk: procurement. The underlying runtime is available now.
- The CLI distribution model is appropriate for an AAAI team of technical users (data scientists, data engineers) who already operate in terminal environments.
- Claude Code supports managed settings deployable via MDM/OS-level policy, which provides a path to centralised permission control if the organisation needs it.
- The stage-gate framework (OBST) is a pure document artefact — no infrastructure concerns — and can be iterated independently of the tooling.
- Open-sourcing under individual ownership with a potential AAAI steering group is a lightweight governance model that avoids NHS/Cabinet Office procurement entanglement during early development.
- Claude Code publishes signed apt, dnf, and apk repositories and GPG-signed release manifests, which means installations can be verified without relying on an untrusted public package registry.

### Issues

#### Distribution and Installation

1. **What** — Claude Code is installed via a `curl | bash` script, Homebrew, WinGet, npm, or Linux package managers. None of these are available as a standard user action on UK government managed Windows devices, where AppLocker or equivalent policies typically restrict execution to an administrator-approved allow list, and IT departments deploy software via MDM or a private software catalogue.

   **Why it matters** — If AAAI civil servants cannot install Claude Code on their work laptops, agenteam is unreachable regardless of how well it is built. This is not a theoretical risk; it is the default posture on managed Windows endpoints under NCSC device security guidance.

   **Question / Decision required** — Has AAAI IT confirmed that Claude Code can be, or already is, approved for installation on managed devices? If not, what is the approval pathway: (a) raise a software request with IT to add it to the approved catalogue, (b) request a dedicated developer device exemption, or (c) build agenteam to run inside a pre-approved environment such as a containerised workspace? The answer determines whether the one-month timeline is realistic and what the actual first-user experience is.

2. **What** — agenteam is described as a CLI tool that wraps agenteam/skills/prompts. The outline does not specify how agenteam itself is distributed: PyPI package, npm package, GitHub release binary, or internal artefact repository.

   **Why it matters** — PyPI (`pip install`) and npm (`npm install -g`) are public package registries. Publishing a government-adjacent tool to a public registry with no supply-chain controls creates a dependency on external infrastructure, exposes version metadata publicly, and may not be permissible depending on the sensitivity classification of the prompt and agent content bundled within the package.

   **Question / Decision required** — What is the distribution channel for agenteam? Options: (a) public PyPI/npm — fast and familiar, but supply-chain exposure and possible policy conflict; (b) GitHub Releases binary — usable without a registry, verifiable via checksums, but requires per-device download; (c) internal government artefact repository (e.g. JFrog Artifactory, Azure Artifacts) — most control, but requires infrastructure AAAI may not have; (d) Claude Code plugin/extension — distributable through Claude Code's own plugin mechanism, which is already on the approved device. Option (d) warrants investigation given that Claude Code already supports plugins and managed settings.

#### Versioning and Updates

3. **What** — The outline does not describe a versioning or update model for agenteam. Claude Code's native installer auto-updates in the background. A tool that wraps Claude Code should define whether its own prompt and agent definitions are pinned, auto-updated, or manually versioned.

   **Why it matters** — In a governance context, prompt definitions directly affect the quality and consistency of artefacts (risk assessments, business cases). If prompts change silently between runs, teams cannot reproduce earlier outputs and the audit trail the framework is designed to support is undermined.

   **Question / Decision required** — Will agenteam prompt and agent content be versioned separately from the CLI binary? Should OBST stage outputs record the agenteam version used so reviews are reproducible? Consider semantic versioning with a locked version file per project (analogous to `package-lock.json`).

#### Open-Source and Data Handling

4. **What** — The tool will be open-sourced and owned by an individual (Matthew Roberts). Civil servants will use it to process project artefacts — problem statements, proposed solutions, architecture documents — which may be OFFICIAL or above.

   **Why it matters** — Open-sourcing the prompt and agent definitions means adversaries can study exactly what questions the tool asks and how it reasons, which could assist in crafting artefacts that pass AI review without genuine merit. More critically, if any project artefact is inadvertently included in a commit or logged by the tool, it becomes public. Individual ownership means there is no formal AAAI data handling agreement in place.

   **Question / Decision required** — Before open-sourcing: (a) define what data the CLI tool logs and where logs are stored; (b) confirm the tool never persists project artefact content outside the local filesystem; (c) decide whether the repository should carry a licence that restricts use to OFFICIAL-classified contexts or remain unrestricted. A data handling impact assessment (DPIA) may be required under UK GDPR if artefacts contain personal data.

#### Operational Readiness

5. **What** — The outline identifies no monitoring, error reporting, or support model. agenteam will be used by non-specialist civil servants who will encounter failures when the Claude API is unavailable, rate-limited, or when the tool itself has a bug.

   **Why it matters** — A single developer owner with no on-call or support SLA cannot provide the response times expected of a tool embedded in a governance-critical delivery process.

   **Question / Decision required** — Define a minimum support model before wider AAAI rollout: (a) a GitHub Issues triage SLA; (b) a Slack/Teams channel for first-line support; (c) a stated dependency on Anthropic's Claude API uptime (currently no SLA is publicly offered for Claude for Government). If the tool becomes critical path for project approvals, unavailability blocks governance sign-off.

#### Timeline and Ownership Risk

6. **What** — A single AI engineer is estimated to deliver both OBST (a governance framework) and agenteam (a CLI tool with subagents, skills, and prompts) in one month. The tool is initially owned by an individual, not AAAI.

   **Why it matters** — Individual ownership is a single point of failure. If Matthew Roberts leaves, changes role, or becomes unavailable, there is no handover path, no access to repository secrets, and no transfer of domain expertise. One month for both deliverables assumes no managed device approval delays, no security review cycles, and no significant rework.

   **Question / Decision required** — (a) Should the repository be transferred to an AAAI organisation on GitHub from day one, with the individual as lead maintainer, rather than personal ownership with a later handover? (b) Is the one-month estimate gated on managed device access being pre-approved? If device access is not confirmed before work starts, what is the contingency?

#### Irreversible Decisions

7. **What** — Choosing Claude Code as the only supported runtime is a dependency on Anthropic's continued UK government assurance status, pricing, and API compatibility.

   **Why it matters** — Claude Code is assured now, but assurance is not permanent. If Anthropic changes its data residency terms, pricing, or API surface, agenteam's architecture would need significant rework. The outline acknowledges this risk in passing ("could enable a user to switch between services") but does not commit to an abstraction layer.

   **Question / Decision required** — Should the initial design include a thin abstraction over the AI runtime so that switching is possible without a full rewrite? The cost is modest at build time; the cost of retrofitting later is high.

---

### Open Questions

- What data classification applies to the artefacts OBST produces? OFFICIAL? OFFICIAL-SENSITIVE? This determines whether Claude for Government (which has data residency commitments) is required, or whether the standard Claude API is acceptable.
- Does AAAI have a GitHub organisation account, or will the repository sit in a personal account? The security implications differ significantly.
- Will agenteam require authentication beyond Claude's own API key management? How are API keys stored and rotated on civil servant devices?
- Is there a plan to integrate agenteam with AAAI's existing toolchain (e.g. Azure DevOps, GitHub Enterprise) or is it a standalone local tool?
- What is the definition of "success" beyond usage count — e.g. is there a target for number of projects using OBST within six months that would trigger a formal AAAI handover and resourcing decision?

---

## Solution 2: Hamster (tryhamster.com)

### Strengths

- Demonstrates that the market has validated the product category: AI-assisted structured brief generation with agentic CLI integration is a real, funded product type.
- Provides a useful benchmark for feature completeness and UX expectations when evaluating OBST+agenteam.

### Issues

#### Procurement and Data Sovereignty

1. **What** — Hamster is a US-domiciled SaaS. The outline itself states it is "highly unlikely ever to be assured for UK government use."

   **Why it matters** — UK government data must not be processed by services that have not passed security assurance (typically Cyber Essentials Plus at minimum, and often a full DSPCA or DSPT). US-domiciled SaaS is subject to CLOUD Act requests, which are incompatible with OFFICIAL-SENSITIVE data handling requirements.

   **Question / Decision required** — Given the outline's own assessment, is Hamster genuinely under consideration or included for completeness? If the latter, it should be noted in the document as explicitly out of scope to avoid consuming review bandwidth in future rounds.

#### Irreversible Decisions

2. **What** — Adopting a third-party SaaS for a governance-critical workflow creates dependency on a vendor's pricing, roadmap, and survival.

   **Why it matters** — Hamster is a startup-scale product. If it pivots, raises prices, or closes, AAAI's governance process would be interrupted with no migration path for existing structured artefacts.

   **Question / Decision required** — Not applicable given the procurement blocker, but noted for completeness: any SaaS adoption for a governance workflow should require data export capability and a documented exit plan.

---

### Open Questions

- None. The document is correct that this option is not procurable and it should be closed out.

---

## Solution 3: AI Playbook-aligned Prompt and Template Library

### Strengths

- Zero infrastructure footprint. No CLI tool to distribute, no managed device approval required, no API key management beyond what users already have through Copilot or Claude for Government.
- Works with already-procured, already-assured tools. The procurement and security assurance problem is already solved.
- Lower single-point-of-failure risk: a library of markdown documents in a version-controlled repository can be maintained by any team member with GitHub access.
- Audit trail is inherent: Git history records every change to prompt and template definitions.
- Immediately deployable — no development sprint required before first use.

### Issues

#### Maintenance and Staleness

1. **What** — The library's value depends on alignment with current UK Government AI Playbook stages, GDS Service Standard, data ethics requirements, and departmental policy. These evolve. The outline identifies this risk but does not propose a maintenance model.

   **Why it matters** — A prompt library that references superseded policy is not merely unhelpful — it is actively harmful if teams follow it and believe their artefacts are governance-compliant when they are not.

   **Question / Decision required** — Define a review cadence and owner before launch: (a) who is responsible for monitoring policy changes (AI Playbook updates, GDS Standard revisions)? (b) what is the trigger for a library review — a calendar schedule (quarterly), a policy publication event, or both? (c) should the library carry a "last reviewed against" date on each template so users can judge staleness themselves?

#### Distribution and Discoverability

2. **What** — The outline does not specify where the library is hosted or how civil servants find and access it. "Version-controlled and open-sourced" describes a GitHub repository, but GitHub is not the natural working environment for non-technical civil servants.

   **Why it matters** — A library that exists but is not discoverable or usable by the target audience is not a solution. If the target users are data scientists, GitHub access is likely fine; if the library is meant to serve a broader AAAI audience including policy and governance colleagues, a GitHub repository alone is insufficient.

   **Question / Decision required** — (a) Is GitHub the intended access point, or should the library also be published as a GOV.UK-adjacent microsite, SharePoint page, or internal wiki? (b) Does the open-source licence create any barrier to internal government use or adaptation?

#### CI/CD and Quality Assurance

3. **What** — The outline does not describe how prompt and template quality is validated before release. There is no equivalent of a test suite for prompt correctness or policy alignment.

   **Why it matters** — Unlike software, incorrect prompts fail silently — they produce plausible but non-compliant artefacts. Without a review gate, a pull request could introduce a prompt that omits a required governance step.

   **Question / Decision required** — Define a lightweight quality gate: (a) mandatory human review (PR approval from a named policy-aware reviewer) before any change to a template merges; (b) a changelog that records what changed and why, linked to the policy change that prompted it; (c) optionally, automated tests that send each prompt to the target AI tool and assert structural properties of the output (e.g. presence of required sections). Option (c) is more complex but meaningful for long-term reliability.

#### Operational Readiness

4. **What** — The library has no usage tracking. There is no mechanism to know whether teams are using the templates, whether they are producing better artefacts, or whether a template is causing systematic errors across multiple projects.

   **Why it matters** — The principal success metric in the outline is usage. For the prompt library, usage is entirely opaque unless a feedback mechanism is built in.

   **Question / Decision required** — Decide whether lightweight telemetry is needed: (a) a GitHub Discussions or Issues template for feedback; (b) a periodic survey of AAAI teams; (c) a structured log in the OBST repository convention (if OBST is also adopted) that records which templates were used. None of these require infrastructure, but at least one is needed to evaluate whether the solution is working.

---

### Open Questions

- Is the library intended to complement OBST+agenteam (i.e. the templates are the foundation that agenteam automates), or is it a standalone alternative? The outline's framing implies the latter, but the two are not mutually exclusive.
- Who has write access to the repository? An open-source repository with no defined maintainer team is a governance risk even for a document library.
- Should templates be made available in formats beyond markdown — e.g. Word/DOCX for civil servants who do not use Copilot or Claude for Government directly?
