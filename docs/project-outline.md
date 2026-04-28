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
by reviews, redrafts, and signoff before implementation, with all
aspects potentially supported by AI. However, the framework is
agnostic about the AI technology used to support it; indeed it could
be followed without AI, given sufficient resource.

The OBST framework would exist as a series of documents published on
the web (Github Markdown or HTML Github pages). These might include a
specification of stages, inputs, outputs etc, and also tutorials and
worked examples. These would also demonstrate how OBST instatiates the
principles of the UK Government AI Playbook for AI projects, data
ethics requirements and other required governance.

agenteam is a proposed CLI tool to support OBST. It is envisaged as a
thin wrapper around a library of AI subagents, skills and prompts,
enabling users to execute predefined combinations efficiently to
support each stage of the OBST process. For example, at the beginning
of the outline stage a user could have a project outline document (such as this
one) reviewed by a team of AI agents specialising in cyber security,
technical delivery, infrastructure, product ownership etc.

agenteam would be opinionated about deliver - it would exist to
support OBST; this would make development tractable by restricting the
scope to OBST processes. At the same time, it would enable users to
add their own agents, skills and prompts as needed.

agenteam could be powered by any AI service that supports subagents. A
the time of writing, this appears to be restricted to Claude Code and
Mistral Vibe. Claude Code is assured for UK government use; Vibe would
require assurance and procurement. Ultimately, agenteam could enable a
user to switch between services depending on the demands of the task
but the near term, Claude Code offers an immediate route to
implementation. 

It is envisaged that the development of OBST and agenteam would be
parallel, with agenteam scaffolding the development of OBST and
vice-versa. It is estimated that a single AI Engineer could produce a
first release of both OBST and agenteam in a month.

Both OBST and agenteam would be open-sourced and owned by the
developer (i.e. me, Matthew Roberts), but if successful might be
governed by a steering group from AAAI.

  
### Hamster (tryhamster.com)

Existing service to generate briefs and plans. Hamster is an AI-native
product planning platform that generates structured briefs and task plans
collaboratively, integrating with tools like GitHub, Linear, Jira, and
Notion, and with AI coding agents via a CLI. It targets founder-to-enterprise
teams and bridges human direction with AI execution.

Hamster is included as a market reference point, as an example of what
currently exists in the market. It is US-domiciled SaaS and therefore
highly unlikely ever to be assured for UK government use; the
procurement process would require months of work.

## Aims

The aim of this project is to define and develop a framework and
supporting software for immediate use in AAAI but also releaseable as
open-source software. The principle success metric would be usage,
e.g. if AAAI projects adopted the framework, and it attracted users
and developed a community more generally.

## Context

The immediate context for the use of this framework is a team of UK
Civil Servants. Any use of AI or other technology must be security
assured and procurable.
