# OBST &#x1F352;

A fruitful delivery framework

## Summary

OBST stands for Outlines, Briefs, Specs, Tests. It is an AI-first
delivery framework for small teams that aims to increase velocity in
the stages between your first idea for a project and being ready to
code.

You _could_ follow OBST without AI. Really it is only existing best
practice, on steroids. But for small teams, complying with _all_ the
best practice that has accreted for software development is very
onerous. That is one reason why OBST exists.

Outlines, Briefs, Specs, and Tests are the key stages of the
framework. Each one produces artefacts and decision records that
enable the following stage. So OBST is also a governance and project
management tool - it provides some assurance that your project has
considered everything it needs to, and it gives you an consistent
narrative framework that tells you how your project is progressing.

OBST deliberately frontloads much of the thinking and investigation
needed to complete a project. This is not the same as a waterfall
approach - OBST has been described as "Agile for an AI world" - but it
does provide _some_ assurance for project leaders and stakeholders
that your coding agents won't be producing slop.

**Outlines** describe the problem you are solving, a rationale for
your approach, the components of your solution, and an initial
sequence of high-level milestones to build those
components. Generating an outline falls in two stages: deciding on
your approach, and identifying your components and
milestones. High-level milestone descriptions include options,
questions and issues that will require resolution in order to specify
the component completely.

**Briefs** describe the milestones in more detail. They provide
answers to the questions identified at the outline stage. An initial
set of briefs is generated to match the initial sequence of milestones
in the outline, but more may be added as the project progresses.

**Specs** are generated from the descriptions contained in
briefs. They are the reference documentation for the component.

**Tests** are generated based on the specs. They provide assurance
that the component you are building meets the spec.

With all these artefacts in your repository, your coding agents are
provided with context that is strongly aligned on your
intention. Human involvement, and model expense, decreases through the
stages.
