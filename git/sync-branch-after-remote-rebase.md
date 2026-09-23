# Sync local branch after someone rebased it remotely

Problem: you're working on `feature`. Someone clicks "Rebase" in the GitLab/GitHub UI (rebasing it onto the latest `main`). You keep committing locally. Now a plain `git pull` gives you a mess of conflicts or duplicate commits, because the remote history was rewritten.

## Try this first

```sh
git pull --rebase
```

With an upstream configured, `git rebase` defaults to `--fork-point`: it looks at the reflog of `origin/feature` to find where your branch *used to* fork off, and replays only your own commits onto the rebased remote. Commits whose patch is already upstream (the old `A`, `B`, `C` below) are skipped as well. Most of the time that's all you need.

## No local-only commits

If you haven't committed anything since the remote rebase, just match the remote:

```sh
git fetch origin
git reset --hard origin/feature   # also throws away uncommitted changes, stash them first
```

## Doing it by hand with `--onto`

If `pull --rebase` still replays old commits (e.g. the reflog doesn't know the old tip), transplant only your new commits explicitly:

```sh
git fetch origin
git rebase --onto origin/feature origin/feature@{1} feature
```

`origin/feature@{1}` is where the remote-tracking branch pointed *before* the last time it moved, i.e. the old (pre-rebase) tip. Git replays only the commits between that old tip and your local `feature` onto the new `origin/feature`.

`git fetch` only adds a reflog entry when the ref actually changes, so fetching several times is fine; `@{1}` is only wrong if the remote branch moved again after the rebase. Then look up the old tip yourself:

```sh
git reflog show origin/feature      # find the pre-rebase tip
git rebase --onto origin/feature <old-tip-hash> feature
```

Or, if you know how many commits you added locally (e.g. 2):

```sh
git rebase --onto origin/feature feature~2 feature
```

## What's happening

```
# after remote rebase + your local commits
origin/feature:  M1---M2---A'---B'---C'           (rebased onto latest main)
local feature:   M1---A---B---C---X---Y            (old base + your commits X, Y)

# after git rebase --onto origin/feature origin/feature@{1} feature
local feature:   M1---M2---A'---B'---C'---X'---Y'  (your commits replayed on top)
```

Your `feature` is now a fast-forward of `origin/feature`, so a normal `git push` works, no force needed.

## Avoiding this in the future

Agree on who rebases shared branches, and make `git pull` rebase by default so you never hit the merge mess:

```sh
git config --global pull.rebase true
```

More: [git-rebase docs](https://git-scm.com/docs/git-rebase), see `--onto` and `--fork-point`
