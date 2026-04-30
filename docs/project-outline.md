# OBST project outline

## Problem Statement

Teams are jumping into code too early.

Coding agents such as Claude Code work best when a repository is
strongly aligned on the authors intention, with consistent specs,
tests and documentation.

Within such a repository, coding becomes the easy part of
development. The harder part is defining the author's intention and
aligning all the repository artefacts. This process is currently
unstructured, largely ungoverned, and frequently overlooked. This can
lead to delayed decisions, rework and longer delivery times.

Product teams in the Advanced Analytics and AI division (AAAI)
could realise substantial efficiencies from a predefined method of
defining and aligning on a project's intention.

## Suggested solution(s)

A well-designed and accepted delivery framework with supporting AI would:

* Make explicit the process by which teams should progress projects
* Focus on explicit decision making and comprehensive
  record keeping to facilitate project governance.
* Emit it's own audit trail
* Provide a narrative structure for project management, making it
  easier to quickly understand project progress
* Surface specialist questions and viewpoints early (before human
  specialists are consulted)
* Align repositories on intention, enabling better use of coding
  agents for implementation
* Structure the use of AI coding and development agents to support the
  process at specific points by
  - drafting artefacts
  - challenging ideas
  - providing specialist perspective

### OBST+agenteam

OBST (Outlines, Briefs, Specs, Tests) is an idea for an AI-first
delivery framework to accelerate teams' progress from idea to
implementation readiness. agenteam is a proposed CLI tool to support
OBST.

#### OBST
OBST is a _framework_ not a technology. It is the definition of a
sequence of stages that a project goes through that would be well
supported by AI tools. The stages are outlines, briefs, specs and
tests. Each stage has defined inputs and outputs, and the output of
one stage licenses the beginning of the next. Each stage is punctuated
by reviews, redrafts, and human signoff before implementation. Whilst
artefacts might be AI-generated and AI-reviewed, all artefacts must
also be reviewed and explicitly accepted by humans. However, the
framework is agnostic about the AI technology used to support it;
indeed it could be followed without AI, given sufficient resource.

The OBST framework would exist as a series of documents published on
the web (Github Markdown or HTML Github pages). These might include a
specification of stages, inputs, outputs etc, and also tutorials and
worked examples. These would also demonstrate how OBST instatiates the
principles of the UK Government AI Playbook for AI projects, data
ethics requirements and other required governance.

#### agenteam

agenteam is envisaged as a library of AI subagents, skills and
prompts, with a minimal standalone CLI runtime to standardize their
execution with claude code. Users would invoke commands to execute
predefined prompts against Claude Code, which would spawn predefined
subagents endowed with predefined skills to support each stage of the
OBST process.

For example, at the beginning of the outline stage a user might
execute a single agenteam command have a project outline document
(such as this one) reviewed by a team of AI agents specialising in
cyber security, technical delivery, infrastructure, product ownership
etc, a synthesis of those reviews written, and some classes of actions
initiated. 

Subagent definitions would be adjustable by users to align them to
required standards specific for the project - for example, UK civil
servants might need their subagents to be aligned to the GDS service
standard and the UK Government AI Playbook. This would also enable
internal integrations for organisations adopting OBST+agenteam.

agenteam would be opinionated about delivery - it would exist to
support OBST; this would make development tractable by restricting the
scope to OBST processes. At the same time, it would enable users to
add their own agents, skills and prompts as needed.

agenteam could in theory be powered by any AI service that supports
subagent spawning and orchestration. However at the time of writing,
Claude Code and Mistral Vibe support this capability. Claude Code is
currently in use by teams in AAAI and has been assured for use with
Official-Sensitive data; Vibe would require assurance and
procurement. In the near term, therefore, only Claude Code offers an
immediate route to implementation. Were Vibe, or any other sub-agent
capable agent to become available in the future, however, agenteam
would not be locked into claude code.

It is envisaged that the development of OBST and agenteam would be
parallel, with agenteam scaffolding the development of OBST and
vice-versa. It is estimated that a single AI Engineer could produce a
first release of both OBST and agenteam in one or two months of work.

Both OBST and agenteam would be open-sourced. It is envisaged that
ownership would sit with AAAI SLT, if this project is taken forward.

Success would be measured in terms of uptake; that might mean the
number of projects adopting OBST/agenteam, or a more sophisticated
metric depending on AAAI SLT expectations.

## Rejected Solutions
  
### Hamster (tryhamster.com)

Existing service to generate briefs and plans. Hamster is an AI-native
product planning platform that generates structured briefs and task plans
collaboratively, integrating with tools like GitHub, Linear, Jira, and
Notion, and with AI coding agents via a CLI. It targets founder-to-enterprise
teams and bridges human direction with AI execution.

Hamster should be considered a market reference point, as an example of what
currently exists in the market. It is US-domiciled SaaS and therefore
highly unlikely ever to be assured for UK government use; the
procurement process would require months of work.

This solution is rejected because:
* The assurance and procurement processes to bring US-domiciled
  startup software into UK government would very likely cancel the
  benefits of usage.

### Standalone AI Playbook-aligned Prompt and Template Library

A curated library of prompts and document templates aligned to the UK
Government AI Playbook and the GDS Service Standard. Each template
would cover one required artefact type (e.g. business case, AI risk
assessment, privacy impact assessment, model card) and include a
structured prompt designed for use with AI tools that are already
procured and assured for UK Government use (e.g. Microsoft Copilot,
Claude for Government).

Advantages:
This option requires no custom software development and no new
procurement. Teams would use existing tools, guided by the
templates, to produce governance-compliant artefacts. The library
would be version-controlled and open-sourced. Governance alignment
and maintenance would depend on regular review against current
policy.

Disadvantages: 

Both the Playbook and the Service Standard are structured around
criteria, not delivery. They are not designed to do the work that the
problem requires - instead they are designed to ensure the UK
government digital projects meet defined standards, operating as
checklists, not processes. Whilst the service standard specifies
gates, they are retrospective and do not suggest _how_ teams should
deliver them.

This means that a standalone prompt and template library would be
relatively unconstrained in how it could be used in practice. That has
at least two consequences:
* there is a strong likelihood that it would contain generic prompts
  that would produce less valuable artefacts; and
* the author(s) would need to work to an implicit usage plan to make
  choices about the prompts and templates in the library. Their
  choices would imply a framework but it would not be documented as
  OBST would be.

Technically, the solutions suggested do not have the capability to run
agents as sub-processes, making it difficult to keep separate opinions
from different perspectives.

This solution is rejected because:
* Creating a library of prompts is also part of the agenteam idea, but
  there it is both more constrained, facilitating development, and
  more flexible, meeting the users' needs.
* agenteam also supports the GDS Service Standard and the AI Playbook
  via its subagent definitions.
* As an open source project, it would restrict usage to target prompts
  to a particular model.

### OBST framework as a standalone

The OBST framework, delivered as a github markdown, could be completed
relatively quickly and allow teams the freedom to implement it's
stages with whatever tools they wish. This would be a more explicit and
coherent offering than a standalone prompt/template library.

However, authoring and orchestrating prompts would:
  * Require substantial work, 
  * be duplicated across teams
  * effectively result in multiple divergent implementations of agenteam

## Aims

The aim of this project is to define and develop a framework and
supporting software for immediate use in AAAI but also releaseable as
open-source software. The principle success metric would be usage,
e.g. if AAAI projects adopted the framework, and it attracted users
and developed a community more generally.

## Context

* The immediate context for the use of this framework is AAAI - a team
  of UK Civil Servants. 
* All artefacts produced by the OBST process must be explicitly signed
  off by human SROs. Signoff points must be an explicit part of the OBST
  definition.
