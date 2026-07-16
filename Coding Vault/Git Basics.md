# Git Reference Guide

## 1. Installation and Setup

### Installation
To install or update Git via Homebrew on macOS:
```bash
brew update
brew install git
```

### Verification
To verify the installation and check the current version:
```bash
git version
```

### Getting Help
Git provides comprehensive built-in documentation.
* **Web Documentation:** [git-scm.com/docs](https://git-scm.com/docs)
* **CLI Command:** `git help git`

When navigating the terminal manual, use the following keybindings:
* `q`: Quits the manual
* `j`: Navigate one line down
* `k`: Navigate one line up
* `d`: Navigate half a page down
* `u`: Navigate half a page up
* `/<term>`: Search forward for "term"
* `n`: Jump to the next search term
* `N`: Jump to the previous search term

---

## 2. Core Concepts: Porcelain and Plumbing

Git commands are categorized into two primary interfaces:
* **Porcelain Commands:** High-level, user-friendly commands used frequently for daily version control tasks.
    * *Examples:* `git status`, `git add`, `git commit`, `git push`, `git pull`, `git log`
* **Plumbing Commands:** Low-level commands that interface directly with Git's internal object database.
    * *Examples:* `git apply`, `git commit-tree`, `git hash-object`

---

## 3. Configuration

Git allows you to customize your environment. Configuration keys are case-insensitive, and multiple values for a single key are permitted. You can even set custom, arbitrary properties (like `abc.def`), though Git will ignore them.

### Configuration Locations
From most general to most specific (a more specific location overrides a general one):
1.  **System:** `/etc/gitconfig` — Configures Git for all users on the operating system.
2.  **Global:** `~/.gitconfig` — Configures Git for all repositories of the current user.
3.  **Local:** `.git/config` — Configures Git for a specific repository.
4.  **Worktree:** `.git/config.worktree` — Configures Git for a specific worktree of a repository.

### Viewing Configurations
```bash
cat ~/.gitconfig                  # View the raw global configuration file
git config get user.name          # Retrieve a specific key's value
git config list --local           # View local configs (or just `git config list`)
git config list --global          # View global configs
```
*Note: While `list` shows all values, the `get` subcommand is best for finding specific keys.*

### Setting Configurations
```bash
git config set --global user.name "github_username_here"
git config set --global user.email "email@example.com"
git config set --global init.defaultBranch master # Set default branch (GitHub uses 'main')
git config --global core.editor "code --wait"     # Set VS Code as the default editor
```
*Tip: Use the `--local` flag instead of `--global` when dealing with repository-specific configurations.*

### Unsetting Configurations
The `unset` subcommand requires a key and cannot target an entire section by itself.
```bash
git config unset <key>                 # Remove a specific configuration value
git config unset --all example.key     # Remove all instances of a key
git config remove-section <section>    # Remove an entire configuration section
```

---

## 4. Repository Initialization & State

### Setting Up a New Repository
```bash
git init                                   # Initialize a repo in the current directory
git init -b main                           # Initialize and change the default branch to 'main'
mkdir my-repo && cd my-repo && git init    # Create directory, enter it, and initialize
```

### Status
Run `git status` to check the current state of the repository. Important possible states of files are:
* **Untracked:** Not being tracked by Git.
* **Staged:** Marked for inclusion in the next commit.
* **Committed:** Saved permanently to the repository's history.

---

## 5. Staging and Committing

### Staging Area (The Index)
The `add` command stages files for the next commit.
```bash
git add <file>          # Stages a specific file
git add file1 file2     # Stages multiple specific files
git add .               # Stages new, modified, and deleted files in the current directory and subdirectories
git add -A              # Stages all changes (new, modified, deleted) across the entire repository
git add -u              # Stages only modified and deleted files (ignores new, untracked files)
git add *.ext           # Stages files matching a pattern (e.g., all .js files)
git add -p              # Interactively stages specific parts (hunks) of files
```

### Committing
```bash
git commit -m "your message here"
git commit --amend -m "New message"   # Modifies the last commit message (Note: this changes the SHA)
```

---

## 6. History and Logging

The `git log` command shows a history of the commits. Each commit has a unique identifier called a "commit hash". For convenience, you can refer to any commit using the first 7 characters of its hash.

### Log Commands
```bash
git log                              # Runs in interactive pager mode
git --no-pager log -n 10             # Runs without pager and limits output to 10 commits
git log -1                           # Shows only the last commit
git log --oneline                    # Compact, single-line view of the log
```

### Commit Hashes
While commit hashes are derived from content changes, other factors affect the final hash:
* The commit message
* The author's name and email
* The date and time
* Parent (previous) commit hashes

Hashes (SHAs) are effectively unique in practice. While SHA-1 collisions are theoretically possible under contrived conditions, you won't accidentally create two different commits with the same hash.

### Log Decorations
A "ref" (reference) is just a pointer to a commit. All branches are refs, but not all refs are branches. The `--decorate` flag alters how these refs are displayed:
* `short` (default): Shows basic branch names.
* `full`: Shows the full ref name (e.g., `refs/heads/main`).
* `no`: Hides branch names entirely.

---

## 7. Git Internals

All data in a Git repository is stored directly in the hidden `.git` directory, specifically within `.git/objects`. A commit is simply a type of object.

### Object Storage
A commit with the hash `abcdefg...` is stored in `.git/objects/ab/cdefg...`. 
Because this file contains raw compressed bytes, running `cat .git/objects/ab/cdefg...` will output unreadable text. To view it in hexadecimal format, use `xxd .git/objects/ab/cdefg...`.

### The `cat-file` Command
Git has a built-in plumbing command to view the contents of an object without futzing with the binary files:
```bash
git cat-file -p <hash>
```

### Trees and Blobs
* **Tree:** Git's way of storing a directory.
* **Blob:** Git's way of storing a file.

Using `cat-file -p`:
1.  Providing a **commit hash** gives you the tree hash.
2.  Providing a **tree hash** gives you the blob hash.
3.  Providing a **blob hash** gives you the exact contents of the modified file.

---

## 8. Branching and HEAD

### Branches
A Git branch allows you to keep track of different changes separately. A branch is just a named pointer to a specific commit. The commit the branch points to is called the "tip" of the branch. The heads (or tips) of branches are stored as text files in `.git/refs/heads`.

```bash
git branch                           # List all branches (current branch marked with *)
git branch --no-pager                # List branches without entering pager mode
git branch -m oldname newname        # Rename a branch (omit 'oldname' to rename current branch)
git branch -d add_classics           # Delete a local branch safely
git branch -D branch_name            # Force delete a local branch (-D is shortcut for --delete --force)
```

### Creating and Switching Branches
When you create a new branch, it uses the current commit you are on as the base.
```bash
git branch my_new_branch             # Create branch
git switch -c my_new_branch          # Create and switch to the new branch (Recommended over `checkout`)

# Create a branch from a specific historical commit (newer commits won't be present)
git switch -c branch_name COMMITHASH 
```

### HEAD
Branches are references to commits, and `HEAD` is a reference to the branch you are currently on. It is stored as a file:
```bash
cat .git/HEAD
```

---

## 9. Merging

### True Merge
If you merge `other_branch` into `main`, Git combines both branches by creating a new merge commit that has *both* histories as parents. This brings all changes back into the main branch.

```text
    A - B - C - F    main
         \     /
          D - E        other_branch
```
*In this ASCII diagram, `F` is the merge commit with parents `C` and `E`.*

**The Merge Process:**
If you run `git switch main` then `git merge vimchadsonly`:
1.  Git finds the "merge base" (best common ancestor, e.g., commit `A`).
2.  It replays changes from `main`, starting from the ancestor.
3.  It replays changes from `vimchadsonly`, starting from the ancestor.
4.  It records the result as a new commit (`F`).
5.  It opens a Vim editor (by default) to edit the commit message.
*(Note: Use `git log --oneline --decorate --graph --parents` to view this visual graph concisely).*

### Fast-Forward Merge
The simplest type of merge. If the feature branch has *all* the commits that the base branch has (meaning the base branch hasn't advanced), Git simply moves the pointer forward. **No merge commit is created.**

```text
# Before Merge
             C      delete_vscode
            /
        A - B      main

# After `git merge delete_vscode`
                   delete_vscode
        A - B - C  main
```

---

## 10. Rebasing

Rebasing avoids a merge commit by replaying the commits from your feature branch directly on top of the updated base branch. 

Suppose you are on the branch `jdsl` and want to bring in changes from `main`:
```bash
git rebase main
```
**The Rebase Process:**
1.  Git identifies the latest commit from `main` to use as the temporary new base.
2.  It replays each commit from `jdsl` one at a time onto this new location.
3.  It updates the `jdsl` branch pointer to the last replayed commit.
4.  `main` remains unaffected, but `jdsl` now includes all changes from `main`.

**Important Rule:** Never rebase a public branch (like `main`) onto anything else.

---

## 11. Undoing Changes

### Reset
The `git reset` command undoes commits or changes in the index and working directory. You can use explicit hashes or relative references like `HEAD~1` (1 commit before HEAD).

* **Soft Reset:** `git reset --soft COMMITHASH`
    Goes back to a previous commit but keeps all your changes. Committed changes become uncommitted and staged; uncommitted changes remain exactly as they were.
* **Hard Reset:** `git reset --hard COMMITHASH`
    Makes your working directory and staging area match the specified commit exactly, discarding any local changes.
    *Warning:* If you delete a committed file, Git tracks it so it's easy to recover. But if you use `git reset --hard` to undo committing that file, it is deleted for good.

### Git Revert
A revert is an *anti-commit*. It does not remove the target commit (like `reset` does). Instead, it creates a new commit that applies the exact opposite of the changes, preserving a full history of the mistake and its undoing.
```bash
git revert <commit-hash>
```

---

## 12. Working with Remotes

A "remote" is another repository, usually hosted elsewhere (like GitHub). The standard convention is to name the authoritative source of truth `origin`.

### Managing Remotes
```bash
git remote -v                        # Check all remotes
git remote show <remote-name>        # Check information about a specific remote
git remote add <name> <uri>          # Add a new remote
```

### Fetching and Merging Remote Branches
Adding a remote does not automatically download its contents.
* **Fetch:** `git fetch <remote-name>`
    Downloads copies of the `.git/objects` directory and metadata. Fetching the remote `webflyx` metadata doesn't automatically update your local working files.
* **Log Remote:** `git log [<remote>/<branch>]`
* **Merge Remote:** `git merge [<remote>/<branch>]`
    *Example:* `git merge origin/primeagen` (merges the remote `primeagen` branch into your current local branch).

### Pulling
```bash
git pull [<remote>/<branch>]
```
*Note: The `[...]` syntax means those arguments are optional. Running `git pull` alone pulls the current branch from the default remote.*

### Pushing
Sends local changes to a remote. 
```bash
git push origin main
```
This means: "Take my local `main` branch and push it to the remote repository named `origin`, updating its `main` branch."

* **Explicit Push:** `git push <remote> <local-branch>:<remote-branch>`
* **Delete Remote Branch:** Push an empty branch to it: `git push origin :<remotebranch>`
* **Force Push:** `git push origin main --force`
    Overrides safeguards and forces the remote branch to match your local branch exactly. Dangerous but useful.

---

## 13. Ignoring Files (.gitignore)

The `.gitignore` file (placed at the repo root) prevents Git from tracking specific files, like dependencies (`node_modules`) or sensitive environment variables (`.env`). Note: Nested `.gitignore` files apply only to the directory they reside in.

If a file is already tracked, adding it to `.gitignore` won't remove it. You must untrack it manually:
```bash
git rm --cached advert.html
```

### .gitignore Example Logic
If your file contains `node_modules`, it ignores every path containing that section:
* *Ignored:* `node_modules/code.js`, `src/node_modules/code.js`, `src/node_modules`
* *Not Ignored:* `src/node_modules_2/code.js`, `env/node_modules_3`, `src/node_modules.js`

### Pattern Rules
1.  **Wildcards (`*`):** Matches any character except a slash.
    *Example:* `*.txt` ignores `/princess_diaries.txt` and `/contacts/your_mom.txt`.
2.  **Rooted Patterns (`/`):** Anchors the pattern to the `.gitignore` directory.
    *Example:* `/main.py` ignores `main.py` in the root, but not `/src/main.py`.
3.  **Negation (`!`):** Re-includes previously ignored files.
    *Example:* `*.txt` followed by `!/important.txt` ignores all text files *except* the root `important.txt` (Note: `/self_affirmations/important.txt` remains ignored).
4.  **Comments (`#`):** Lines starting with `#` are comments.
5.  **Order Matters:** Processed top to bottom; later patterns override earlier ones.
    *Example:* `temp/*` followed by `!temp/instructions.md` keeps the instructions. If reversed, `instructions.md` would be ignored.

---

## 14. Forks and Open Source

A fork is a copy of a repository hosted on services like GitHub or GitLab (not a native Git operation). It allows you to experiment without affecting the original project.

**Standard Contribution Workflow:**
1.  Fork their repo into your account.
2.  Clone your fork to your local machine.
3.  Create a new feature branch (`your_feature`).
4.  Make changes.
5.  Commit and push changes to your fork's remote `your_feature` branch.
6.  Create a Pull Request to the original owner's `main` branch.
*(You can also add an `upstream` remote to pull the latest changes directly from their repo).*

---

## 15. Handling Conflicts

### Merge Conflicts
During a merge conflict, Git modifies the file with markers:
* Between `<<<<<<< HEAD` and `=======`: Your branch's version (current change).
* Between `=======` and `>>>>>>> main`: The incoming version from the merged branch.

**Resolution:** Edit the files manually, remove the markers (Git will let you commit markers if you forget!), then `git add` and `git commit` to finish the merge.
You can also force one side using checkout:
* `git checkout --ours path/to/file` (Keeps your current branch's changes)
* `git checkout --theirs path/to/file` (Keeps the incoming branch's changes)

### Rebase Conflicts
Rebase conflicts feel reversed. This is because `rebase` actually checks out the *source* branch (e.g., `main`) to replay your feature branch (`banned`) on top of it.
* During a rebase, `HEAD` points to `main` (the base).
* Therefore, `--ours` refers to the branch you are rebasing *onto* (`main`).
* `--theirs` represents your feature branch (`banned`).

**Resolution:** Edit the files to resolve, `git add .`, and then run `git rebase --continue`. Do *not* commit during a rebase conflict. If you accidentally commit, run `git reset --soft HEAD~1` and then continue the rebase.

### Git Rerere
"Reuse Recorded Resolution" (`rerere`) asks Git to remember how you resolved a hunk conflict and apply it automatically if it sees the exact same conflict again.
```bash
git config set --local rerere.enabled true
rm -rf .git/rr-cache    # Command to clear the rerere cache
```

---

## 16. Advanced Commands and Tools

### Reflog
The `git reflog` (Reference Log) tracks changes to references over time, allowing you to recover from mistakes like a `reset --hard`. A "commitish" is anything that looks like a commit (branch, tag, `HEAD@{1}`).

Instead of looking up hashes in the reflog and manually running `cat-file` on trees and blobs to save to a file, you can just merge the lost state:
```bash
git merge HEAD@{1}
```

### Squashing
Squashing compresses a series of commits into a single commit using interactive rebase. It is a destructive operation—it erases the individual markers of each change, meaning you can't go back to those specific checkpoints.

1.  Start an interactive rebase: `git rebase -i HEAD~n` (`n` is the number of commits before HEAD).
2.  Git opens your editor. Change the word `pick` to `squash` for all but the first commit (oldest, usually at the top).
3.  Save and close.

### Stashing
A stash safely records the current state of your working directory *and* index (staged changes), and reverts the working directory to match `HEAD`. Stashes are stored in a Stack (LIFO - Last In, First Out), meaning you retrieve the most recent stash first.
Note that this will not work for untracked file. For them, either track them using `add` or use the `-u` flag.

```bash
git stash                        # Stash changes
git stash -m "Your message"      # Stash with a message
git stash list                   # View all stashes
git stash pop                    # Apply most recent stash and remove it from the list (undoes a stash)
git stash apply                  # Apply most recent without removing it from the list
git stash drop                   # Delete the most recent stash
git stash apply stash@{2}        # Reference a specific stash
git stash -u                     # Sta
```

### Git Diff
Shows differences between states.
```bash
git diff                           # Changes between working tree and last commit
git diff HEAD~1                    # Changes between previous commit and current state (including uncommitted)
git diff COMMIT_HASH_1 COMMIT_HASH_2 # Changes between two specific commits
```

### Cherry-Pick
When you want to "yoink" a single commit from another branch without merging the whole branch.
1. Ensure a clean working tree.
2. Identify the commit hash using `git log`.
3. Run `git cherry-pick <commit-hash>` on the destination branch.

### Bisect
Used to find the exact commit that introduced a bug (or any specific change) using a binary search (`O(log n)`) instead of checking linearly (`O(n)`).

**The 7 Steps:**
1.  Start: `git bisect start`
2.  Mark good: `git bisect good <commitish>` (where you know the bug wasn't present)
3.  Mark bad: `git bisect bad <commitish>` (where you know the bug is present)
4.  Git checks out a middle commit. Test your code.
5.  Mark the current commit via `git bisect good` or `git bisect bad`.
    *(Automation: Run `git bisect run script_name arguments`. The script should exit with code 0 if good, and a code between 1-127 (excluding 125) if bad).*
6.  Loop back to step 4 until Git isolates the exact commit.
7.  Exit: `git bisect reset`

### Git Blame
Identifies the author, timestamp, and commit hash for the last modification made to every single line of a specific file. It's heavily used to figure out who wrote a specific line of code (or when a bug was introduced).
```bash
git blame <file-name>
```

---

## 17. Worktrees

A worktree is a directory tracking your Git code. By default, your main directory is a worktree. Worktrees let you switch contexts without using branches or stashes, maintaining a light footprint.

* **Main Worktree:** Contains the heavy `.git` directory with the entire state of the repo.
* **Linked Worktree:** Contains a lightweight `.git` *file* pointing to the main worktree.

Linked worktrees behave exactly like a normal repo. Changes in a linked worktree automatically reflect in the main worktree because they share the same `.git/worktrees` data.

**Commands:**
```bash
git worktree list                    # List all worktrees
git worktree add <path> [<branch>]   # Create a new linked worktree (branch name defaults to path name)
git worktree remove WORKTREE_NAME    # Removes the worktree (but keeps the branch)
git worktree prune                   # Cleans up references if you deleted the directory manually
```
*Important Restriction:* You cannot check out a branch that is currently active in another worktree. If you run `git branch`, a branch active in another worktree will be prefixed with a `+`.

---

## 18. Tags and Versioning

A tag is a static, unmoving name linked to a commit (unlike branches, which move). Tags can be created and deleted, but not modified. Because tags act as a "commitish", they can be used anywhere a commit hash is required.

```bash
git tag                                      # List all tags
git tag -a "tag name" -m "tag message"       # Create an annotated tag on current commit
git push origin --tags                       # Push tags to the remote repository
```

### Semantic Versioning (Semver)
A naming convention (`MAJOR.MINOR.PATCH`) to communicate the impact of an update:
* **MAJOR:** Increments for breaking, backward-incompatible changes (e.g., Python 2 -> 3).
* **MINOR:** Increments for new features added in a backward-compatible manner.
* **PATCH:** Increments for backward-compatible bug fixes.

*Sorting Rules:* Compare Major -> Minor -> Patch. `2.0.0` is always greater than `1.9.9`. (Note: Major version `0` designates pre-release software).