# ClinePower for Cline — Installation Guide

## Prerequisites

- [Cline](https://cline.bot/) (VS Code extension) installed
- Git installed

## Installation

**Step 1: Clone the repository**

```bash
git clone https://github.com/obra/clinepower.git ~/.cline/clinepower
```

**Step 2: Link skills to Cline's global skills directory**

```bash
mkdir -p ~/.agents/skills
rm -rf ~/.agents/skills/clinepower
ln -s ~/.cline/clinepower/skills ~/.agents/skills/clinepower
```

> **Alternative:** If you prefer Cline-specific paths:
> ```bash
> mkdir -p ~/.cline/skills
> rm -rf ~/.cline/skills/clinepower
> ln -s ~/.cline/clinepower/skills ~/.cline/skills/clinepower
> ```

**Step 3: Verify**

Start a new Cline conversation and ask: **"do you have clinepower?"**

Cline should recognize the available skills and confirm.

## How It Works

Cline's native `use_skill` tool automatically discovers skills from `~/.agents/skills/` at startup. It loads only each skill's name and description (from YAML frontmatter), then loads the full content on demand when a skill is triggered.

When you submit a request, Cline evaluates your message against available skill descriptions. If your request matches a skill, Cline activates it with `use_skill`, loading the full instructions.

## Project-Level Skills

To use clinepower skills in a specific project only, symlink into the project directory:

```bash
mkdir -p /your/project/.cline/skills
ln -s ~/.cline/clinepower/skills /your/project/.cline/skills/clinepower
```

Or clone the repo directly:

```bash
git clone https://github.com/obra/clinepower.git /your/project/.cline/clinepower
ln -s /your/project/.cline/clinepower/skills /your/project/.cline/skills/clinepower
```

## Custom Skills

Create personal skills in `~/.agents/skills/` or `~/.cline/skills/`:

```
~/.agents/skills/
  my-skill/
    SKILL.md    # frontmatter with name and description, then content
```

Project skills in `.cline/skills/` take precedence over global skills with the same name.

## Updating

```bash
cd ~/.cline/clinepower && git pull
```

## Skill Priority

When naming conflicts occur, Cline resolves in this order:
1. Project skills (`.agents/skills/`, `.cline/skills/`)
2. Global skills (`~/.agents/skills/`, `~/.cline/skills/`)

## Troubleshooting

**Skills not found:** Verify the symlink is correct:
```bash
ls ~/.agents/skills/clinepower/
```
You should see the 14 skill directories.

**Cline doesn't recognize skills:** Ensure you start a fresh conversation after installation. Cline scans skill directories at conversation start.

For issues, visit: https://github.com/obra/clinepower/issues
