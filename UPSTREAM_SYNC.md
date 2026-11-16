# Syncing with the original project

This repository was copied from [HevEbii/bayanihan-connect](https://github.com/HevEbii/bayanihan-connect), including the original `main` branch's commit history. The original repository is the **upstream** source. Your repository is an independent copy, so GitHub's fork-only **Sync fork** button is replaced by the manual workflow below.

## Sync on GitHub

1. Open [Actions → Sync from upstream](https://github.com/natsusawasaku123/bayanihan-connect/actions/workflows/sync-upstream.yml).
2. Click **Run workflow**, select **main**, then click **Run workflow** again.
3. Open the run to see whether new changes were merged or your repository was already up to date.

The workflow runs only when you start it. It merges the original project's `main` branch into your `main` branch, preserving your own commits and the sync workflow. If there are conflicts, it stops without pushing any changes. It never force-pushes or sends changes to the original repository.

The workflow uses GitHub's built-in `GITHUB_TOKEN` with `contents: write`; no personal token is needed for ordinary source updates. GitHub Actions must be enabled. Repository rules that require pull requests can block its direct push; use the local method and open a pull request in that case. If an upstream update changes workflow files and GitHub refuses the push because of workflow permissions, use the local method with an account allowed to update workflows.

## Sync locally or resolve a conflict

Clone your repository once:

```bash
git clone https://github.com/natsusawasaku123/bayanihan-connect.git
cd bayanihan-connect
git remote add upstream https://github.com/HevEbii/bayanihan-connect.git
```

If you already have a clone, add the `upstream` remote only if it is missing (`git remote -v` lists your remotes). Commit or stash any local work before syncing.

For each sync:

```bash
git switch main
git pull --ff-only origin main
git fetch upstream main
git merge --no-edit upstream/main
```

If Git reports conflicts, edit the listed files to resolve them, then stage each resolved file and complete the merge:

```bash
git add path/to/resolved-file
git commit
```

Check the merged changes and run any relevant project tests before publishing them:

```bash
git push origin main
```

To cancel an unfinished merge, run `git merge --abort`. A failed workflow does not leave a merge in your local clone, so start with the local sync commands above to reproduce and resolve it.

To use a pull request when repository rules require one, create a branch with `git switch -c sync-upstream` after pulling your `main`, merge `upstream/main` there, and push with `git push -u origin sync-upstream`. Open a pull request from that branch into your own `main`.
