---
name: analyse-plan
description: Analyse a written plan or spec for logical consistency, decomposition, and completeness. Reports findings plainly and asks up to five targeted questions only where the plan is genuinely unresolvable. Use when the user wants a focused plan review, not an exhaustive grilling like /grill-me.
---

Analyse the plan the user points you at — a file path, a document in the conversation, or an inline description — across three dimensions and report what you find.

## Dimensions

1. **Logical consistency** — internal contradictions, sequencing errors, assumptions broken by a later step, conflicts between requirements and design.
2. **Decomposition** — is the plan too big for a single PR? Where are the natural seams? If the plan already has phases or numbered steps, are they sensibly sized and ordered?
3. **Completeness** — anything under-specified, on a spectrum from vague language to fully absent. Includes handwave wording ("somehow", "appropriate", "handle correctly"), decisions that haven't actually been made, and missing sections the plan should have but doesn't: verification plan, error cases, rollback or migration story, affected consumers, testing strategy.

## How to analyse

Stay plan-internal by default. Do not proactively read every referenced file.

When a finding hinges on what the code actually does — "does this function exist?", "does this API behave this way?", "is that file structured as the plan assumes?" — read the referenced file to verify before reporting. Don't guess, and don't report speculation as a finding.

## What to report vs. what to ask

- Report clear findings plainly. A contradiction, a missing verification plan, or an oversized phase is a finding, not a question.
- Escalate to a question only when you cannot reasonably determine what is right without user input — typically because the plan is genuinely silent on a decision, or two reasonable interpretations exist.
- Soft cap: **five questions maximum across the whole analysis** (initial questions plus any follow-ons, counted cumulatively). If more unresolvable ambiguities exist after you hit the cap, mention the remaining areas in the final summary rather than asking more questions.
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

**Completeness**
- <finding>

### Questions (N/5)

1. <question>
2. <question>
```

Omit any dimension section that has no findings. Omit the Questions section entirely if nothing needs user input. Keep each finding to one or two sentences; cite the plan section or line it refers to.

## After the user answers

Do not stop after the first batch of answers. Follow these two steps until the skill is complete:

1. **Ask follow-on questions, one at a time.** Answers frequently resolve one ambiguity while surfacing another, or give you enough context to raise a question you had previously deferred. If more questions remain — new ones, deferred ones, or ones that were cut from the initial batch to stay concise — ask the next one. Ask them one at a time from this point on (not in batches), and keep the running total at five or fewer. Only stop asking when there are genuinely no more worthwhile questions, or when you have asked five in total.

2. **Produce a summary once questions are done.** When no more questions remain (or the five-question cap has been hit), end with a summary in this structure:

   ```
   ## Analysis summary

   **Resolved** — one line per answered question, capturing the decision that was reached.

   **Still open** — anything unresolved: items the user deferred, questions cut because of the cap, areas flagged for later.

   **Recommendation** — one short sentence: is the plan ready to implement, does it need revisions, or does it need more work before execution?
   ```

   Omit any section of the summary that has nothing to report. Do not re-open closed findings, and do not start a grill-me-style interview.
