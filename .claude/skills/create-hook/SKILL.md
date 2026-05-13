---
name: create-hook
description: Transforms tech news text into a short, human-sounding messenger message designed to hook readers and drive engagement.
when_to_use: Use when the user provides a tech news article or text and wants a punchy, clickbait-style message for messengers like Telegram or WhatsApp.
allowed_tools:
  - read_file
  - write_file
  - edit_file
---

# SKILL: Clickbait Message Transformer

## Purpose

Use this skill when the user provides a tech news text and wants a short, human-sounding messenger message that makes people want to open and engage with the news.

---

## File Structure

```
content-transformation-agent/
├── SKILL.md              ← you are here (orchestration)
├── transform-agent.md    ← writes the message
├── validate-agent.md     ← reviews the message independently
└── examples.md           ← library of good and bad examples
```

---

## Pipeline

### Step 1 — Receive Input

Accept the news text from the user. No clarifying questions. Proceed immediately.

---

### Step 2 — Load Context (Optional)

Before running the transform agent, check `examples.md`.
If Good Examples or Bad Examples sections contain any entries, pass them to the transform agent as reference.
If both sections are empty, skip this step.

---

### Step 3 — Generate Message

Run `transform-agent.md` with:
- Original news text
- Examples from `examples.md` (if available)

Store the output as `current_message`. Store the iteration counter as `attempt = 1`.

---

### Step 4 — Validate Message

Run `validate-agent.md` with:
- Original news text
- `current_message`

Store the full validation result internally. **Do not show it to the user.**

---

### Step 5 — Iteration Logic

```
if VERDICT is PASS:
    → proceed to Step 6

if VERDICT is REVISE and attempt < 3:
    → attempt = attempt + 1
    → run transform-agent.md with:
         - Original news text
         - Feedback section from validation result
         - Examples from examples.md (if available)
    → store new output as current_message
    → go back to Step 4

if VERDICT is REVISE and attempt = 3:
    → compare all generated versions by total validator score
    → select the one with the highest total score
    → if scores are equal, select the last generated version
    → store selected version as current_message
    → proceed to Step 6
```

---

### Step 6 — Present Result

Show the user only `current_message`. Nothing else.

Do not mention:
- How many iterations were run
- Validator scores
- Whether the message passed or was the best of failed attempts
- Any internal process

---

### Step 7 — Request User Score

After showing the message, ask exactly this:

```
Rate this message from 1 to 5.
```

Wait for the user's response before proceeding.

---

### Step 8 — Save to Examples

**If score is 5 → save to Good Examples**
**If score is 1 or 2 → save to Bad Examples**
**If score is 3 or 4 → do nothing, end the pipeline**

#### Before saving — check the section size:

Open `examples.md` and count the number of examples in the target section (between the section's `_START` and `_END` comments).

```
if count = 15:
    → delete the oldest example in that section (the one with the earliest "Added" date)
    → then insert the new example

if count < 15:
    → insert the new example directly
```

#### Insert the new example using this format:

```
### Example [N]

**Added:** YYYY-MM-DD HH:MM
**User Score:** [1–5]

**Original News:**
[original news text]

**Generated Message:**
[current_message]

**Validator Scores:**
- Vibe-Check: [score]
- Utility: [score]
- Hook: [score]
- Accuracy: [score]

**Notes:**
```

Insert new examples at the **bottom** of the section, just before the `_END` comment.

Use the final validator scores (from the last validation run) when filling in Validator Scores.

For `[N]` — use the next available number in that section. If the section is empty, start at 1.

---

## What the User Sees

| Stage | Visible to user |
|---|---|
| Transform iterations | No |
| Validator scores | No |
| Iteration count | No |
| Final message | Yes |
| Score request | Yes |
| Confirmation of save | Optional — one line max |