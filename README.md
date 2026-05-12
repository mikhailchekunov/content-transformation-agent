# content-transformation-agent

A Claude Code skill that turns dry technical news into short, human-sounding messenger hooks that make people want to reply.

## What It Does

Takes a raw news excerpt (AI, software, hardware, industry) and produces a 3-5 sentence message that sounds like a sharp colleague texting you something useful — not a newsletter, not a bot.

## Architecture

The skill lives in `.claude/skills/transform_news/` and consists of four files:

```
SKILL.md                ← pipeline entrypoint and orchestration logic
transform_agent.md      ← writes the hook from the raw news
validator_agent.md      ← independently scores and approves the hook
rules_and_examples.md   ← self-growing style handbook
```

### Transform Agent

Reads `rules_and_examples.md` before writing to align with current style rules and learn from past rated examples. Produces a messenger hook that meets three hard requirements:

- **100% human tone** — no AI markers, no filler words, no synthetic phrasing
- **Instant utility** — concrete and specific value, quantified where possible (time saved, task automated, specific impact)
- **Compelling CTA** — a specific action or time-framed offer that makes replying feel natural ("want me to show you how in 30 seconds?")

### Validator Agent

An independent critic that sees only the hook and the original news — it has no knowledge of who wrote it or how many attempts were made. Scores across four criteria (max 10 points):

| Criterion | Points |
|-----------|--------|
| Human Tone | 0-3 |
| Instant Utility | 0-3 |
| Hook / CTA | 0-2 |
| Brevity & Format | 0-2 |

**Hard Blocking Rules** are applied before the score check. Any single violation → automatic FAIL, regardless of total:
- Human Tone < 2 (sounds synthetic)
- Instant Utility < 2 (value is too abstract)
- Hook / CTA < 2 (CTA is weak or absent)

Threshold for PASS: 7+/10, with all blocking rules satisfied.

### Iteration Loop

If the hook fails, the validator's specific feedback is passed back to the transform agent for a rewrite. Maximum 3 attempts. If all fail, the best-scoring draft is output with a warning label.

### Self-Growing Handbook

After each run, the user rates the hook 1-5. High and low scores are saved as examples:

- **Rating 5** → added to Good Examples
- **Rating 1-2** → added to Bad Examples
- **Rating 3-4** → no action

Each section holds a maximum of 10 entries. When the 11th is added, the oldest is dropped. This keeps the handbook compact and the examples fresh — the more the skill is used, the better it gets.

## Usage

```
/transform_news

NEWS: <paste raw technical news here>
```

The skill matches the language of the input — Russian news produces a Russian hook, English news produces an English hook.
