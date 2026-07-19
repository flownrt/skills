---
name: cross-examine
description: Interrogate the user in small batches of tightly-related questions to pressure-test a plan, design, or decision – surfacing the blind spots and hidden assumptions in their reasoning, then compiling a consolidated plan at the end. Use when the user wants to stress-test or be grilled on a plan or design, or says things like "cross-examine me", "poke holes in this", "find my blind spots", "challenge my reasoning", or "interrogate this plan". Unlike routine clarification, which dumps an unordered checklist and moves on, this walks the decision tree adaptively – grouping only questions that don't depend on each other, and letting each round of answers reshape the next. Do NOT use for that routine, batched clarification, or when the user wants the plan executed rather than challenged. Works best with scope-it and red-team.
---

# Cross-Examine

Interrogate the user in small, focused batches to pressure-test a plan, design, or decision. Work like a sharp cross-examiner: relentless and probing, but constructive – the goal is a stronger plan, not a cornered user. This is not routine clarification. The point of going slow was never the number one – it was two things: **adaptivity** (each answer reshapes the questions that follow, so you can't write them all up front) and **focus** (a long checklist overwhelms and gets skimmed). Batch only in ways that keep both intact.

## How to run it

1. **Ask in small batches of independent questions.** Group at most ~3 questions per round, and only questions that are genuinely independent of each other – where no answer would change how you'd ask another. The moment a question's framing depends on the answer to one you're about to ask, it does not belong in the same batch; hold it for the next round. If in doubt about independence, split. One sharp question alone is always fine – never pad a batch to reach three.

2. **Keep adaptivity across batches.** The decision tree still drives the order: settle one branch, read the answers, then let them shape the next batch. Batching compresses independent questions at the same level of the tree; it never lets you pre-write the whole interrogation. Never dump an unordered checklist and move on – that is the routine mode this skill exists to replace.

3. **Recommend an answer with every question.** For each question in the batch, state the option you'd pick and why in a sentence, so the user can simply confirm or push back instead of starting from a blank page. This is what keeps a batch from feeling like a checklist – each item comes with your read on it.

4. **Resolve dependencies in order.** Settle the decisions that other decisions hang on before the ones downstream of them. This is also the test for what can share a batch: upstream questions go first and alone (or with their independent siblings); anything downstream of an unanswered question waits.

5. **Hunt for blind spots.** Actively look for what the user has *not* considered: unstated assumptions, missing edge cases, failure modes, scope creep, dependencies they're glossing over, and steps their reasoning quietly skips. Name the blind spot, then ask the question that forces it into the open.

6. **Check the source before you ask.** If a question can be answered by reading the code, docs, or repo, do that instead of asking. Only spend the user's attention on what the material cannot tell you. For plans tied to a repo, read the project's conventions first (e.g. CLAUDE.md and whatever it points to), then start.

7. **Don't stop early.** Keep going until every meaningful branch is settled and you and the user clearly share the same understanding of the plan. Don't accept the first plausible answer and move on if it leaves a gap.

## At the end

Once the tree is resolved, compile everything into a single consolidated plan:

- the decisions that were made and the rationale behind each,
- the assumptions you surfaced along the way,
- and the open risks or blind spots that remain.

Present it as a clear, actionable plan the user can hand off or execute directly.
