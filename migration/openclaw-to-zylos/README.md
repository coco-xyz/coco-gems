# OpenClaw to Zylos Migration

Migrate your bot's memory, skills, and configuration from OpenClaw to Zylos using LLM-driven conversion — no scripts or parsers needed.

## How It Works

Both ends leverage the LLM's own understanding:
1. **OpenClaw exports** — you give it a prompt, it dumps everything it knows in a structured format
2. **Zylos imports** — you give it the export + import guide, it maps data into its memory structure

## Steps

### 1. Export from OpenClaw

Open `export-prompt.md`, copy the prompt, and send it to your OpenClaw bot.

Save the bot's response as `openclaw-export.md`.

### 2. Import into Zylos

Send the export file to your Zylos instance along with the import instructions from `import-prompt.md`.

Zylos will:
- Draft a mapping (which data goes where)
- Show you for review
- Write to memory files after your approval
- Report what needs manual setup (credentials, skill installs, etc.)

## Files

| File | Purpose |
|------|---------|
| `export-prompt.md` | Prompt to send to OpenClaw for data export |
| `import-prompt.md` | Import guide for Zylos (mapping rules, conflict resolution) |
