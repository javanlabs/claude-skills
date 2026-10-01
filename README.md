# Javan Skills

Claude Code skills shared across Javan teams.

## Install

```
/plugin marketplace add javanlabs/claude-skills
/plugin install collab-tools@javan-skills
```

Update later with `/plugin marketplace update javan-skills`.

## Plugins

| Plugin | Skill | Purpose |
|---|---|---|
| `collab-tools` | `collab-content-formatter` | Format Collab task, discussion, and comment bodies as HTML; titles stay plain text |

## Adding a skill

1. Put it in `plugins/<plugin>/skills/<skill-name>/SKILL.md` (folder name = `name` in frontmatter).
2. For a new plugin, add `plugins/<plugin>/.claude-plugin/plugin.json` and list it in `.claude-plugin/marketplace.json`.
3. Bump `version` in the plugin's `plugin.json`.
4. This repo is **public** — no internal URLs, tokens, or customer data.
