# Parallels Desktop skills

![License](https://img.shields.io/badge/License-MIT-green)

An agent skill for automating [Parallels Desktop](https://www.parallels.com/products/desktop/)
for Mac and its Linux or Windows virtual machines through the official `prlctl` and `prlsrvctl`
command-line tools. Your agent discovers VMs instead of guessing names, reads local `--help`
before changing anything, and verifies every change with a fresh read.

Works with Claude Code, Codex, Cursor, Gemini CLI, Antigravity, GitHub Copilot, OpenCode and
any other agent that reads [Agent Skills](https://agentskills.io).

## Skills

| Skill | What it does |
|-------|--------------|
| `parallels-cli` | Inventory, lifecycle (start, stop, suspend, clone, move, archive, unregister), CPU, memory, disks, network adapters, USB and shared folders, guest commands via Parallels Tools, screenshots and key events, snapshots and backups, host networks and preferences, sharp GNOME Retina/HiDPI scaling. Includes `parallels-doctor`, a read-only host and VM inventory. |

## Install

### Any agent: the `skills` CLI

```bash
npx skills add kastruppro/parallels-skills
```

Non-interactive examples:

```bash
npx skills add kastruppro/parallels-skills -a claude-code -a codex -y   # this project
npx skills add kastruppro/parallels-skills -g -y                        # globally, all detected agents
```

### Claude Code: the plugin

```text
/plugin marketplace add kastruppro/parallels-skills
/plugin install parallels@parallels-skills
```

Use either the plugin or the `skills` CLI for Claude Code, not both, or the skill appears twice.

### Codex

`.codex-plugin/plugin.json` lets Codex load this repo as a plugin.

### Manually

Copy `skills/parallels-cli` into your agent's skills directory, for example `~/.claude/skills/`
(Claude Code) or `~/.agents/skills/` (Codex, Cursor, Gemini CLI, Antigravity, OpenCode, Copilot).

## Use

- "List my Parallels VMs and summarize their state and Parallels Tools status."
- "Start my Linux dev VM, verify it booted and show its IP addresses."
- "Run a read-only PowerShell inventory in my Windows VM."
- "Snapshot the VM before changing its CPU and memory."
- "Make GNOME text sharp at Retina density without fractional scaling."

## Safety

The skill separates read-only inspection, reversible operations and destructive work. Deleting
or unregistering VMs, removing snapshots or disks, forced stop/reset, moving, encrypting or
archiving storage, and network or licensing changes require an exact target and, where data can
be lost, your confirmation. Passwords and keys are never put on the command line or in logs.

## Requirements

- macOS with Parallels Desktop; `prlctl` and `prlsrvctl` in `PATH`.
- Parallels Tools in guests for guest command execution.

## Credits

Based on [ParallelsSkill](https://github.com/wieslawsoltes/ParallelsSkill) by Wiesław Šoltés
(MIT). Parallels and Parallels Desktop are trademarks of Parallels International GmbH. This is
an independent project, not affiliated with or endorsed by Parallels.

## License

MIT - see [LICENSE](LICENSE).
