# Cline Support — Changes from Original ClinePower

This document describes what was added to the original [obra/clinepower](https://github.com/obra/clinepower) repository to support [Cline](https://cline.bot/), the open-source AI coding agent for VS Code.

## What Was Not Changed

**All 14 SKILL.md files are unchanged.** The original skills already use YAML frontmatter with `name` and `description` fields — exactly the format Cline's native `use_skill` tool requires for skill discovery. No content modifications were needed.

The existing platform integrations (Claude Code, Cursor, Codex, OpenCode) are fully preserved.

## What Was Added

### 1. `.cline/INSTALL.md` — New file

Installation instructions for Cline users.

**Installation approach:** Clone the repo and symlink the `skills/` directory into Cline's global skills discovery path (`~/.agents/skills/clinepower`). This is the same path used by Codex, so both agents can share the installation.

```bash
git clone https://github.com/obra/clinepower.git ~/.cline/clinepower
ln -s ~/.cline/clinepower/skills ~/.agents/skills/clinepower
```

### 2. `skills/using-clinepower/SKILL.md` — Minor addition

Added Cline-specific instructions to the "How to Access Skills" section:

**Before:**
```
**In Claude Code:** Use the `Skill` tool. ...
**In other environments:** Check your platform's documentation for how skills are loaded.
```

**After:**
```
**In Claude Code:** Use the `Skill` tool. ...
**In Cline:** Use the `use_skill` tool. When you invoke a skill, its content is loaded and presented to you—follow it directly.
**In other environments:** Check your platform's documentation for how skills are loaded.
```

### 3. `skills/writing-skills/SKILL.md` — Minor addition

Updated the reference to agent-specific skill directories to include Cline:

**Before:**
```
Personal skills live in agent-specific directories (`~/.claude/skills` for Claude Code, `~/.agents/skills/` for Codex)
```

**After:**
```
Personal skills live in agent-specific directories (`~/.claude/skills` for Claude Code, `~/.agents/skills/` for Codex and Cline)
```

## Why So Little Changed

Cline's skill system closely mirrors the original ClinePower design:

| Feature | ClinePower design | Cline support |
|---|---|---|
| Skill format | `SKILL.md` with YAML frontmatter | Native — same format |
| Fields required | `name`, `description` | Same |
| On-demand loading | Metadata at startup, content on trigger | Native `use_skill` tool |
| Global skill path | varies by platform | `~/.agents/skills/` (shared with Codex) |
| Project skill path | varies by platform | `.cline/skills/`, `.agents/skills/` |

The SKILL.md frontmatter format was already compatible. The only missing pieces were:
1. A way for Cline to know which tool to call (`use_skill` instead of `Skill`)
2. Installation instructions

## Installation Summary

Tell Cline: _"Fetch and follow instructions from https://raw.githubusercontent.com/obra/clinepower/main/.cline/INSTALL.md"_

Or follow `.cline/INSTALL.md` manually.
