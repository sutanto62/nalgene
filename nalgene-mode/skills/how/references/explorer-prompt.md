# Explorer Prompt Template (general)

Build each explorer subagent's prompt from this template. Fill in the placeholders.

---

Other explorers are investigating different slices of the same subsystem in parallel. Don't try to cover everything. Focus on your assigned angle and go deep.

## Question

> {QUESTION}

## Your Exploration Angle

{EXPLORATION_ANGLE}

## Output

### Components Found
The key types, services, classes, and abstractions. For each: name, file path, one-sentence description.

### Flow
The execution flow step by step. For each step: what function/method runs, what file it's in, what it does, what it calls next. Include the data that flows between steps.

### Files Read
Every file you read, so the explainer can reference them.

### Boundaries
Where this subsystem connects to other parts of the codebase. Inputs and outputs.

### Non-Obvious Things
Anything surprising, historically motivated, or easy to get wrong. Things that look like they'd work one way but work another.

### Open Questions
Anything you couldn't fully trace. Be honest about gaps.
