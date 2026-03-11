# learn-me

An OpenClaw skill that gets to know you better through natural conversation.

When active, the agent will occasionally weave in a question — about your work, interests, habits, or whatever feels relevant. It remembers what you share and builds on it over time. The goal is understanding, not data collection.

## Install

Via ClawHub:

```
clawhub install learn-me
```

Manual:

```
git clone https://github.com/YevhenDiachenko0/openclaw-learn-me-skill.git ~/.openclaw/skills/learn-me
```

## How it works

On first use, the agent introduces itself and asks if you'd like to set up a daily schedule. If you say yes, it creates cron jobs that prompt a question at times you choose. You can skip, reschedule, or disable anytime.

You can also trigger it manually:

```
/learn-me
```

## What it remembers

Facts and directions are stored in `memory/next-questions.md`. No secrets or sensitive data are ever stored. If you deflect a question twice, it's permanently dropped.

## Requirements

- OpenClaw with memory enabled
- `openclaw` on PATH
