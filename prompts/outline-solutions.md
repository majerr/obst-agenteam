Please conduct a review of `docs/project-outline.md`.

It is a description of the idea for this project, and includes:
* A problem statement
* One or more suggested solution
* Some aims for the project
* Some context for the project (e.g. constraints on development)

The purpose of this review is to help decide on a solution or
problem-solving approach. **The aim at this stage is to elicit
sufficient detail to make an informed choice of approaches.** It is
not to make that choice.

## First, consider the problem statement. 

Carefully research the domain.

* If the problem statement describes a known problem, or class of
  problems, to which known solutions exist, and they are not listed,
  please identify them.
* If the problem statement does NOT describe a known problem or class
  of problem, consider whether there are any obvious approaches to it
  based on your domain research that are not already described.
* Edit the document as necessary to include your most promising
  suggestions, up to a maximum of 4 in total, including those already
  in the document. **ONLY ADD GENUINE ALTERNATIVES**; it is better to
  recognise that few solutions exist than to include filler.

## Second, consider the suggested solutions.

1. For each suggested solution, research any links or paths to
   thoroughly understand the idea and how it solves the problem.

2. Use available agents to review each solution from as many
   perspectives as you think appropriate at this stage.
   * Agents should read the document
   * Agents should research any links or paths to thoroughly
     understand the idea and how it solves the problem
   * Agents should identify any 
	 - gaps, 
	 - errors,
	 - risks, or
	 - issues
	 from the perspective of their specialism
  * Agents should provide **only enough** information to enable
    decision between the suggested approaches.
  * Agents should write their reviews to docs/reviews, using a
    filename like:
    yyyymmdd-{review-id}-{review-name}-{agent-name}.md. **Make sure
    they all use the same review id and review name, and that those
    are unique in the docs/reviews directory**
  * Run agents in parallel to complete reviews faster.
  * If an Agent tool call fails with a rate limit error (429) or a
	timeout error (504), **DO NOT RETRY**. Instead, end all tool
	calls, stop immediately, and report: "ERROR: {subagent_name}
	encountered {error_name} ({error_code})"


When the agents have completed their reviews, please synthesize them
into a single document under docs/reviews, using the same naming
convention: yyyymmdd-{review-id}-{review-name}-{agent-name}.md. For
the synthesis, 'agent-name' should be 'synthesis'.

Include in your synthesis:
  * any additional solutions you identified. Include and you added to
    the outline document **and** any you did not. Say how you decided
    which to add.
  * advantages and disadvantages of suggested solutions (those in now in the outline document).
  * questions that humans need to answer before a decision on the
    appropriate solution is possible
  * recommended edits to the project outline, including edits that
    humans need to make, in order to support a decision on the
    approach.

