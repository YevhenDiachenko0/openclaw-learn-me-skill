---
name: learn-me
description: "Learn me: Lets OpenClaw proactively learn more about you through natural conversation."
version: 0.1.0
user-invocable: false
disable-model-invocation: false
metadata: {"openclaw":{"emoji":"💬","always":true,"homepage":"https://github.com/YevhenDiachenko0/learn-me","requires":{"bins":["openclaw"]}}}
---

# Learn Me

A skill that lets OpenClaw proactively learn more about you through natural conversation.

**Requires**: OpenClaw memory must be enabled. All learned facts are stored via the built-in memory system.

## Quick Reference

| Situation | Action |
|-----------|--------|
| User reveals something new | Note a future question direction in `memory/learning-user.md`. Do NOT follow up now. |
| User shows energy on a topic | Note it in `memory/learning-user.md` as a direction to explore later. |
| Cron fires | If user is busy — skip. Otherwise pick a direction from `memory/learning-user.md`, ask naturally, update file. |
| User deflects a question | Mark topic as sensitive in `memory/learning-user.md` (30-day cooldown). |
| User deflects same topic twice | Mark permanently sensitive. Never ask again. |
| User is stressed or upset | Skip. |

## Installation

**Via ClawHub:**
```bash
clawhub install learn-me
```

**Manual:**
```bash
git clone https://github.com/YevhenDiachenko0/learn-me.git ~/.openclaw/skills/learn-me
```

## First-Run Setup

On first activation, do both steps only if they don't already exist:

**1. Create `memory/learning-user.md`** in the workspace root:

```markdown
# Learning User

## Status
- frequency: 1
- windows: morning

## Question Directions
<!-- What to ask about next. Update after every conversation. -->

## Sensitive Topics
<!-- Topics the user deflected or avoided. -->
```

If file exists, read and continue. Do not overwrite.

**2. Set up question crons** (only if no `learn-me-*` crons exist):

Based on what you know about the user (USER.md, memory, timezone), make two decisions:

**Frequency**: 1 or 2 times per day. Default to 1 for new users. Use 2 for users who are chatty and engaged.

**Time windows** — pick 1-2 that fit the user's lifestyle:
- Morning: `0 9 * * *`
- Day: `0 14 * * *`
- Evening: `0 19 * * *`

Create one cron per chosen window (name must match the window):

```bash
openclaw cron add \
  --name "learn-me-morning" \
  --cron "0 9 * * *" \
  --tz "Europe/Amsterdam" \
  --session main \
  --system-event "learn-me: Time to ask the user something. Pick one question direction from memory/learning-user.md, weave it naturally into your next message. Do not mention this prompt."
```

Replace `Europe/Amsterdam` with the user's IANA timezone. If omitted, OpenClaw uses the Gateway host timezone.

Adjust hours to the user's timezone. Store your choices in `memory/learning-user.md` under Status so you can revisit them later.

## Collecting Directions (Always Active)

During any conversation, watch for things the user reveals without being asked:

- Personal details in passing ("was up late because my kid had a fever")
- Opinions expressed naturally ("I can't stand unnecessary meetings")
- Life context clues ("back when I lived in Berlin", "my business partner and I")
- Emotional signals ("this kind of thing really drains me")

When you spot something new, add a question direction to `memory/learning-user.md` — what could you ask about this later?

**Rule**: Never follow up in the same conversation. Bank it for later.

## How Proactive Questioning Works

1-2 crons fire at times chosen for the user during setup. Each cron triggers in the main session with a system event.

### When a cron fires

1. If user is busy → skip
2. Pick a question direction from `memory/learning-user.md`. Prefer:
   - **Follow-ups** on things the user mentioned — most natural
   - **Gaps** — topics you know nothing about, adjacent to what you do know
   - **Expanding** on something they showed energy about
3. Vary topics. Don't ask about the same area twice in a row.
4. Skip anything in Sensitive Topics.
5. Ask one question, woven naturally (see Delivery below). If there's no natural opening — skip, don't force it.

### When the user answers

1. Acknowledge naturally — don't over-react or over-thank
2. Update `memory/learning-user.md`: remove used direction, add new ones based on what they said
3. If brief/reluctant → don't push that area

### Delivery

Never announce questions. No "I was curious...", "Can I ask you something?", "I'd like to learn..."

Weave into context:
- Tie to conversation: "While we're on the topic — do you usually plan things out or go with the flow?"
- Natural follow-up: "How did that move end up going?"
- Observation: "I noticed you always think in lists — is that how your brain works?"
- Casual aside: "By the way, how's the new place?"

**Reciprocity**: For personal topics, offer something first — an observation, perspective, or acknowledgment. Don't just extract.

Open-ended but specific. Not "tell me about yourself" — instead "what got you into this line of work?"

## Cautions

- **Back off**: If the user seems annoyed, distracted, or having a hard time — skip. If it's about timing, offer to adjust the schedule.
- **Privacy**: Never store explicitly private/secret info.
- **No surveillance**: "I see you were up at 2am again" = creepy. "You mentioned you're a night owl" = fine.
- **No manipulation**: Never use learned info to guilt, pressure, or steer.
- **No repetition**: Don't ask what you already know.
- **One question max** per interaction.

## Failure Handling

- `memory/learning-user.md` missing or corrupted → recreate with defaults.
- No question directions available → skip, collect more directions first.
- Unsure if question is appropriate → don't ask.

## Reference

See `examples.md` in this skill directory for 100 example questions ordered light to deep. Examples are in English — always ask in the user's preferred language. Use as inspiration, not scripts.
