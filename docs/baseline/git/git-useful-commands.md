# Useful Git Commands Beyond the Basics

You've learned the basic Git workflow, core concepts like branching/merging, and how to work with remotes. This page introduces some additional commands and options that help you inspect history, manage changes, and troubleshoot more effectively.

**Prerequisites:** Understanding of Git basics, core concepts, and remotes ([Introduction](./git-introduction.md), [Core Concepts](./git-core-concepts.md), [Remotes](./git-remotes.md)).

## Inspecting History: `git log`

The `git log` command is your window into the project's history. It has many powerful options:

- **Basic Usage:** `git log` shows commits in reverse chronological order (newest first), including commit hash, author, date, and commit message.
- **Common Formatting Options**:
    - `git log --oneline`: Shows each commit as a single line (short hash and message). Great for a quick overview.
    - `git log --graph`: Displays an ASCII graph showing branch and merge history. Often combined with `--oneline` and `--decorate` (shows branch/tag names): `git log --graph --oneline --decorate`.
    - `git log --stat`: Shows basic stats about which files were changed in each commit and how many lines were added/removed.
    - `git log -p` or `git log --patch`: Shows the full diff (changes introduced) for each commit.
- **Filtering Commits**:
    - `git log -n <number>` or `git log -<number>`: Limit output to the specified number of commits (e.g., `git log -5`).
    - `git log --author="<pattern>"`: Show commits by a specific author.
    - `git log --grep="<pattern>"`: Show commits where the message matches the pattern.
    - `git log --since="<date>" --until="<date>"`: Show commits within a date range (e.g., `"2 weeks ago"`, `"2025-01-01"`).
    - `git log <branch-or-commit>..<branch-or-commit>`: Show commits reachable from the second reference but not the first.
    - `git log -- <path/to/file>`: Show commits that affected a specific file or directory.

## Comparing Changes: `git diff`

The `git diff` command shows differences between various states in your repository.

- **Working Directory vs. Staging Area**: `git diff` shows changes in tracked files that are modified but *not yet staged*.
- **Staging Area vs. Last Commit**: `git diff --staged` (or `git diff --cached`) shows changes that *are staged* but not yet committed. This shows what *will* be in your next commit if you run `git commit` now.
- **Between Commits**: `git diff <commit1> <commit2>` shows the differences introduced between two specific commits.
- **Between Branches**: `git diff <branch1>..<branch2>` shows the differences between the tips of two branches.
- **Ignoring Whitespace**: Add `-w` or `--ignore-all-space` to ignore whitespace changes.

## Temporarily Saving Changes: `git stash`

Sometimes you need to quickly switch branches, but you have uncommitted changes in your working directory or staging area that you're not ready to commit yet. `git stash` is perfect for this.

- **Purpose**: It takes your modified tracked files and staged changes, saves them away on a stack of unfinished changes, and reverts your working directory to match the `HEAD` commit (like it was clean).
- **Commands**:
    - `git stash push -m "Optional message"` or just `git stash`: Save current changes to the stash.
    - `git stash list`: Show all the stashes you've saved.
    - `git stash apply <stash-id>`: Reapply the changes from a specific stash (e.g., `stash@{0}`) to your working directory *without* removing it from the stash list.
    - `git stash pop <stash-id>`: Reapply changes *and* remove the stash from the list (most common). If no ID is given, it applies/pops the latest stash (`stash@{0}`).
    - `git stash drop <stash-id>`: Delete a specific stash from the list.
    - `git stash clear`: Delete *all* stashes.

## Finding Faulty Commits: `git bisect`

If you discover a bug but don't know which commit introduced it, `git bisect` can help you find it quickly using a binary search through your commit history.

- **Concept**: You tell Git a "bad" commit (where the bug exists) and a "good" commit (before the bug existed). Git then checks out a commit halfway between them and asks you if it's good or bad. Based on your answer, it narrows down the range until it pinpoints the exact commit that introduced the problem.
- **Basic Workflow**:
    1.  `git bisect start`: Begin the bisect session.
    2.  `git bisect bad`: Mark the current commit (or specify one) as having the bug.
    3.  `git bisect good <commit-id>`: Mark a known commit *before* the bug existed as good.
    4.  Git checks out a commit in the middle. **Test your code now.**
    5.  Tell Git the result: `git bisect good` or `git bisect bad`.
    6.  Repeat steps 4 & 5 until Git tells you which commit is the first bad one.
    7.  `git bisect reset`: End the bisect session and return to your original branch/commit.

## Cleaning Up: `git clean` (Use with Extreme Caution!)

This command removes untracked files from your working directory. Useful for getting a truly clean state, but **dangerous because it can permanently delete files not tracked by Git.**

- **Always Preview First**: `git clean -n` (or `--dry-run`) shows you what *would* be deleted without actually deleting anything.
- **Common Usage**:
    * `git clean -f`: Force removal of untracked files.
    * `git clean -fd`: Force removal of untracked files *and* directories.
    * `git clean -fx`: Force removal of untracked files, *even those ignored by `.gitignore`*. Be very careful!

## Rewriting History (Brief Mention)

Commands like `git commit --amend` (modify the most recent commit) and `git rebase -i` (interactive rebase - allows squashing, reordering, editing commits) are powerful but rewrite history. **Use them cautiously, especially on commits already pushed to a remote.** These are generally considered more advanced topics.

These commands provide more control and insight into your Git repository, helping you manage history, compare changes, and troubleshoot effectively. Explore their `man` pages (`man git-log`, `man git-diff`, etc.) or use `--help` for even more options.
