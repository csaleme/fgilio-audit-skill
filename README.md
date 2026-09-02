# fgilio-audit skill

A Claude Code skill that runs a coordinated, **audit-only** search for materially useful simplifications in data structures, state representation, control flow, algorithms, and ownership. It never edits files, runs tests, commits, or pushes.

## Credit

The audit prompt is by **[Aaron Francis](https://aaronfrancis.com)**: [Audit your codebase](https://gist.github.com/aarondfrancis/8735edbe48532f97ee5ea818db4dbd47). This skill wraps his prompt verbatim for the `project` scope and adds two derived variants (`branch`, `feature`) so the same method applies to a diff, a PR, or a feature spread over several repositories. The idea, the structure (coverage contract, bounded worker reviews, coordinator validation, audit the audit), and the worker brief are his. See [reference.md](skills/fgilio-audit/reference.md) for the exact provenance of each file.

Everything lives in [skills/fgilio-audit](skills/fgilio-audit): `SKILL.md` picks the scope; `prompts/` holds one prompt per scope.

## Scopes

One skill, four scopes, selected with `--scope=` (or the bare word):

- **changes (default)** — `/fgilio-audit` audits the current or last changes: the uncommitted diff if the tree is dirty, otherwise the last commit.
- **branch** — `/fgilio-audit --scope=branch [PR]` audits the current branch against its merge base and its open PR.
- **project** — `/fgilio-audit --scope=project` audits the entire codebase (Aaron's original prompt).
- **feature** — `/fgilio-audit --scope=feature repoA repoB …` audits one feature implemented across several repositories, one branch/PR each. Review areas may cross repository boundaries.

```
/fgilio-audit
/fgilio-audit branch 65
/fgilio-audit --scope=project
/fgilio-audit --scope=feature ~/dev/api ~/dev/web ~/dev/worker
```

Installed as a plugin, skills are namespaced as `/<plugin>:<skill>`, so the command becomes `/fgilio-audit:fgilio-audit`.

## Installation

### As a plugin

Installs through the [fgilio marketplace](https://github.com/fgilio/claude-plugins):

```
/plugin marketplace add fgilio/claude-plugins
/plugin install fgilio-audit@fgilio
```

### Manual clone

Clone and symlink into your skills directory (Claude Code shown; Codex uses `~/.codex/skills`):

```bash
git clone https://github.com/fgilio/fgilio-audit-skill.git ~/dev/skills/fgilio-audit-skill
ln -s ~/dev/skills/fgilio-audit-skill/skills/fgilio-audit ~/.claude/skills/fgilio-audit
```

This path also works with any agent that supports the open [Agent Skills](https://agentskills.io) format. Updates require a manual `git pull`.

## License

[MIT](LICENSE). The original prompt is © Aaron Francis and is reproduced here with attribution; see [reference.md](skills/fgilio-audit/reference.md).
