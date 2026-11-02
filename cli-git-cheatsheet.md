# CLI & Git Cheat Sheet

Quick reference for common Linux terminal and Git commands.

## Linux CLI

### Navigation and files

| Task | Command |
|---|---|
| Go to home directory | `cd ~` |
| Create directory | `mkdir d1` |
| Create empty file | `touch d1/a.txt` |
| Create directory and parent directories if needed | `mkdir -p d1/d2` |
| List files | `ls` |
| List hidden files | `ls -a` |
| Show current directory | `pwd` |

### File permissions

- Permissions apply to **owner**, **group** and **others**.
- `r` = read (`4`), `w` = write (`2`), `x` = execute (`1`).
- Permission digits are octal values formed by adding the permission values.
- `755` means owner `rwx`, group `r-x`, others `r-x`.
- `755` in binary permission bits is `111 101 101`.

| Task | Command |
|---|---|
| Check permissions | `ls -l d1/a.txt` |
| Set file permissions to 755 | `chmod 755 d1/a.txt` |
| Change directory and its contents recursively | `chmod -R 755 d2` |
| Set directory permissions to 755 | `find d2 -type d -exec chmod 755 {} +` |
| Set regular file permissions to 644 | `find d2 -type f -exec chmod 644 {} +` |

### Processes

| Task | Command |
|---|---|
| Start Firefox in background | `firefox &` |
| List processes | `ps aux` |
| Display process details | `ps -ef` |
| Find Firefox process IDs | `pgrep -a firefox` |
| Show PID, parent PID and command | `ps -o pid,ppid,comm -p PID` |
| Display process tree | `pstree -p` |
| Stop Firefox by process name | `pkill firefox` |
| Stop a process by PID | `kill PID` |
| Force-stop a process | `kill -9 PID` |

**Process terms**

- **PID:** Process ID.
- **PPID:** Parent Process ID.
- **Parent process:** Process that starts another process.
- **Child process:** Process started by another process.
- `kill` normally sends `SIGTERM`; use `SIGKILL` only if normal termination fails.

### Pipes and text filtering

A pipe (`|`) sends the output of one command to another command.

| Command | Purpose |
|---|---|
| `grep` | Find matching text |
| `awk` | Select and process fields |
| `xargs` | Pass input as command arguments |
| `wc -l` | Count lines |
| `2>/dev/null` | Hide error output |

Find Firefox processes and send their PIDs to `kill`:

```text
ps -eo pid,comm | awk '$2 == "firefox" {print $1}' | xargs -r kill
```

### User and system information

| Task | Command |
|---|---|
| Current username | `whoami` |
| User and group IDs | `id` |
| Current groups | `groups` |
| Kernel and system information | `uname -a` |
| Kernel name | `uname -s` |
| Kernel release | `uname -r` |
| Machine architecture | `uname -m` |
| Hostname | `uname -n` |

### Hardware and drivers

| Task | Command |
|---|---|
| Find audio hardware | `lspci | grep -i audio` |
| Find audio kernel driver | `lspci -k | grep -A 3 -i audio` |

Check the `Kernel driver in use` line to identify the active driver.

### Finding files and directories

Find names containing `java` across the system:

```text
find / -iname '*java*' 2>/dev/null
```

- `-iname` searches names without case sensitivity.
- This searches file and directory names, not file contents.
- A full-system search may take time.

### Listing files by time and size

List all home-directory entries, including hidden ones:

```text
ls -lathr ~
```

- `-l`: detailed listing.
- `-a`: include hidden entries.
- `-t`: sort by modification time.
- `-h`: human-readable sizes.
- `-r`: reverse the sort order.

### Reading files and counting words

| Task | Command |
|---|---|
| Show last 30 lines | `tail -n 30 "Harry Potter.txt"` |
| Count whole-word matches, ignoring case | `grep -oiw "harry" "Harry Potter.txt" \| wc -l` |
| Count lines, words and bytes | `wc "Harry Potter.txt"` |
| Count words only | `wc -w "Harry Potter.txt"` |

Use the correct file path if the file is not in the current directory.

---

## Git

Git tracks changes and maintains a project's version history.

### Basic workflow

| Task | Command |
|---|---|
| Initialize repository | `git init` |
| Check repository status | `git status` |
| Stage a file | `git add a.txt` |
| Stage all changes in current directory | `git add .` |
| Create a commit | `git commit -m "Add a.txt"` |
| View commit history | `git log --oneline` |
| View unstaged changes | `git diff` |
| View staged changes | `git diff --staged` |

### Branches and merging

| Task | Command |
|---|---|
| List branches | `git branch` |
| Create branch | `git branch leaf` |
| Switch branch | `git switch leaf` |
| Create and switch to branch | `git switch -c leaf` |
| Merge branch into current branch | `git merge leaf` |
| Delete local branch | `git branch -d leaf` |
| Show current branch | `git branch --show-current` |

### Sample review workflow

```text
git init
touch a.txt
git add a.txt
git commit -m "Add a.txt"
git switch -c leaf
touch b.txt
git add b.txt
git commit -m "Add b.txt"
git switch master
git merge leaf
```

If the repository uses `main`, switch to `main` instead of `master`.

### Git concepts

- **Working directory:** Files currently being created or modified.
- **Staging area:** Changes selected for the next commit.
- **Repository:** Stores committed project history.
- **Commit:** A saved snapshot of staged changes.
- **Branch:** A separate line of development.
- **HEAD:** Normally points to the currently checked-out branch.
- **Merge:** Combines changes from another branch into the current branch.

| Task | Command |
|---|---|
| Show current branch | `git branch --show-current` |
| Show latest commit | `git log -1 --oneline` |
| Show commit details | `git show` |

---

## Undoing Git Changes

Check `git status` and `git log` before undoing changes.

| Situation | Command |
|---|---|
| Correct the latest commit message | `git commit --amend -m "Correct message"` |
| Add a missed file to the latest commit | `git add file.txt` then `git commit --amend --no-edit` |
| Unstage a file, keeping its changes | `git restore --staged file.txt` |
| Discard unstaged changes to a file | `git restore file.txt` |
| Restore a file to the latest commit and unstage it | `git restore --source=HEAD --staged --worktree file.txt` |
| Safely reverse a commit | `git revert COMMIT_ID` |
| Reverse the last five commits with new commits | `git revert HEAD~5..HEAD` |
| Move current branch back five commits | `git reset --hard HEAD~5` |

### If a commit was made on the wrong branch

If the commit is local and should be kept on a new branch:

```text
git branch leaf
git reset --hard HEAD~1
git switch leaf
```

If the destination branch already exists, switch to it and use:

```text
git cherry-pick COMMIT_ID
```

**Be careful:** `git reset --hard` discards uncommitted changes and moves the branch reference. Avoid rewriting history that has already been shared unless the team agrees.

### More useful Git commands

| Task | Command |
|---|---|
| Temporarily save uncommitted changes | `git stash` |
| Reapply latest stash and remove it from stash list | `git stash pop` |
| List remote repositories | `git remote -v` |
| Show a commit | `git show COMMIT_ID` |
| Apply a commit to current branch | `git cherry-pick COMMIT_ID` |
