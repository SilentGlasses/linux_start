# Git Troubleshooting and Common Pitfalls

Even experienced Git users run into problems from time to time. This page covers common issues, error messages, and how to recover from mistakes.

## Resolving Merge Conflicts

When two branches change the same part of a file, Git can’t merge automatically.

**How to resolve:**

1. Git will mark the conflicted files. Open them and look for `<<<<<<<`, `=======`, and `>>>>>>>`.
2. Edit the file to keep the correct changes.
3. Stage the resolved file:  
   ```bash
   git add <file>
   ```
4. Complete the merge:  
   ```bash
   git commit
   ```

## Undoing Commits

- **Undo the last commit (keep changes staged):**  
  ```bash
  git reset --soft HEAD~1
  ```
- **Undo the last commit (keep changes unstaged):**  
  ```bash
  git reset --mixed HEAD~1
  ```
- **Undo the last commit (discard changes):**  
  ```bash
  git reset --hard HEAD~1
  ```
- **Revert a specific commit (safe for shared history):**  
  ```bash
  git revert <commit-hash>
  ```

## Recovering Lost Commits with Reflog

If you lose a branch or commit, use `git reflog` to find its reference and recover it.

```bash
git reflog
git checkout <commit-hash>
```

## Fixing Detached HEAD State

If you see “detached HEAD”, you’re not on a branch.

- To save your work:  
  ```bash
  git checkout -b my-new-branch
  ```

## Dealing with Accidental Pushes

- **Undo a pushed commit (if safe):**  
  ```bash
  git revert <commit-hash>
  git push origin main
  ```
- **Force push (dangerous, avoid on shared branches):**  
  ```bash
  git push --force
  ```

## Permission Errors

- **Check your SSH keys:**  
  ```bash
  ssh -T git@github.com
  ```
- **Update remote URL to use SSH or HTTPS as needed.**

## More Resources

- [GitHub Docs: Resolving merge conflicts](https://docs.github.com/en/get-started/using-git/resolving-merge-conflicts)
- [Atlassian Git Tutorials: Reset, Revert, and Checkout](https://www.atlassian.com/git/tutorials/undoing-changes)