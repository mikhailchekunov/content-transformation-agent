---
name: transform-news
description: Takes a raw technical news update (AI, software, hardware, industry) and transforms it into a short, human-sounding messenger hook that makes the reader want to engage immediately. Use when you need to turn a dry announcement into a compelling message that drives interaction.
when_to_use: When the user provides a technical news excerpt and wants it transformed into a short engaging messenger message or micro-lesson hook.
allowed-tools: read_file, write_file
---

# Skill: Technical News to Messenger Hook

## What This Skill Does
Takes a raw technical news update (AI, software, hardware, industry) and transforms it into a short, human-sounding messenger hook that makes the reader want to engage immediately.

## File Structure
```
/transform_news/
  ├── SKILL.md                  ← you are here
  ├── transform_agent.md        ← main agent: turns news into a hook
  ├── validator_agent.md        ← critic agent: scores and approves the hook
  └── rules_and_examples.md    ← style rules + rated examples (grows over time)
```

## How It Works

### Step 1 — Transform
Load `transform_agent.md`. Pass it the raw news text.
Before generating, the agent reads `rules_and_examples.md` to align with current style rules and learn from past examples.

### Step 2 — Validate
Load `validator_agent.md`. Pass it the original news and the generated hook.
The validator scores the hook across 4 criteria and returns PASS or FAIL with a breakdown.

### Step 3 — Iterate (if needed)
If FAIL: pass the news + validator feedback + previous draft back to the transform agent.
Repeat up to **3 iterations total**.
If the hook fails all 3 attempts, output the best-scoring draft with the label:
`⚠️ Best available draft (did not pass validation after 3 attempts)`

### Step 4 — Output
Present the final hook to the user, along with a short validator report:
```
HOOK:
<final message>

VALIDATOR REPORT:
Score: X/10
Attempts: N
[breakdown if relevant]
```

### Step 5 — Collect Feedback
After presenting the output, ask the user:
```
How would you rate this hook? (1-5)
```

Then apply the feedback rule:
- **5** → add to `rules_and_examples.md` under Good Examples with full metadata
- **1 or 2** → add to `rules_and_examples.md` under Bad Examples with full metadata
- **3, 4** → no action

**Cap rule (applies to both sections independently):**
Each section (Good Examples, Bad Examples) holds a maximum of **10 entries**.
When adding an 11th entry, delete the oldest one first (lowest position in the list = oldest).
This keeps the handbook compact and the examples relevant.

### Metadata Format (for saving examples)
```yaml
- date: <YYYY-MM-DD>
  topic: <model-release | tool-launch | industry-shift | regulation | research | hardware>
  score: <user rating>
  iterations: <number of attempts>
  input: "<original news excerpt>"
  hook: "<final hook text>"
```

## Input Format
```
NEWS: <paste raw technical news text here>
```

## Key Constraints
- The hook must match the language of the input (Russian in → Russian out, English in → English out)
- No bullet points, headers, or formatting inside the hook
- The hook is a messenger message, not a summary or a caption
- The validator is independent: it does not know how many attempts were made until the final output
