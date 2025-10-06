# Working with This Fork

This repository is a personal fork of the upstream project.
Upstream changes are pulled regularly, and all personal work happens on a separate long-lived branch.

-----------------------------------------------------------------------

Branch Structure
----------------

Branch     | Purpose
----------- | ----------------------------------------------------------
main        | Mirror of the upstream main. Never commit here.
jon/dev     | Long-lived personal branch with all custom changes.
pr/*        | Temporary clean branches for pull requests upstream.

-----------------------------------------------------------------------

Initial Setup
--------------

# Add upstream remote
git remote add upstream https://github.com/ORIGINAL_OWNER/REPO.git

# Configure safer defaults
git config pull.rebase true
git config rerere.enabled true

# Sync main to match upstream
git fetch upstream
git checkout -B main upstream/main
git push -u origin main

# Create the long-lived dev branch
git checkout -b jon/dev
git push -u origin jon/dev

-----------------------------------------------------------------------

Useful Commands
---------------

# See what commits you have locally compared to upstream
git log --oneline main..jon/dev

# See a diff-style summary of your local changes
git range-diff main...jon/dev

-----------------------------------------------------------------------

Recommended Aliases
-------------------

Add these to ~/.gitconfig for convenience.

[alias]
  up = !"git fetch upstream && git checkout main && git reset --hard upstream/main && git push"
  devup = !"git checkout jon/dev && git rebase main && git push --force-with-lease"
  mine = !"git log --oneline main..jon/dev"
  rd = !"git range-diff main...jon/dev"

Usage:
git up      # Sync main from upstream
git devup   # Rebase dev on latest main

-----------------------------------------------------------------------


Regular Syncing (Weekly or as Needed)
------------------------------------

Keep your fork aligned with upstream and your dev branch rebased on top.

# 1. Update main from upstream
git up

or

git fetch upstream
git checkout main
git reset --hard upstream/main
git push -u origin main

# 2. Rebase your dev branch
git devup

or

git checkout jon/dev
git rebase main
git push --force-with-lease

This keeps history clean and avoids merge bubbles.
--force-with-lease safely updates your branch after rebasing.

-----------------------------------------------------------------------

Daily Workflow
---------------

git checkout jon/dev
# make changes
git add .
git commit -m "Describe what you changed"
git push

-----------------------------------------------------------------------

Making a Pull Request (Optional)
--------------------------------

If you want to propose changes upstream, make a clean PR branch with only the relevant commits.

# Start clean from upstream
git fetch upstream
git checkout -B main upstream/main

# Create a temporary PR branch
git checkout -b pr/feature-name

# Cherry-pick only what’s needed from your dev branch
git log --oneline main..jon/dev
git cherry-pick <commit1> [<commit2> ...]

# Push and open a PR
git push -u origin pr/feature-name

-----------------------------------------------------------------------

Handling Conflicts During Rebase
--------------------------------

# During rebase:
git rebase main
# fix conflicts in files
git add <file>
git rebase --continue

# To abort and start over:
git rebase --abort

Git will remember how you resolved similar conflicts next time (rerere.enabled).

-----------------------------------------------------------------------

Summary
-------

- Never commit directly to main.
- Always keep main equal to upstream/main.
- Do all your work on jon/dev.
- Rebase regularly to pull in upstream fixes.
- Use --force-with-lease after rebasing.
- For PRs, cherry-pick minimal commits to clean branches.
- Enable rerere to auto-apply past conflict resolutions.

This flow keeps your fork clean, rebasing painless, and your work isolated while staying up-to-date with upstream.
