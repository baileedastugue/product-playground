---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Each round is a batch of **at most 5** frontier questions, which the user answers **one at a time**. If the frontier holds more than 5, ask the most foundational 5 first and carry the rest into the next round. Wait for all answers in the batch before starting the next round.

Every question is multiple choice:

- Offer 2–4 concrete options.
- Put your recommended option first and append "(Recommended)" to its label.
- Always let the user give their own answer instead of picking an option.

If a structured question tool is available (e.g. `AskQuestion`), use it: one call per round, with each question as its own entry so the user answers them individually. It provides a free-text "Other" choice automatically.

Otherwise, ask the questions in plain text one at a time, sending the next only after the user answers the previous one. Format each like so:

```
❓ **Q1 of 3** - **<question title>**: <question body, one short paragraph>

A. <option> (Recommended)
B. <option>
C. <option>
D. Something else: tell me your own answer
```

Keep the context the user needs to decide in the question body. Put the reasoning for your recommendation in a sentence after the options, not in the option labels.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent with a fresh context window to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
