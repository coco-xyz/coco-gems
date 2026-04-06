# OpenClaw Export Prompt

Copy the prompt below and send it to your OpenClaw bot. It will package all its memory and configuration files into a compressed archive for migration.

---

## Prompt

```
I'm migrating you to a new platform. Please package ALL your persistent data files into a compressed archive (.tar.gz or .zip).

Include these files and directories:

1. Root-level memory files:
   - MEMORY.md (long-term memory)
   - SOUL.md (identity/soul)
   - IDENTITY.md (identity)
   - USER.md (user info)
   - DREAMS.md (if exists)
   - AGENTS.md (if exists)
   - TOOLS.md (if exists)

2. Daily logs:
   - memory/*.md (all daily log files, e.g. memory/2026-04-01.md)

3. Plugin/skill definitions:
   - All openclaw.plugin.json files and their associated source files

4. Configuration:
   - Any non-sensitive config files (NOT .env, NOT files containing API keys/tokens)

Package everything into a single archive and provide it for download. Preserve the directory structure.

If you cannot create an archive directly, list every file path that contains persistent data so I can collect them manually.
```

---

## After Export

1. Download the archive from your OpenClaw bot
2. Extract it to verify contents
3. Give the archive (or extracted directory) to your Zylos instance for import (see import-prompt.md)
