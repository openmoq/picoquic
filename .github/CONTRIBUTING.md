# Contributing to openmoq/picoquic

Fork of [private-octopus/picoquic](https://github.com/private-octopus/picoquic),
maintained with minimal divergence for use in the openmoq / moqx
ecosystem. Contributions welcome.

## Guiding principle

> Producing code in the era of AI is cheap. Reviewer attention is the
> scarce resource.

Because this is a fork, anything that isn't strictly openmoq-specific
is best contributed to upstream picoquic — see *Contributing upstream*
below.

## Naming convention to avoid upstream collision

Operator artifacts added by this fork live under `.github/` or use an
`omoq-` / `OPENMOQ_` prefix so they never collide with upstream
picoquic files:

- This `CONTRIBUTING.md` lives in `.github/` (upstream has none, and
  GitHub renders it automatically).
- Workflows added by the fork are named `omoq-*.yml`. Upstream
  workflows (`ci-tests.yml`, `ci-asan-ubsan.yml`, `mbedtls.yml`, …)
  are left intact.
- `.github/CODEOWNERS` is added by the fork for review routing.

If you find yourself needing to modify an upstream-owned file, prefer
upstreaming the change first.

## Pull request scope

**One PR = one cohesive thesis.** A reviewer should read the title and
predict the diff.

- ✅ `fix: h3zero parse path correctly when query string contains '?'`
- ❌ `various fixes and cleanups`
- ❌ `feature X + refactor Y` (split it)

## PR state

Useful PRs with all checks green are merged when a maintainer is
available. Signal intent:

- **Draft** — not ready for review. No auto-reviewer request; CI still runs.
- **Ready** (non-draft, no `WIP:` prefix) — merge when green.
- **`WIP:` prefix** — ready for review and CI, not for merge.

## How to contribute

- Outside contributors: fork, branch, PR against `main`.
- Org members: branch directly on this repo, PR against `main`.

PRs run CI with no secrets. Sync, publish, and deploy run only on
`push: main` after merge.

First-time fork PRs show "Waiting for approval" on Actions — a
maintainer unblocks them; subsequent runs are automatic.

## Reviews

At least one approving review from a collaborator is required.
[CODEOWNERS](CODEOWNERS) auto-requests reviewers. Review on GitHub or
[Reviewable](https://reviewable.io/reviews/openmoq/picoquic).

**Admin override** (`gh pr merge --admin`) is for:
- CI/infrastructure repairs blocked by branch protection itself.
- Release-critical merges under urgency.
- Docs-only changes when waiting costs more than reviewing.

Note the override in the PR description: `Admin override: <reason>`.

## CI

Every PR must pass the openmoq `ci pr` workflow (cmake build + ctest
on Ubuntu, mirroring upstream picoquic's `CITests` invocation). See
[workflows/omoq-ci-pr.yml](workflows/omoq-ci-pr.yml).

Picoquic upstream's own workflows continue to run too — treat them as
advisory; the required check is openmoq `ci pr`.

CI changes go in the same PR as the code that needs them.

## Branches

- `main` — working branch; all openmoq carry-patches live here.
- `upstream` — mirror of `private-octopus/picoquic:master`, advanced
  by the daily sync workflow. Do not push to it.
- `sync/<sha>` — sync PR branches from the sync bot. Push conflict
  fixes as needed.
- `devops/*`, `feature/*`, `fix/*`, `hotfix/*` — working branches.
  Convention only.

## Upstream sync

[workflows/omoq-upstream-sync.yml](workflows/omoq-upstream-sync.yml)
mirrors `private-octopus/picoquic:master` to `upstream` daily and
opens a `sync/<sha>` PR against `main`. Auto-merges on green CI;
conflicts are resolved on the `sync/<sha>` branch.

## Contributing upstream

For any change that isn't strictly openmoq-specific, please
contribute it to
[private-octopus/picoquic](https://github.com/private-octopus/picoquic)
following whatever process the upstream maintainers have in place.
That is the preferred path for all such changes.

## Merge

PRs are squash-merged; the PR title becomes the commit message on `main`.
Authors are encouraged to maintain a concise, informative commit
history on the branch — it aids review. Request a merge commit in the
PR description if preserving history on `main` is warranted.

The `omoq-sync-bot` App merges sync PRs automatically.

## Security & License

Report security issues privately rather than via public issues.
Contributions are licensed under the upstream picoquic
[LICENSE](../LICENSE) (BSD-3-Clause).
