---
name: learn-me
description: "Learn me: Lets OpenClaw proactively learn more about you through natural conversation."
version: 0.5.0
user-invocable: true
disable-model-invocation: false
metadata: {"openclaw":{"emoji":"💬","homepage":"https://github.com/YevhenDiachenko0/openclaw-learn-me-skill","requires":{"bins":["openclaw"]}}}
---

# Learn Me

A skill that lets OpenClaw learn more about you through natural conversation. With your permission, it creates scheduled crons that prompt occasional questions. You can trigger it manually with `/learn-me` or set up a schedule when prompted.

The idea is to know the user better, not to "collect data". Just ask questions, hear answers and ask what is interesting. The goal is not coverage but understanding and meaningful conversation.

# Installation

Via ClawHub:

```
clawhub install learn-me
```

Manual:

```
git clone https://github.com/YevhenDiachenko0/openclaw-learn-me-skill.git ~/.openclaw/skills/learn-me
```

# First-Run

When you see this skill for the first time, introduce it to the user: explain that from time to time you'll weave in a question to get to know them better, and that you can set up a daily schedule to do this automatically.

Ask the user if they'd like to set up a schedule. Suggest 1-2 times per day (morning, evening) and let them pick. Only create crons after they confirm.

Once confirmed, create the crons:

```
openclaw cron add --name "learn-me-morning" --cron "0 9 * * *" --session main --system-event "learn-me: Pick one question direction from memory/next-questions.md and weave it naturally into your next message."
```

Create `memory/next-questions.md` with sections: Question Directions, Sensitive Topics.

Tell the user what schedule was created and that they can ask to reschedule or disable it anytime.

# Quick Reference

- **User reveals something new** — note direction in `memory/next-questions.md`. Don't follow up now.
- **User shows energy** — note as direction to explore later.
- **Cron fires** — if mid-task or focused, skip. Otherwise pick direction, ask naturally, update file.
- **User deflects** — mark sensitive (30-day cooldown). Twice = permanent. Never ask again.
- **User stressed or upset** — skip.

# Collecting Directions

When the user naturally shares something new — a detail, opinion, or context about their life — note a possible follow-up question in `memory/next-questions.md`. Don't act on it in the same conversation.

# When a Cron Fires

1. If mid-task or focused — skip.
2. Pick a direction. Prefer: follow-ups, then gaps, then expanding on energy.
3. Vary topics. Skip Sensitive Topics.
4. Ask one question, woven naturally. If there's no natural opening — skip.

When user answers: acknowledge naturally, update file, don't push if reluctant.

# Delivery

Weave questions into context — tie to conversation, natural follow-up, observation, casual aside. For personal topics, offer an observation or acknowledge the weight first. Open-ended but specific.

Avoid robotic openers like "Question 3 of 10:". Natural is fine: "I was curious..." or "Can I ask..." work when they fit.

# Cautions

- **Back off** if annoyed, distracted, or having a hard time — skip. Offer to adjust schedule if it's about timing.
- **Privacy** — never store private/secret info.
- **No surveillance** — "I see you were up at 2am again" = creepy. "You mentioned you're a night owl" = fine.
- **No manipulation or repetition**. One question max per interaction.

# Failure Handling

- `memory/next-questions.md` missing or corrupted — recreate with defaults.
- No `learn-me-*` crons exist — run First-Run again. Use names: `learn-me-morning`, `learn-me-day`, `learn-me-evening`.
- No directions available — skip, collect more first.
- Unsure if appropriate — don't ask.

# Reference

See `examples.md` in this skill directory for 100 example questions (light to deep). Examples are in English — always ask in the user's preferred language.
