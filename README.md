# reviewable-prs

<img src="assets/logo.jpg" alt="A black-and-white muscular smiling man in three-quarter view, wearing a gold medal with a glowing green plus and a glowing red minus" width="280">

Agents tend to land a feature as one diff, then stack the next branch on the one still in review. The result is a multi-package change no one can hold in their head, and later work waits on an unmerged parent.

reviewable-prs is an [Agent Skill](https://agentskills.io) that makes the agent cut that work into pull requests a human can finish in one sitting. The standard:

- **One package.** One workspace member per pull request. Tests and docs for that change travel with it. A second package is a second pull request.
- **One concern.** One behavior, or one refactor that behavior needs. A rename, a format sweep, or a "while I'm here" does not belong in the diff.
- **One base.** Every branch is cut from the same base. Stack only when a later slice cannot build or pass its own checks without the unmerged parent.
- **One sitting.** Split hand-written logic past about 400 lines. Lockfiles, generated files, and snapshots do not count; name them in the pull request body.
- **Smallest correct diff.** Every changed line has to serve the one sentence a reviewer would use to describe the pull request.

The agent writes that plan and waits. Approval covers local branches and commits. It pushes and opens draft pull requests only when you ask.

The procedure lives in [`SKILL.md`](SKILL.md). Harnesses that follow the Agent Skills format load that file, including Claude Code, Codex, Cursor, OpenCode, GitHub Copilot, and Grok.

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
