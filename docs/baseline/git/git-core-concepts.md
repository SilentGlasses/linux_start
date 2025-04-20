# Core Git Concepts: Branching, Merging, and More

This page builds upon the basic introduction to Git, exploring the fundamental concepts that give Git its power and flexibility, particularly branching and merging.

**Prerequisites:** Understanding of basic Git concepts (Repository, Commit, Working Directory, Basic Workflow) from the [Introduction](./git-introduction.md).

## The Staging Area (Index) Revisited

Before diving into branching, let's clarify the **Staging Area** (also called the **Index**). Think of it as a draft space for your next commit.

- **Purpose:** It allows you to group related changes into a single, focused commit, even if you've made other unrelated changes in your working directory. You decide exactly what goes into the snapshot.
- **Command:** You use `git add <filename>` or `git add .` (to stage all changes in the current directory and subdirectories) to move changes from your Working Directory to the Staging Area.
- **Checking Status:** `git status` shows you which files are modified, staged, or untracked.

## Branching: Parallel Universes

A **branch** in Git is essentially a movable pointer to a specific commit. It represents an independent line of development.

- **Why Branch?**
    - **Isolate Work**: Develop new features or fix bugs without disrupting the main stable codebase (often the `main` or `master` branch).
    - **Experimentation**: Try out new ideas without consequence if they don't work out.
    - **Parallel Development**: Multiple team members can work on different features simultaneously on separate branches.
- **`main` / `master` Branch**: By default, your repository starts with one branch, usually called `main` (newer standard) or `master` (older standard). This typically represents the stable, production-ready version of your project.
- **HEAD**: This is a special pointer that usually indicates which branch you are currently working on (i.e., your current *checked out* branch). When you commit, the branch `HEAD` points to moves forward.
- **Commands**:
    - `git branch`: List all local branches.
    - `git branch <branch-name>`: Create a new branch pointing to the current commit.
    - `git checkout <branch-name>` or `git switch <branch-name>`: Switch your working directory (and `HEAD`) to the specified branch.
    - `git checkout -b <branch-name>` or `git switch -c <branch-name>`: Create a new branch *and* switch to it immediately.
    - `git branch -d <branch-name>`: Delete a local branch (usually after merging).

## Merging: Bringing Work Together

Once work on a branch is complete, you typically want to integrate those changes back into another branch (like `main`). This is called **merging**.

- **Process**:
    1.  Switch to the branch you want to merge *into* (e.g., `git switch main`).
    2.  Run the merge command with the name of the branch you want to merge *from* (e.g., `git merge feature-branch`).
- **Types of Merges**:
    * **Fast-Forward Merge**: If the target branch (`main`) hasn't diverged (no new commits) since the feature branch was created, Git simply moves the `main` pointer forward to match the feature branch's pointer. It's a simple, linear history.
    * **Three-Way Merge**: If both branches have new commits since they diverged, Git creates a new **merge commit**. This special commit has *two* parent commits (one from each branch) and integrates the changes from both histories.

## Merge Conflicts: When Worlds Collide

Sometimes, Git can't automatically merge changes because the *same lines* were modified differently on both branches being merged. This is a **merge conflict**.

- **What Happens**: Git stops the merge process and marks the conflicting files. Inside these files, you'll see markers like `<<<<<<<`, `=======`, and `>>>>>>>` indicating the conflicting sections from each branch.
- **Resolution**:
    1.  **Open** the conflicted file(s) in your editor.
    2.  **Edit** the file manually to keep the desired changes, removing the conflict markers.
    3.  **Stage** the resolved file(s) using `git add <resolved-file>`.
    4.  **Commit** the merge using `git commit` (Git usually provides a default merge commit message).

## Rebasing: Rewriting History (Use with Care!)

**Rebasing** is an alternative way to integrate changes from one branch onto another. Instead of creating a merge commit, it *rewrites* the commit history.

- **How it Works**: `git rebase <base-branch>` (when on your feature branch) takes all the commits unique to your current branch, temporarily removes them, updates your branch to the latest commit from `<base-branch>`, and then reapplies your unique commits one by one *on top* of the updated base.
- **Why Rebase?** It creates a cleaner, linear project history, making it easier to follow changes compared to potentially complex merge commit graphs. Often used to update a feature branch with the latest changes from `main` before merging.
- **The Golden Rule**: **Never rebase commits that have already been pushed and shared with others.** Rebasing rewrites history, which can cause significant problems for collaborators who have based their work on the original history. Use `merge` for shared branches.

## Tags: Marking Milestones

Tags are used to mark specific, important points in your repository's history, like version releases (e.g., `v1.0`, `v2.1-beta`).

- **Command**: `git tag <tag-name>` (creates a lightweight tag) or `git tag -a <tag-name> -m "Message"` (creates an annotated tag, which is generally recommended as it includes metadata like tagger, date, and message).

Understanding these core concepts, especially branching and merging, is key to leveraging Git effectively for both solo and collaborative projects.
