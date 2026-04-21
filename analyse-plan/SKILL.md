---
name: analyse-plan
description: Analyse a written plan or spec for logical consistency, decomposition, ambiguity, and gaps. Reports findings plainly and asks up to five targeted questions only where the plan is genuinely unresolvable. Use when the user wants a focused plan review, not an exhaustive grilling like /grill-me.
---

Analyse the plan the user points you at — a file path, a document in the conversation, or an inline description — across four dimensions and report what you find.

## Dimensions

1. **Logical consistency** — internal contradictions, sequencing errors, assumptions broken by a later step, conflicts between requirements and design.
2. **Decomposition** — is the plan too big for a single PR? Where are the natural seams? If the plan already has phases or numbered steps, are they sensibly sized and ordered?
3. **Ambiguity** — vague requirements, handwave language ("somehow", "appropriate", "handle correctly"), decisions that haven't actually been made.
4. **Obvious gaps** — missing verification plan, unaddressed error cases, no rollback or migration story, affected consumers not mentioned, no testing strategy.

## How to analyse

Stay plan-internal by default. Do not proactively read every referenced file.

When a finding hinges on what the code actually does — "does this function exist?", "does this API behave this way?", "is that file structured as the plan assumes?" — read the referenced file to verify before reporting. Don't guess, and don't report speculation as a finding.

## What to report vs. what to ask

- Report clear findings plainly. A contradiction, a missing verification plan, or an oversized phase is a finding, not a question.
- Escalate to a question only when you cannot reasonably determine what is right without user input — typically because the plan is genuinely silent on a decision, or two reasonable interpretations exist.
- Soft cap: **five questions maximum**. If more unresolvable ambiguities exist, list the remaining areas in one line and ask the user which to prioritize.
- Do not ask for confirmation of findings you are confident about.

## Output

Produce a single message in this structure:

```
## Plan analysis: <plan title>

### Findings

**Logical consistency**
- <finding>

**Decomposition**
- <finding>
- Suggested split: <if relevant>

**Ambiguity**
- <finding>

**Obvious gaps**
- <finding>

### Questions (N/5)

1. <question>
2. <question>
```

Omit any dimension section that has no findings. Omit the Questions section entirely if nothing needs user input. Keep each finding to one or two sentences; cite the plan section or line it refers to.

## After the user answers

One short follow-up covering any new observations those answers surface. Then stop. Do not re-open closed findings and do not start a grill-me-style interview.
