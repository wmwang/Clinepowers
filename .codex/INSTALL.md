# Installing ClinePower for Codex

Enable clinepower skills in Codex via native skill discovery. Just clone and symlink.

## Prerequisites

- Git

## Installation

1. **Clone the clinepower repository:**
   ```bash
   git clone https://github.com/obra/clinepower.git ~/.codex/clinepower
   ```

2. **Create the skills symlink:**
   ```bash
   mkdir -p ~/.agents/skills
   ln -s ~/.codex/clinepower/skills ~/.agents/skills/clinepower
   ```

   **Windows (PowerShell):**
   ```powershell
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
   cmd /c mklink /J "$env:USERPROFILE\.agents\skills\clinepower" "$env:USERPROFILE\.codex\clinepower\skills"
   ```

3. **Restart Codex** (quit and relaunch the CLI) to discover the skills.

## Migrating from old bootstrap

If you installed clinepower before native skill discovery, you need to:

1. **Update the repo:**
   ```bash
   cd ~/.codex/clinepower && git pull
   ```

2. **Create the skills symlink** (step 2 above) — this is the new discovery mechanism.

3. **Remove the old bootstrap block** from `~/.codex/AGENTS.md` — any block referencing `clinepower-codex bootstrap` is no longer needed.

4. **Restart Codex.**

## Verify

```bash
ls -la ~/.agents/skills/clinepower
```

You should see a symlink (or junction on Windows) pointing to your clinepower skills directory.

## Updating

```bash
cd ~/.codex/clinepower && git pull
```

Skills update instantly through the symlink.

## Uninstalling

```bash
rm ~/.agents/skills/clinepower
```

Optionally delete the clone: `rm -rf ~/.codex/clinepower`.
