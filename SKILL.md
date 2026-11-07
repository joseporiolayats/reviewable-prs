---
name: reviewable-prs
description: >
  Ship a change as small pull requests a human can review in one sitting.
  One workspace package and one concern per PR. Every branch is cut from the
  same base; stack only when a later slice cannot pass its checks without an
  unmerged earlier diff. Keep each diff to the smallest correct change.
  Use when the user asks for small PRs, incremental PRs, one package per PR,
  reviewable diffs, a common base instead of a stack, or runs /reviewable-prs.
  Also use before implementing work that would touch more than one package
  or grow past a single review.
license: MIT
---

# Reviewable PRs

A reviewer should be able to say what the pull request does in one sentence, and the pull request should do that whole thing and nothing beside it.

## Slice

A slice is one pull request. It is the right size when all of these hold:

- It lives in **one package**. A package is one workspace member (the project manifest the workspace includes: `pyproject.toml`, `package.json`, `Cargo.toml`, `go.mod`, or the repo's equivalent). If the repo is not a workspace, the repo is the one package, and you still slice by concern.
- It does **one concern**. A concern is one behavior change, or one behavior-preserving refactor that a later behavior needs. Tests and docs travel with the behavior they exist for. A refactor and a feature are two slices.
- A human can review it in one sitting. Split hand-written logic past about **400 lines**, or past a second package or a second concern, whichever comes first. Do not split a change that already meets the rules above. Lockfiles, generated files, and snapshots do not count toward the line cap; name them in the pull request body.
- Every changed line is required for that sentence. No drive-by format, rename, comment sweep, or "while I'm here."

Allowed beside the package: tests and docs that exist only for this slice, a lockfile or generated artifact the slice requires, and a one-line root registration the package cannot be used without (workspace member list, plugin registry). Say so in the body. A second package's behavior is never allowed.

## Base, not a stack

Resolve the base in this order: the branch the user named, else the pull request target they named, else the repository's default branch.

Cut **every** slice branch from that base:

```bash
git fetch origin
git checkout -b <branch> origin/<base>
```

Finish the slice. Do not start the next slice on that branch. Fetch again and cut the next branch from `origin/<base>` again.

**Stack only** when the approved plan marks `depends-on` **and** the later slice cannot build or pass its own checks without the earlier unmerged diff. Then branch from that parent and open the pull request against the parent's branch. Prefer the other shape: land the earlier pull request, update the base, and cut the next branch from the base.

Do not stack to keep working while review is pending, to order independent work, or because the slices are about the same feature.

If a shared package must change before a consumer can, the shared package is its own slice from the same base. The consumer is another slice from that same base. Stack the consumer only under the rule above.

## Before editing

If the work is already on a branch, read the diff and split that. Do not edit further until the plan is approved. Leave the original branch untouched. Apply each slice onto a new branch with a cherry-pick or a file subset. Never rewrite the original branch.

Write a plan. One block per slice, in the order a reviewer should read them:

```text
Slice N: <one sentence a reviewer could repeat>
  Package:    <workspace member, or "repo">
  Concern:    <behavior | refactor | docs>
  Paths:      <owned paths>
  Out:        <what you will not touch>
  Verify:     <the narrowest command>
  Base:       <branch>
  Depends-on: none | Slice M, because <the check that cannot pass>
```

Order: shared package first, then a refactor this behavior needs, then the behavior with its tests, then a separate docs or polish concern.

If one slice is enough, the plan is one block. Say that you are not splitting.

Stop. Wait for approval. Approval lets you create local branches and commits. Push and open pull requests only when the user asks, or when the approval explicitly says to publish.

## Implement one slice

1. Branch from the base as above. Follow the repo's branch and commit conventions when it has them.
2. State the smallest complete change that makes the slice's sentence true. Prefer the obvious local edit. Do not add an abstraction for one caller. Do not build for a later slice inside this one.
3. Edit only the owned paths.
4. Run the verify command. Then run the package's test command when it has one. The slice is done when those pass, or when you have reported the command you could not run and why.
5. Re-read the diff against the base. Revert any hunk that does not serve the sentence. If you noticed out-of-scope work, list it in the handoff. Do not fix it.
6. Commit on this branch only.

Then return to the base and take the next approved slice. Do not implement ahead of the slice you are on.

## Pull request

Open a **draft** against the base (against the parent branch only when the slice stacks).

Title: the slice's sentence, in the repo's title convention.

Body, and nothing else required:

- The sentence, plus the package.
- The verify command and what it reported.
- Left out on purpose (the list from the diff review).
- `Depends on #<n>` only when this slice stacks.

Do not mark the draft ready unless the user asks.

## Hand back

For each slice, report the branch, the base, whether it stacks, the verify result, and the out-of-scope list. Point at a diff that fails a rule above instead of opening it.
