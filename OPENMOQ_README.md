# openmoq/picoquic

Community-maintained fork of
[private-octopus/picoquic](https://github.com/private-octopus/picoquic)
for use in the openmoq / [moqx](https://github.com/openmoq/moqx)
ecosystem.

## Why the fork exists

- **Predictable snapshots** — moqx depends on a specific picoquic
  snapshot via its build pipeline. Pointing at a fork we control
  prevents upstream rebases or branch deletions from breaking our
  builds.
- **Patch capability** — if openmoq needs a picoquic change that
  hasn't landed in `private-octopus/picoquic` yet, we can carry it
  here while pursuing upstream.
- **Audit trail** — daily sync PRs document each upstream advance and
  run CI before merging into `main`.

## Branches

- `main` — our default. Tracks `private-octopus/picoquic:master`
  plus any carry-patches we maintain.
- `upstream` — read-only mirror of `private-octopus/picoquic:master`.
  Advanced fast-forward only by the daily sync workflow.
- `sync/<short-sha>` — short-lived sync PR branches; deleted on merge.

## Sync workflow

A daily `omoq-upstream-sync` GitHub Action (08:15 UTC):

1. Checks for an open `sync/*` PR; if one exists, sync is paused
   (Slack + email notification).
2. Scans the 20 newest commits on
   `private-octopus/picoquic:master` and picks the newest one whose
   upstream CI is green.
3. Fast-forwards `origin/upstream` to that commit.
4. Opens a `sync/<sha>` PR into `main`, pre-merging `main` with
   `-X ours` to resolve mechanical conflicts (carry-patches stay).
5. The `ci pr` workflow runs against the sync branch.
6. On green, `omoq-auto-merge-sync` merges the PR and deletes the
   branch.

Manual conflict resolution: push commits directly to the `sync/<sha>`
branch.

## Carry-patch policy

For any change that isn't strictly openmoq-specific, the preferred
path is contributing it to
[private-octopus/picoquic](https://github.com/private-octopus/picoquic)
following whatever process the upstream maintainers have in place.

## Tracking

Tracking issue: [openmoq/moxygen#181](https://github.com/openmoq/moxygen/issues/181).
