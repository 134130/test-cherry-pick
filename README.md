# test-cherry-pick

Fixture repository for manual gh-cherry-pick v3 end-to-end testing.

Run `./init` to reset this repository and create a reproducible GitHub PR
fixture. The script force-pushes `main`, closes open fixture PRs, and deletes
fixture branches whose names match the fixture branch patterns.

## Usage

```sh
./init [basic|methods|conflict|blocked|all]
```

The default scenario is `basic`.

Generated branch names are readable and deterministic by default:

```text
case-squash
target-squash
case-conflict
target-conflict
```

If you need isolated timestamped branches, set `CHERRY_PICK_BRANCH_PREFIX`.
Generated names will then use `<prefix>-case-<logical-id>` and
`<prefix>-target-<logical-id>`.

The script prints a manifest at the end that maps each logical id to the source
PR branch, demo target branch, PR URL, apply method, and expected behavior.

## Scenarios

### basic

Creates one squash-merged PR and one target branch that does not contain the PR.

Use this for the simplest current-branch demo:

```sh
git fetch origin
git switch -C demo-basic origin/target-basic
gh cherry-pick <basic-pr> --dry-run
gh cherry-pick <basic-pr> --yes
```

Expected: the command applies the PR to `demo-basic`, keeps the current branch
checked out, does not push, and does not create a new branch.

### methods

Creates merged PRs for the apply-method matrix:

```text
squash   GitHub squash merge
rebase   GitHub rebase merge
merge    GitHub merge commit
multi    multi-commit PR for explicit --method commits
```

Use this to verify auto method detection and explicit method selection:

```sh
git switch -C demo-squash origin/target-squash
gh cherry-pick <squash-pr> --dry-run --json
gh cherry-pick <squash-pr> --yes

git switch -C demo-rebase origin/target-rebase
gh cherry-pick <rebase-pr> --yes

git switch -C demo-merge origin/target-merge
gh cherry-pick <merge-pr> --yes

git switch -C demo-multi origin/target-multi
gh cherry-pick <multi-pr> --method commits --yes
```

### conflict

Creates a squash-merged PR and a target branch that edits the same line.

Use this to verify native conflict handling:

```sh
git switch -C demo-conflict origin/target-conflict
gh cherry-pick <conflict-pr> --yes
```

Expected: the repository is left in native Git conflict state and the CLI prints
recovery guidance.

### blocked

Creates one open PR.

Use this to verify mutation is blocked before applying an unmerged PR:

```sh
git switch -C demo-open origin/target-open
gh cherry-pick <open-pr> --dry-run
```

Dirty worktree behavior can be checked against any merged PR:

```sh
git switch -C demo-dirty origin/target-basic
printf 'dirty\n' > dirty.txt
gh cherry-pick <basic-pr> --yes
```

Expected: the command fails before mutation.

### all

Builds every scenario in one reset. This is the preferred setup for manual
smoke testing and external demos.
