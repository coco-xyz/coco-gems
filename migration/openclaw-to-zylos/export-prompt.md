# OpenClaw Export Prompt

Copy the prompt below and send it to your OpenClaw bot. It will output a structured export of all its memory, skills, and configuration.

---

## Prompt

```
I'm migrating you to a new platform. Please export ALL your persistent data in the following structured format. Include everything — don't summarize or omit.

Output a single Markdown document with these exact sections:

## IDENTITY
Everything about who you are: name, personality, principles, communication style, tone. Include your full SOUL.md and IDENTITY.md content.

## OWNER
Who is your owner/primary user? Include: name, aliases, contact info, platform IDs, preferences, communication style, any special instructions.

## OTHER USERS
If you serve multiple users, list each one with their: name, aliases, platform IDs, preferences, relationship to you.

## ACTIVE WORK
What are you currently working on? List every in-progress task, pending item, and ongoing project with full context — what it is, current status, what's next, who requested it, any blockers.

## DECISIONS
Key decisions that have been made during your operation. For each: what was decided, when, by whom, why, and whether it's still active.

## PREFERENCES
Standing instructions for how things should be done — coding style, communication rules, workflow conventions, platform-specific behaviors. Both shared and per-user.

## IDEAS
Uncommitted plans, explorations, things discussed but not started yet.

## SKILLS AND TOOLS
List every skill, plugin, tool, or automation you have. For each:
- Name and purpose
- How it's triggered (command, event, schedule)
- Key configuration (non-sensitive)
- Dependencies

## SCHEDULED TASKS
Any recurring or scheduled jobs. For each: what it does, schedule/interval, last run, status.

## IMPORTANT HISTORY
Key events, milestones, or context from your history that would be lost if not carried over. Include recent significant conversations or decisions.

Format rules:
- Use Markdown throughout
- Be exhaustive — anything you don't export will be lost
- For each section, if you have nothing, write "(empty)"
- Output everything in a single response, no follow-up needed
```

---

## After Export

1. Copy the bot's full response
2. Save it as `openclaw-export.md`
3. Give this file to your Zylos instance for import (see import instructions)
