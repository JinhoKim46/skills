---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user until you reach a shared understanding, but the user answers only what only they can answer. Map the plan as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled. Sort each frontier item into exactly one of three lanes:

1. **Fact**: answerable from the environment (filesystem, code, docs, tools). Dispatch a sub-agent to look it up. Never ask the user for a fact.
2. **Agent-decided**: a non-critical decision. Agents settle it using the protocol below. The user is not asked.
3. **User-decided**: a critical decision. Put it to the user this round.

## Critical or not

A decision is **critical** (user lane) if any of these hold:

- Reversing it later is expensive or impossible: data migrations, public APIs, schemas, anything shipped, published, or sent to others.
- It changes scope, cost, timeline, or who is affected, or touches security, money, or legal exposure.
- It depends on the user's priorities, taste, or values, which the environment cannot reveal.
- It has an outward-facing effect: sending, publishing, deleting, paying.
- The agents disagree after the challenge step, or neither option is clearly better.

Everything else is **non-critical**: reversible, local, and settled by evidence or convention. Apply the list honestly. Don't call something non-critical to skip asking, and don't call it critical to avoid deciding.

## Agent-decided protocol

Run this for each non-critical frontier item. It is bounded: one proposal, one challenge, at most one revision.

1. **Propose.** A sub-agent proposes the best option, with a one-line rationale and the strongest alternative. Sub-agents do not see this conversation, so paste in the question, the settled decisions it depends on, and the facts gathered so far.
2. **Challenge.** A second, fresh sub-agent receives the question, the proposal, and the same context. It must argue for the alternative or find a flaw or hidden dependency. It may instead answer `critical: <reason>` if the list above applies.
3. **Resolve.**
   - If the challenge fails to change the answer, the proposal is settled.
   - If the challenge lands, the proposer revises once, and the revision is settled.
   - If it is still contested, or the challenger flagged it critical, move the item to the user lane. Do not run a third round.

If sub-agents are unavailable, do steps 1 and 2 yourself as two explicit passes, arguing each side in writing before you decide.

## Format a round

Ask the user-lane items, numbered. Give your recommended answer for each. Word each question so "yes" accepts your recommended answer. Wait for the user's answers before the next round.

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Then list the agent-decided items from this round so the user sees them. Don't ask about them:

```
🤖 **A1** - **<decision title>**: <decision>. Why: <one or two lines>. Object to reopen it.
```

## Rules

- **Don't ask the user what agents can decide.** Don't ask "shall I go ahead?" or present a menu of options that the protocol can settle.
- **If a round has no user-lane items, don't wait.** Settle the agent items, recompute the frontier, and keep going.
- **Agent decisions bind later rounds** unless the user objects. An objection reopens that branch, and everything downstream of it moves back into the frontier.
- **Promote on new facts.** An item can move to the user lane at any point if new information makes it critical.
- **A running fact lookup is an unsettled prerequisite.** Only the items downstream of it wait. Ask the rest of the frontier now.
- **Questions depending on an open user-lane question** belong to a later round, not this one.

## Done

The session is done when the frontier is empty: every branch visited, and nothing left silently assumed. Before you ask for confirmation, give one summary of every agent-decided item, so the user can review them in a single pass. Do not act on the design until the user confirms the shared understanding.
