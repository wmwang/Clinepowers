# ClinePower for Cline

Guide for using ClinePower with [Cline](https://cline.bot/) (VS Code extension) via native skill discovery.

## Quick Install

Tell Cline:

```
Fetch and follow instructions from https://raw.githubusercontent.com/obra/clinepower/refs/heads/main/.cline/INSTALL.md
```

## Manual Installation

### Prerequisites

- [Cline](https://marketplace.visualstudio.com/items?itemName=saoudrizwan.claude-dev) VS Code extension installed
- Git

### Steps

1. Clone the repo:
   ```bash
   git clone https://github.com/obra/clinepower.git ~/.cline/clinepower
   ```

2. Create the skills symlink:
   ```bash
   mkdir -p ~/.agents/skills
   ln -s ~/.cline/clinepower/skills ~/.agents/skills/clinepower
   ```

3. Start a new Cline conversation — skills are discovered at conversation start.

### Alternative: Cline-specific path

If you prefer to keep skills under Cline's own directory:

```bash
mkdir -p ~/.cline/skills
ln -s ~/.cline/clinepower/skills ~/.cline/skills/clinepower
```

### Windows

Use a junction instead of a symlink (works without Developer Mode):

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\clinepower" "$env:USERPROFILE\.cline\clinepower\skills"
```

## How It Works

Cline has native skill discovery — it scans `~/.agents/skills/` (and `~/.cline/skills/`) at conversation start, parses SKILL.md frontmatter, and loads skills on demand using the `use_skill` tool.

ClinePower skills are made visible through a single symlink:

```
~/.agents/skills/clinepower/ → ~/.cline/clinepower/skills/
```

**Progressive loading:** Cline reads only each skill's `name` and `description` at startup (~100 tokens/skill). Full skill content is loaded only when a skill is triggered, keeping context lean.

The `using-clinepower` skill is discovered automatically and enforces skill usage discipline — no additional configuration needed.

## Skill Priority

When the same skill name exists in multiple locations, Cline resolves in this order:

1. Project skills (`.agents/skills/`, `.cline/skills/` in project root)
2. Global skills (`~/.agents/skills/`, `~/.cline/skills/`)

Project-level skills always override global ones with the same name.

## Usage

Skills are discovered automatically. Cline activates them when:
- The task matches a skill's description
- You mention a skill by name (e.g., "use brainstorming")
- The `using-clinepower` skill directs Cline to use one

### Personal Skills

Create your own global skills in `~/.agents/skills/`:

```bash
mkdir -p ~/.agents/skills/my-skill
```

Create `~/.agents/skills/my-skill/SKILL.md`:

```markdown
---
name: my-skill
description: Use when [condition]
---

# My Skill

[Your skill content here]
```

The `description` field is how Cline decides when to activate a skill — write it as a clear trigger condition starting with "Use when...".

### Project-Level Skills

For skills specific to one project, place them in the project root:

```
your-project/
└── .cline/
    └── skills/
        └── my-project-skill/
            └── SKILL.md
```

Or use the shared path:

```
your-project/
└── .agents/
    └── skills/
        └── my-project-skill/
            └── SKILL.md
```

## Updating

```bash
cd ~/.cline/clinepower && git pull
```

Skills update instantly through the symlink — no restart needed for content changes. For new skills added to the repo, start a fresh conversation.

## Uninstalling

```bash
rm ~/.agents/skills/clinepower
```

Optionally delete the clone:
```bash
rm -rf ~/.cline/clinepower
```

**Windows (PowerShell):**
```powershell
Remove-Item "$env:USERPROFILE\.agents\skills\clinepower"
Remove-Item -Recurse -Force "$env:USERPROFILE\.cline\clinepower"
```

## Troubleshooting

### Skills not showing up

1. Verify the symlink: `ls -la ~/.agents/skills/clinepower`
2. Check skills exist: `ls ~/.cline/clinepower/skills`
3. Start a **new** Cline conversation — skills are scanned at conversation start, not mid-conversation
4. Ask Cline: "do you have clinepower?" to confirm discovery

### Skill triggered but wrong content loaded

Each skill directory must contain a `SKILL.md` with valid YAML frontmatter:
```yaml
---
name: skill-name
description: Use when ...
---
```
If frontmatter is missing or malformed, the skill won't be discoverable.

### Windows junction issues

Junctions normally work without special permissions. If creation fails, try running PowerShell as administrator.

## Getting Help

- Report issues: https://github.com/obra/clinepower/issues
- Main documentation: https://github.com/obra/clinepower
- Cline documentation: https://docs.cline.bot
