# content-transformation-agent

A Claude Code skill that turns dry technical news into short, human-sounding messenger hooks that make people want to reply.

## Skills

### `/create-hook` — Current

Takes a raw tech news text and produces a 1–2 sentence message that sounds like a sharp colleague dropping something useful in a group chat — not a newsletter, not a bot.

#### Architecture

The skill lives in `.claude/skills/create-hook/` and consists of four files:

```
SKILL.md              ← pipeline entrypoint and orchestration logic
transform-agent.md    ← writes the hook from the raw news
validate-agent.md     ← independently scores and approves the hook
examples.md           ← self-growing library of good and bad examples
```

#### Transform Agent

Reads `examples.md` before writing to calibrate tone and learn from past rated messages. Produces a messenger hook with a strict structure:

- **Sentence 1 — The Hook:** leads with the most surprising or counterintuitive thing from the news. Not what happened — why anyone should care.
- **Sentence 2 — The CTA:** a specific, actionable offer. The reader should feel like they're about to receive something useful, not be asked a question.

Hard rules for the message:
- No yes/no questions, no "have you tried…?" endings
- No filler: "simply", "easily", "just", "quickly"
- No hype: "game-changing", "revolutionary", "cutting-edge"
- No em dash, no rhetorical fragments
- No passive voice or press-release energy
- Language matches the input — Russian news → Russian hook

#### Validator Agent

An independent critic that sees only the hook and the original news. Scores across five criteria (max 15 points):

| Criterion | Points |
|-----------|--------|
| Vibe-Check (Human Tone) | 1–3 |
| Utility (Clear Value) | 1–3 |
| Hook / CTA | 1–3 |
| Accuracy | 1–3 |
| Grammar & Word Order | 1–3 |

**Threshold for PASS:** all scores ≥ 2 AND total ≥ 13. Any single score of 1 → automatic REVISE.

#### Iteration Loop

If the hook fails, the validator's specific feedback is passed back to the transform agent for a targeted rewrite. Maximum 3 attempts. If all fail, the best-scoring draft is output.

#### Self-Growing Examples Library

After each run, the user rates the hook 1–5. High and low scores are saved to `examples.md`:

- **Rating 5** → added to Good Examples
- **Rating 1–2** → added to Bad Examples
- **Rating 3–4** → no action

Each section holds a maximum of 15 entries. When the 16th is added, the oldest is dropped.

#### Usage

```
/create-hook

<paste raw tech news here>
```

---

### `/create-fast-hook` — Lightweight

A single-agent, no-pipeline alternative to `/create-hook`. Skips the validator and examples library — just one fast pass that produces a punchy one-sentence hook with a CTA.

#### When to use

Use this when you want a quick result without the multi-step validation loop. Good for rapid iteration or lower-stakes content where a single sharp pass is enough.

#### Architecture

The skill lives in `.claude/skills/create-fast-hook/` and is a single file:

```
SKILL.md    ← all logic: rules, examples, and execution in one place
```

#### How it works

The model reads the source text, identifies the most interesting angle for the reader, and writes one sentence that:

- Captures attention immediately
- Sounds like a message from a real person, not a bot
- Ends with a CTA so the reader knows why it matters to them

Hard rules:
- One sentence only
- No em dash, en dash, or hyphen
- No AI-sounding phrasing
- Language matches the input — Russian news → Russian hook

#### Usage

```
/create-fast-hook

<paste raw news or any content here>
```

---

### `/transform_news` — Deprecated

> **Deprecated.** Use `/create-hook` instead.

The original version of this skill. Still functional but no longer maintained. Superseded by `/create-hook`, which has a more precise transform agent, a 5-criterion validator (vs. 4), a larger examples library (15 entries vs. 10), and cleaner pipeline execution.

The skill lives in `.claude/skills/transform_news/`.
