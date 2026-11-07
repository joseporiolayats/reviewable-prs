# reviewable-prs

Agent skill for small, reviewable pull requests: one workspace package, one concern, every branch cut from the same base.

The instructions live in [`SKILL.md`](SKILL.md). Harnesses that follow the [Agent Skills](https://agentskills.io) format load that file. This includes Claude Code, Codex, Cursor, OpenCode, GitHub Copilot, and Grok.

## Install

From this folder, into every supported harness, for all projects:

```bash
npx skills add /Users/b514xo/Projects/reviewable-prs -g -a '*' -y
```

After you push this repository, the same command works from the GitHub shorthand:

```bash
npx skills add <owner>/reviewable-prs -g -a '*' -y
```

Drop `-g` to install into the current project only. Replace `'*'` with one harness name, such as `claude-code` or `codex`, to install into that harness only.

Update later with `npx skills update reviewable-prs`.

## License

[MIT](LICENSE)
