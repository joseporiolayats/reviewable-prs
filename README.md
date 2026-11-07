# reviewable-prs

Agent skill for small, reviewable pull requests: one workspace package, one concern, every branch cut from the same base.

The instructions live in [`SKILL.md`](SKILL.md). Harnesses that follow the [Agent Skills](https://agentskills.io) format load that file. This includes Claude Code, Codex, Cursor, OpenCode, GitHub Copilot, and Grok.

## Install

Every supported harness, for all projects:

```bash
npx skills add joseporiolayats/reviewable-prs -g -a '*' -y
```

One harness. `-a` takes the skills CLI agent name. Drop `-g` to install into the current project only. From a local checkout, replace `joseporiolayats/reviewable-prs` with the folder path.

Claude Code (`~/.claude/skills/`):

```bash
npx skills add joseporiolayats/reviewable-prs -g -a claude-code -y
```

Codex (`~/.agents/skills/`):

```bash
npx skills add joseporiolayats/reviewable-prs -g -a codex -y
```

GitHub Copilot (`~/.copilot/skills/`). The agent name is `github-copilot`:

```bash
npx skills add joseporiolayats/reviewable-prs -g -a github-copilot -y
```

Hermes (`~/.hermes/skills/`). The agent name is `hermes-agent`:

```bash
npx skills add joseporiolayats/reviewable-prs -g -a hermes-agent -y
```

Pi (`~/.agents/skills/`, the same global folder Codex uses):

```bash
npx skills add joseporiolayats/reviewable-prs -g -a pi -y
```

A global Codex install and a global Pi install land in the same directory, so either command is enough for both.

Update later with `npx skills update reviewable-prs`.

## License

[MIT](LICENSE)
