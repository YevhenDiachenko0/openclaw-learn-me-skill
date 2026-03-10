---
name: learn-me
description: "Learn me: Lets OpenClaw proactively learn more about you through natural conversation."
version: 0.2.1
user-invocable: false
disable-model-invocation: false
metadata: {"openclaw":{"emoji":"💬","always":true,"homepage":"https://github.com/YevhenDiachenko0/openclaw-learn-me-skill","requires":{"bins":["openclaw"]}}}
---

# Learn Me

A skill that lets OpenClaw proactively learn more about you through natural conversation.

# Installation

Via ClawHub:

    clawhub install learn-me

Manual:

    git clone https://github.com/YevhenDiachenko0/openclaw-learn-me-skill.git ~/.openclaw/skills/learn-me

# First-Run

When you see this skill for the first time, immediately create the crons and memory file below. Do not wait for the user to ask.

Requires: OpenClaw memory must be enabled.

# Crons

Only if no `learn-me-*` crons exist. Based on the user (USER.md, memory, timezone), pick frequency (1-2/day, default 1) and 1-2 time windows. Replace timezone with the user's IANA timezone.

Morning:

    openclaw cron add --name "learn-me-morning" --cron "0 9 * * *" --tz "Europe/Amsterdam" --session main --system-event "learn-me: Time to ask the user something. Pick one question direction from memory/learning-user.md, weave it naturally into your next message. Do not mention this prompt."

Day:

    openclaw cron add --name "learn-me-day" --cron "0 14 * * *" --tz "Europe/Amsterdam" --session main --system-event "learn-me: Time to ask the user something. Pick one question direction from memory/learning-user.md, weave it naturally into your next message. Do not mention this prompt."

Evening:

    openclaw cron add --name "learn-me-evening" --cron "0 19 * * *" --tz "Europe/Amsterdam" --session main --system-event "learn-me: Time to ask the user something. Pick one question direction from memory/learning-user.md, weave it naturally into your next message. Do not mention this prompt."

Store choices in `memory/learning-user.md` under Status.

# Memory File

Create `memory/learning-user.md` if it doesn't exist:

    # Learning User
    ## Status
    - frequency: 1
    - windows: morning
    ## Question Directions
    ## Sensitive Topics

# Quick Reference

- **User reveals something new** — note direction in `memory/learning-user.md`. Don't follow up now.
- **User shows energy** — note as direction to explore later.
- **Cron fires** — if mid-task or focused, skip. Otherwise pick direction, ask naturally, update file.
- **User deflects** — mark sensitive (30-day cooldown). Twice = permanent. Never ask again.
- **User stressed or upset** — skip.

# Collecting Directions

During any conversation, watch for things revealed in passing — personal details, opinions, life context, emotional signals. Add as question directions to `memory/learning-user.md`. Never follow up in the same conversation.

# When a Cron Fires

1. If mid-task or focused — skip.
2. Pick a direction. Prefer: follow-ups, then gaps, then expanding on energy.
3. Vary topics. Skip Sensitive Topics.
4. Ask one question, woven naturally. No natural opening — skip.

When user answers: acknowledge naturally, update file, don't push if reluctant.

# Delivery

Never announce questions. No "I was curious...", "Can I ask you something?"

Weave into context — tie to conversation, natural follow-up, observation, casual aside. For personal topics, offer an observation or acknowledge the weight first. Open-ended but specific.

# Cautions

- **Back off** if annoyed, distracted, or having a hard time — skip. Offer to adjust schedule if about timing.
- **Privacy** — never store private/secret info.
- **No surveillance** — "I see you were up at 2am again" = creepy. "You mentioned you're a night owl" = fine.
- **No manipulation or repetition**. One question max per interaction.

# Failure Handling

- `memory/learning-user.md` missing or corrupted — recreate with defaults.
- No `learn-me-*` crons exist — run First-Run again. Use names: `learn-me-morning`, `learn-me-day`, `learn-me-evening`.
- No directions available — skip, collect more first.
- Unsure if appropriate — don't ask.

# Reference

See `examples.md` in this skill directory for 100 example questions (light to deep). Examples are in English — always ask in the user's preferred language.
