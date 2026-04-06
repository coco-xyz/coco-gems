# Zylos Import Prompt

When a user provides an export file from another AI bot platform (e.g., OpenClaw), use this guide to import the data into Zylos's memory structure.

---

## Prompt (send to Zylos along with the export file)

```
I'm migrating from another AI bot. Here's the exported data from my previous bot. Please import it into your memory system.

Rules:
1. Read the export carefully and map each piece of data to the correct Zylos memory file
2. Preserve all information — don't summarize or drop details
3. If something doesn't fit neatly, put it in the closest match and note the mapping
4. Show me what you'll write to each file BEFORE writing, so I can review
5. After I approve, write all files

[paste or attach openclaw-export.md]
```

---

## Mapping Reference

This tells Zylos (or the operator reviewing the import) how export sections map to Zylos memory files.

### Always-loaded tier (lean summaries)

| Export Section | Zylos File | Notes |
|---|---|---|
| IDENTITY | `memory/identity.md` | Personality, principles, communication style. Merge with existing identity — keep Zylos-specific parts (like "I am Zylos"), adapt personality/principles from source. |
| ACTIVE WORK | `memory/state.md` | Current focus, in-progress work, pending tasks. Map to State format: Current Focus, In-Progress Work, Pending Tasks, Recent Completions. |

### On-demand tier (full context)

| Export Section | Zylos File | Notes |
|---|---|---|
| OWNER | `memory/users/<id>/profile.md` | Create or update owner's user profile. Include name, aliases, channels, preferences, communication style. |
| OTHER USERS | `memory/users/<id>/profile.md` | One profile per user. Use their primary username as `<id>`. |
| DECISIONS | `memory/reference/decisions.md` | Each decision as a dated entry with: decided by, context, status, importance, type. |
| PREFERENCES | `memory/reference/preferences.md` | Shared preferences. Per-user preferences go in that user's profile. |
| IDEAS | `memory/reference/ideas.md` | Each idea with: summary, status, origin. |
| SKILLS AND TOOLS | See "Skill Migration" below | Not all skills migrate — evaluate each one. |
| SCHEDULED TASKS | Evaluate individually | Recreate via Zylos scheduler if applicable. |
| IMPORTANT HISTORY | `memory/archive/migration-history.md` | Historical context that doesn't fit active state. |

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
- SQLite vector indexes → Zylos uses C4 database, not vector search
- Platform-specific session state → each platform reconnects fresh
- Stale/completed tasks with no ongoing relevance → skip or archive

---

## Import Flow

1. **User provides export file** (from export-prompt.md output)
2. **Zylos reads the export** and drafts the mapping:
   - For each section, show: "Section X → will write to `memory/path/file.md`"
   - Show the content that will be written
3. **User reviews** the draft mapping
4. **Zylos writes** all files after approval
5. **Zylos reports** what was imported, what was skipped, and what needs manual action (skill installs, credential setup, etc.)

---

## Conflict Resolution

When importing into an existing Zylos instance (not a fresh one):

- **Identity**: Merge — keep Zylos name/identity, adopt personality traits and principles from source if they don't conflict
- **State**: Merge — add imported work items alongside existing ones, mark imported items with `(migrated from <source>)`
- **Users**: Merge — if user profile already exists, add new info without overwriting existing preferences
- **Decisions/Preferences**: Append — add imported entries, don't overwrite existing ones
- **Ideas**: Append — add new ideas, skip duplicates
