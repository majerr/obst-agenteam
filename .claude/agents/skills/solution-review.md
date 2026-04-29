---
name: solution-review
description: Output format, constraints, and behavioural norms for reviewing solution outline documents
---

## Output format

Produce a single markdown file with the following YAML frontmatter:

```yaml
---
agent: {your identity}
document_reviewed: {path to the reviewed document}
date: {today's date in ISO 8601 format}
---
```

Structure the review body as follows:

### Summary
A short (2–4 sentence) summary of your understanding of the brief for this review and the contents of the document.

## Solutions

For each suggested solution provide the following sections:

### Strengths
Aspects of the idea that are well-considered and should not be changed without careful thought. Be specific — vague praise is not useful.

### Issues
For each issue found, write a numbered entry with three parts:
1. **What** — what the issue is
2. **Why it matters** — why resolving it now (before proceeding) is important
3. **Question / Decision required** — the specific question or
   decision that must be resolved before sign-off. For decisions,
   always list the alternatives and their consequences.

Organise issues under headings that reflect your specialism.

### Open Questions
A concise list of any additional questions that do not fit the above
categories but should be answered before the project proceeds. Include
a short synopsis of why each open question matters.

## Constraints and behaviour

### Edits
- Never modify any file except the review file you are writing.

### Web Search
- During your review, use WebSearch and WebFetch only to verify your own assumptions or check that your knowledge is current. Do not search speculatively.
- If your review surfaces factual questions which you could answer by searching the web, you should attempt to do so and include the answer in your review.

### Style
- Be upfront about uncertainty. If an outcome cannot be confidently
  predicted, say so and suggest an experiment or spike to resolve the
  question.
- Be pragmatic. The goal is to evaluate and strengthen project ideas,
  not to achieve perfection. Do not create unnecessary hold-ups.
- Be direct and specific. Cite the part of the document you are commenting on.
- Be concise. Avoid vague praise or filler commentary.
- Remember that you are an advisor, and that project decision makers
  may be able to explain away your concerns. Phrase your review
  accordingly - do not pre-suppose the kind of answer that will be
  needed.

### Errors
- If you encounter rate-limits (429 errors, 504 errors, 'please try again later', or similar), **do not retry**. Report the error and stop.
- If the document is a binary or non-text format you cannot read, state this clearly and stop.
