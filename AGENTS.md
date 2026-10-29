# AGENTS.md - parallels-skills

An Agent Skill that lets AI agents inspect and automate Parallels Desktop for Mac and its Linux
or Windows VMs with `prlctl` and `prlsrvctl`. This repo is two things at once:

1. An [Agent Skills](https://agentskills.io/specification) collection installable into any agent
   with `npx skills add kastruppro/parallels-skills` (Vercel's `skills` CLI).
2. A Claude Code plugin + marketplace (`/plugin marketplace add kastruppro/parallels-skills`,
   then `/plugin install parallels@parallels-skills`).

Based on [wieslawsoltes/ParallelsSkill](https://github.com/wieslawsoltes/ParallelsSkill)
(MIT, commit `a1434b6`). Keep the upstream copyright line in `LICENSE`.

## Layout

```
skills/parallels-cli/SKILL.md                  the skill; `name` in frontmatter == folder name
skills/parallels-cli/references/               topic guides, loaded on demand from SKILL.md
skills/parallels-cli/scripts/parallels-doctor  the only code: read-only inventory (POSIX sh)
skills/parallels-cli/agents/openai.yaml        Codex UI metadata for the skill
.claude-plugin/plugin.json                     Claude Code plugin "parallels" (skills only)
.claude-plugin/marketplace.json                marketplace "parallels-skills", one plugin, source "./"
.codex-plugin/plugin.json                      Codex plugin manifest, skills -> ./skills/
```

## Rules for editing skills

- Follow the Agent Skills spec: frontmatter `name` (lowercase kebab, equal to the folder name,
  max 64 chars) and `description` (max 1024 chars). The description decides whether an agent
  uses the skill: say what it does AND when, with the words a user would actually say.
- Keep `SKILL.md` short (well under 500 lines). Long material goes in `references/`, linked by
  a relative path one level from `SKILL.md`.
- Write tool-neutral instructions. Refer to the script as `$SKILL_DIR/scripts/...` so it works
  wherever the skill is installed.
- `parallels-doctor` must stay read-only and POSIX `sh`.
- Treat local `prlctl <cmd> --help` as the syntax authority; cite first-party Parallels docs for
  new workflows.
- No absolute paths to anyone's machine, no real VM names, UUIDs or credentials. Use placeholders.
- Use plain hyphens, not em/en dashes.

## Checks before committing

```bash
# skills discovered by the skills CLI (needs Node.js)
npx skills add ./ --list

# plugin + marketplace manifests
claude plugin validate .

# doctor script syntax, and a read-only smoke run on a Mac with Parallels
sh -n skills/parallels-cli/scripts/parallels-doctor
sh skills/parallels-cli/scripts/parallels-doctor

# no personal paths
git grep -nE "/Users/|/home/[a-z]" -- skills && echo "FOUND - fix before commit"
```

## Git

Conventional Commits, one line. Do not push without an explicit instruction.
