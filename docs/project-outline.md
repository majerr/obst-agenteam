# OBST project outline

## Problem Statement

Teams in the Advanced Analytics and AI Division (AAAI) are struggling
to deliver high-value products. 

This is both a process- and a resource-gap. AAAI is a relatively new
division without mature process and governance procedures. Teams
struggle to get approval for projects to progress because the process
is not defined. Better governance and audit trails would likely
improve this, as would a narrative structure for project progress. 

The division resourcing is heavily weighted towards builders e.g. data
scientists and data engineers, who do often do not have the right
skills to produce the required artefacts, consider the right options
or surface the right questions to specialists (e.g. cyber,
infrastructure). Specialists are available for consultation, but have
limited bandwidth for engaging with new projects.

Teams of data scientists are spending weeks discussing user value,
user research, infrastructure, governance, devops and delivery, none
of which are core data science skills. In attempting to close this
gap, they are spending considerable time explaining and re-explaining
project ideas to non-technical specialists.

## Suggested solution(s)

A well-designed and accepted framework with supporting AI would:
* Make explicit the process by which teams should progress projects
* Clarify the required governance for AAAI projects
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
implementation readiness. 

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

agenteam is a proposed CLI tool to support OBST. It is envisaged as a
standalone runtime that facilitates the execution of a library of AI
subagents, skills and prompts, enabling users to execute predefined
combinations efficiently to support each stage of the OBST
process. For example, at the beginning of the outline stage a user
might execute a single agenteam command have a project outline
document (such as this one) reviewed by a team of AI agents
specialising in cyber security, technical delivery, infrastructure,
product ownership etc, a synthesis of those reviews written, and some
classes of actions initiated.

agenteam would be opinionated about delivery - it would exist to
support OBST; this would make development tractable by restricting the
scope to OBST processes. At the same time, it would enable users to
add their own agents, skills and prompts as needed.

agenteam could in theory be powered by any AI service that supports
subagent spawning and orchestration. A the time of writing, Claude
Code and Mistral Vibe support this capability. Claude Code is
currently in use by teams in AAAI; Vibe would require assurance and
procurement. In the near term Claude Code offers an immediate route to
implementation. In the future, depending on need and compatibility
between services, agenteam could potentially enable a user to switch between
services depending on the demands of the task.

It is envisaged that the development of OBST and agenteam would be
parallel, with agenteam scaffolding the development of OBST and
vice-versa. It is estimated that a single AI Engineer could produce a
first release of both OBST and agenteam in a month.

Both OBST and agenteam would be open-sourced and owned by the
developer (i.e. me, Matthew Roberts), enabling rapid early iteration
and proving and aligning with the GDS principle of open sourcing
wherever possible. A GPL from the start ensures that if/when AAAI is
ready to adopt, there will be nothing to prevent it from forking
and/or contributing, without an inappropriate level of governance for
an experimental project.

Success would be measured in terms of uptake. It is envisaged that
usage logs would be used to estimate the amount of human work done by
agenteam, in FTE.

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

### AI Playbook-aligned Prompt and Template Library

A curated library of prompts and document templates aligned to the
stages of the UK Government AI Playbook (stages 0–5) and the GDS
Service Standard. Each template would cover one required artefact
type (e.g. business case, AI risk assessment, privacy impact
assessment, model card) and include a structured prompt designed for
use with AI tools that are already procured and assured for UK
Government use (e.g. Microsoft Copilot, Claude for Government).

This option requires no custom software development and no new
procurement. Teams would use existing tools, guided by the
templates, to produce governance-compliant artefacts. The library
would be version-controlled and open-sourced. Governance alignment
and maintenance would depend on regular review against current
policy.

A standalone prompt and template library is unconstrained, but the
author(s) would need to make choices about the prompts and templates
they wanted to include. Their choices imply a framework but it is not
made explicit. Agenteam is fundamentally a prompt/template library
with the addition of a small runtime to ease execution - but with the
addition of an explicit framework to explain the choices of prompts,
and how they should be used.

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
* For an experimental project such as this, any proposed AI (or other
  technology) should be already in use by the team - assurance and
  procurement adds too great an overhead.
* All artefacts produced by the OBST process must be explicitly signed
  off by human SROs. Signoff points must be an explicit part of the OBST
  definition.
