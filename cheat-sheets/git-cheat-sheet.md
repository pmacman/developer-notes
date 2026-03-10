
# Git Cheat Sheet

## Basic Workflow

| Command                                 | Description                                           |
| --------------------------------------- | ----------------------------------------------------- |
| `git clone <url>`                       | Clone a repository locally                            |
| `git status`                            | Check status of modified, staged, and untracked files |
| `git add <file>`                        | Stage a specific file for commit                      |
| `git add .`                             | Stage all modified and new files                      |
| `git add -A`                            | Stage all changes (including deletions)               |
| `git commit -m "message"`               | Commit staged changes with a message                  |
| `git commit -S -m "message"`            | Signed commit                                         |
| `git commit --allow-empty -m "message"` | Committing without changes (used for testing)         |
| `git push origin <branch>`              | Push commits to remote repository                     |
| `git pull`                              | Fetch and merge remote changes into current branch    |

## Inspecting Changes

| Command                                      | Description                                        |
| -------------------------------------------- | -------------------------------------------------- |
| `git log`                                    | View commit history with details                   |
| `git log --oneline`                          | View commit history in compact format              |
| `git log --graph --all --decorate --oneline` | Visualize branch history                           |
| `git log -n 5`                               | Show last 5 commits                                |
| `git log --author="<name>"`                  | Show commits by a specific author                  |
| `git show <commit-id>`                       | Show changes from a specific commit                |
| `git diff`                                   | Show unstaged changes compared to last commit      |
| `git diff --staged`                          | Show staged changes ready to commit                |
| `git diff <branch-1> <branch-2>`             | Compare two branches                               |
| `git reflog`                                 | Show history of HEAD changes (useful for recovery) |

## Branch Management

| Command | Description |
|---------|-------------|
| `git branch` | List local branches |
| `git branch -r` | List remote-tracking branches |
| `git branch -a` | List all local and remote branches |
| `git branch <branch-name>` | Create a new branch |
| `git switch <branch-name>` | Switch to an existing branch |
| `git switch -c <branch-name>` | Create and switch to a new branch |
| `git checkout <branch-name>` | Switch to branch (older syntax) |
| `git checkout -b <branch-name>` | Create and switch to branch (older syntax) |
| `git branch -m <old-name> <new-name>` | Rename a branch locally |
| `git branch -v` | Show last commit on each branch |
| `git branch -d <branch-name>` | Delete a local branch (safe) |
| `git branch -D <branch-name>` | Force delete a local branch |
| `git push origin --delete <branch-name>` | Delete a remote branch |
| `git push -u origin <branch-name>` | Push new branch to remote and set upstream |

## Merging & Rebasing

| Command                                 | Description                                             |
| --------------------------------------- | ------------------------------------------------------- |
| `git merge <branch-name>`               | Merge branch into current branch                        |
| `git merge --no-ff <branch-name>`       | Merge with a merge commit (preserves branch history)    |
| `git merge --squash <branch-name>`      | Combine all commits into one before merging             |
| `git rebase <branch-name>`              | Reapply current branch commits on top of another branch |
| `git rebase -i HEAD~n`                  | Interactive rebase to modify last n commits             |
| `git cherry-pick <commit-id>`           | Apply a specific commit from another branch             |
| `git cherry-pick <commit-1> <commit-2>` | Apply multiple specific commits                         |

## Tagging

| Command                               | Description                           |
| ------------------------------------- | ------------------------------------- |
| `git tag`                             | List all tags                         |
| `git tag <tag-name>`                  | Create a lightweight tag              |
| `git tag -s <tag-name>`               | Create a lightweight signed tag       |
| `git tag -a <tag-name> -m "message"`  | Create an annotated tag with metadata |
| `git tag -d <tag-name>`               | Delete a local tag                    |
| `git push origin <tag-name>`          | Push a specific tag to remote         |
| `git push origin --delete <tag-name>` | Delete a remote tag                   |
| `git push --tags`                     | Push all tags to remote               |
| `git show <tag-name>`                 | Show tag details and commit info      |

## Undoing Changes

| Command                        | Description                                    |
| ------------------------------ | ---------------------------------------------- |
| `git reset <file>`             | Unstage a file (keep changes)                  |
| `git reset --soft HEAD~1`      | Undo last commit, keep changes staged          |
| `git reset --mixed HEAD~1`     | Undo last commit, keep changes unstaged        |
| `git reset --hard <commit-id>` | Reset to commit and discard all changes ⚠️     |
| `git revert <commit-id>`       | Create a new commit that undoes changes (safe) |
| `git restore <file>`           | Discard changes in working directory           |
| `git restore --staged <file>`  | Unstage a file (modern alternative)            |
| `git clean -fd`                | Remove untracked files and directories         |
| `git clean -fX`                | Remove only ignored files                      |
| `git stash`                    | Temporarily save changes (stash)               |
| `git stash list`               | List all stashes                               |
| `git stash pop`                | Apply latest stash and remove it               |
| `git stash apply <stash-id>`   | Apply specific stash without removing          |
| `git stash drop <stash-id>`    | Delete a stash                                 |

## Remote Repository Management

| Command | Description |
|---------|-------------|
| `git remote` | List remote repositories |
| `git remote -v` | List remotes with URLs |
| `git remote add <name> <url>` | Add a new remote |
| `git remote remove <name>` | Remove a remote |
| `git remote set-url <name> <new-url>` | Change remote URL |
| `git remote show <name>` | Show remote details |
| `git fetch` | Fetch changes without merging |
| `git fetch -p` | Fetch and prune deleted remote branches |
| `git pull` | Fetch and merge changes |
| `git pull --rebase` | Fetch and rebase instead of merge |
| `git push origin <branch>` | Push branch to remote |
| `git push -u origin <branch>` | Push and set upstream tracking |
| `git push --all` | Push all local branches |
| `git push --tags` | Push all tags |
| `git push --force-with-lease` | Force push with safety check |
| `git push --force` | Force push (use with caution) ⚠️ |

## Common Git Workflows

### Feature Branch Workflow

```bash
# Create feature branch
git switch -c feature/my-feature      

# Make changes...

git add .
git commit -m "Add feature"
git push -u origin feature/my-feature # Push to remote

# Create pull request on version control platform...
```