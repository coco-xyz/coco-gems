# Zylos Import Prompt

When a user provides an export archive from another AI bot platform (e.g., OpenClaw), use this guide to import the data into Zylos's memory structure.

---

## Prompt (send to Zylos along with the export archive)

```
I'm migrating from another AI bot. Here's the exported archive from my previous bot (OpenClaw). Please import it into your memory system.

The archive contains the bot's original files: MEMORY.md, SOUL.md, IDENTITY.md, USER.md, DREAMS.md, daily logs (memory/*.md), plugin definitions, etc.

Rules:
1. Extract and read all files in the archive
2. Understand what each file contains by reading it — don't rely on filenames alone
3. Map each piece of data to the correct Zylos memory file
4. Preserve all information — don't summarize or drop details
5. Show me what you'll write to each file BEFORE writing, so I can review
6. After I approve, write all files

[provide the archive path or extracted directory]
```

---

## Mapping Reference

This tells Zylos (or the operator reviewing the import) how OpenClaw source files map to Zylos memory files.

### Source → Target File Mapping

| OpenClaw Source | Zylos Target | Notes |
|---|---|---|
| `SOUL.md` + `IDENTITY.md` | `memory/identity.md` | Personality, principles, communication style. Merge with existing identity — keep Zylos-specific parts (like "I am Zylos"), adapt personality/principles from source. |
| `MEMORY.md` (active work sections) | `memory/state.md` | Extract current focus, in-progress work, pending tasks. Map to State format: Current Focus, In-Progress Work, Pending Tasks, Recent Completions. |
| `MEMORY.md` (decisions) | `memory/reference/decisions.md` | Each decision as a dated entry with: decided by, context, status, importance, type. |
| `MEMORY.md` (preferences) | `memory/reference/preferences.md` | Shared preferences. Per-user preferences go in that user's profile. |
| `MEMORY.md` (ideas/plans) | `memory/reference/ideas.md` | Each idea with: summary, status, origin. |
| `USER.md` | `memory/users/<id>/profile.md` | Create or update owner's user profile. Include name, aliases, channels, preferences, communication style. |
| `memory/*.md` (daily logs) | `memory/archive/` | Historical daily logs. Extract any still-active items into `state.md`. |
| `DREAMS.md` | `memory/reference/ideas.md` | Valuable insights get merged into ideas; rest can be discarded. |
| `AGENTS.md` + `TOOLS.md` | Evaluate individually | See "Skill Migration" below. |
| Plugin definitions (`openclaw.plugin.json` + source) | Evaluate individually | See "Skill Migration" below. |

### Key: MEMORY.md Classification

OpenClaw's `MEMORY.md` is a single file containing mixed content. The LLM must read and classify each section:

| Content Type | Zylos Target |
|---|---|
| Who the bot is, personality | `memory/identity.md` |
| Current tasks, active work | `memory/state.md` |
| Past decisions, choices made | `memory/reference/decisions.md` |
| Standing rules, conventions | `memory/reference/preferences.md` |
| Uncommitted ideas, explorations | `memory/reference/ideas.md` |
| User info, preferences | `memory/users/<id>/profile.md` |
| Historical events, completed work | `memory/archive/migration-history.md` |

### Skill Migration

Skills don't auto-migrate — they need evaluation:

| Source Skill Type | Zylos Action |
|---|---|
| Communication channel (Telegram, Lark, etc.) | Check if Zylos already has this channel. If yes, just migrate config. If no, note as "needs component install". |
| Automation / scheduled task | Recreate via Zylos scheduler skill (`cli.js add`). |
| Custom tool / plugin | Evaluate if Zylos has an equivalent. If not, flag for manual skill creation. |
| API integration | Note the integration details in `reference/projects.md` for later setup. |

### What NOT to import

- API keys, tokens, passwords → user must re-configure in `.env`
- SQLite vector indexes (`memory/*.sqlite`) → Zylos uses C4 database, not vector search
- Platform-specific session state → each platform reconnects fresh
- Stale/completed tasks with no ongoing relevance → skip or archive

---

## Import Flow

1. **User provides export archive** (from export-prompt.md output)
2. **Zylos extracts and reads all files**, classifies content
3. **Zylos drafts the mapping:**
   - For each source file, show: "`SOURCE_FILE` → will write to `memory/path/file.md`"
   - Show the content that will be written to each target file
4. **User reviews** the draft mapping
5. **Zylos writes** all files after approval
6. **Zylos reports** what was imported, what was skipped, and what needs manual action (skill installs, credential setup, etc.)

---

## Conflict Resolution

When importing into an existing Zylos instance (not a fresh one):

- **Identity**: Merge — keep Zylos name/identity, adopt personality traits and principles from source if they don't conflict
- **State**: Merge — add imported work items alongside existing ones, mark imported items with `(migrated from <source>)`
- **Users**: Merge — if user profile already exists, add new info without overwriting existing preferences
- **Decisions/Preferences**: Append — add imported entries, don't overwrite existing ones
- **Ideas**: Append — add new ideas, skip duplicates
