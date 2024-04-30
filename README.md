# Git (Cheat Sheet)

Previous version:
[gist.github.com](https://gist.github.com/0xarthurxyz/8ba2eefd99c8f72ec50684605fb08be5)

| Topics                        | Commands                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Setup and Config              | [git](https://git-scm.com/docs/git), [config](https://git-scm.com/docs/git-config), [help](https://git-scm.com/docs/git-help)                                                                                                                                                                                                                                                                                                       |
| Getting and Creating Projects | [init](https://git-scm.com/docs/git-init), [clone](https://git-scm.com/docs/git-clone)                                                                                                                                                                                                                                                                                                                                              |
| Basic Snapshotting            | [add](https://git-scm.com/docs/git-add), [status](https://git-scm.com/docs/git-status), [diff](https://git-scm.com/docs/git-diff), [commit](https://git-scm.com/docs/git-commit), [notes](https://git-scm.com/docs/git-notes), [restore](https://git-scm.com/docs/git-restore), [reset](https://git-scm.com/docs/git-reset), [rm](https://git-scm.com/docs/git-rm), [mv](https://git-scm.com/docs/git-mv)                           |
| Branching and Merging         | [branch](https://git-scm.com/docs/git-branch), [checkout](https://git-scm.com/docs/git-checkout), [switch](https://git-scm.com/docs/git-switch), [merge](https://git-scm.com/docs/git-merge), [mergetool](https://git-scm.com/docs/git-mergetool), [log](https://git-scm.com/docs/git-log), [stash](https://git-scm.com/docs/git-stash), [tag](https://git-scm.com/docs/git-tag), [worktree](https://git-scm.com/docs/git-worktree) |
| Sharing and Updating Projects | [fetch](https://git-scm.com/docs/git-fetch), [pull](https://git-scm.com/docs/git-pull), [push](https://git-scm.com/docs/git-push), [remote](https://git-scm.com/docs/git-remote), [submodule](https://git-scm.com/docs/git-submodule)                                                                                                                                                                                               |
| Inspection and Comparison     | [show](https://git-scm.com/docs/git-show), [log](https://git-scm.com/docs/git-log), [diff](https://git-scm.com/docs/git-diff), [difftool](https://git-scm.com/docs/git-difftool), [range-diff](https://git-scm.com/docs/git-range-diff), [shortlog](https://git-scm.com/docs/git-shortlog), [describe](https://git-scm.com/docs/git-describe)                                                                                       |
| Patching                      | [apply](https://git-scm.com/docs/git-apply), [cherry-pick](https://git-scm.com/docs/git-cherry-pick), [diff](https://git-scm.com/docs/git-diff), [rebase](https://git-scm.com/docs/git-rebase), [revert](https://git-scm.com/docs/git-revert)                                                                                                                                                                                       |
| Debugging                     | [bisect](https://git-scm.com/docs/git-bisect), [blame](https://git-scm.com/docs/git-blame), [grep](https://git-scm.com/docs/git-grep)                                                                                                                                                                                                                                                                                               |

Source: [git-scm.com](https://git-scm.com/docs)

## Topics

### Basic Snapshotting

#### Changing the last commit messsage

If you simply want to modify your last commit message, that's easy:

```sh
git commit --amend
```

The command above loads the previous commit message into an editor session, where you can make
changes to the message, save those changes and exit. When you save and close the editor, the editor
writes a new commit containing that updated commit message and makes it your new last commit.

Source: [git-scm.com](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)

#### Changing the last commit content (undo last commit)

If you want to change the actual *content* of your last commit, the process works basically the same
way:

```sh
git add file # stage the changes you want to add
#       ^file name
git commit --amend # replace the last commit with the staged changes
```

First make the changes you think you forgot, stage those changes, and the subsequent 
`git commit --amend` *replaces* that last commit with your new, improved commit.

> **NOTE** You need to be careful with this technique because amending changes the SHA-1 of the
> commit. It's like a very small rebase --- don't amend your last commit if you've already pushed
> it.

Source: [git-scm.com](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)

### Branching and Merging

#### `git branch -m` (rename branch)

Source:
[git-scm.com > `git branch -m`](https://git-scm.com/docs/git-branch#Documentation/git-branch.txt--m)

> Move/rename a branch, together with its config and reflog.

Syntax:

```sh
git branch --move {{current-branch-name}} {{new-branch-name}}
```

Example:

```sh
# before
git branch
  arthurgousset/feat/add-USDC-on-Celo-Alfajores
* main

git branch --move arthurgousset/feat/add-USDC-on-Celo-Alfajores arthurgousset/OLD/feat/add-USDC-on-Celo-Alfajores

# after
git branch
  arthurgousset/OLD/feat/add-USDC-on-Celo-Alfajores
* main
```

#### Open branch from remote repository

```sh
git checkout --track origin/my-branch-name
```

Source: 
[Git Branches: List, Create, Switch to, Merge, Push, & Delete](https://www.nobledesktop.com/learn/git/git-branches)

### Sharing and Updating Projects

In Git, "remotes" are pointers to versions of your repository that are hosted on the internet or on
a network somewhere, as opposed to being on your local machine.

When you work with Git, you have a local repository that lives on your computer. This repository
contains all of your project's files and the history of changes made to those files. However, when
you want to share your code with others or synchronize it across multiple computers, you need a way
to connect your local repository to a remote location.

Under the hood:

- **Configuration Files**: Remotes are configured in the `.git/config` file of your local
  repository. This file contains the URLs and settings for your remotes.

- **References**: Remotes work with references, which are pointers to commits. These are stored in a
  subdirectory of the `.git` directory called `refs/remotes/`.

- **Network Protocols**: Git supports several protocols for communicating with remotes, including
  HTTP, HTTPS, SSH, and Git's own protocol.

- **Security**: When you push to or pull from a remote, Git ensures the integrity of the data
  transfer. With SSH, for example, the data is encrypted.

- **Pruning**: Over time, a remote repository may have branches deleted. To clean up these stale
  references, you can use `git fetch` with the `--prune` option.

- **Tags**: Remotes also keep track of tags, which are like snapshots of your repository at a
  certain point in time. You can push tags to remotes with `git push --tags`.

- **Branch Tracking**: Local branches can track remote branches. This means changes in the remote
  branch can be pulled into the local branch with a simple `git pull`, without specifying the remote
  name and branch.

Source: ChatGPT

#### Existing remotes

When you clone a repository, Git automatically adds a remote called `origin`. This points to the
location from which you cloned the repository. You can see which remotes are configured with:

```sh
git remote -v
```

The `-v` flag stands for "verbose" and tells Git to show the URLs that each remote points to.

For example, in my `viem` fork I have the following remotes:

```sh
~/Documents/0xarthurxyz/viem main $ git remote -v
origin	git@github.com:0xarthurxyz/viem.git (fetch)
origin	git@github.com:0xarthurxyz/viem.git (push)
upstream	https://github.com/wagmi-dev/viem.git (fetch)
upstream	https://github.com/wagmi-dev/viem.git (push)
```

The output is expected and does not indicate duplicate remotes. In Git, `remote` refers to a remote
repository that your local repository can track. Each remote can have multiple URLs associated with
it for different actions---typically at least for fetching and pushing changes.

In the output above, there are two remotes configured for your local repository:

1.  **origin**: This is the default name Git gives to the remote from which you cloned your repo. It
    has two URLs listed, one for fetching (`git fetch`, `git pull`) and one for pushing
    (`git push`). Both actions are using the SSH protocol, as indicated by the `git@github.com`
    syntax.

2.  **upstream**: This is the remote you've added manually, which points to the original repository
    (often the one you forked from). Like `origin`, it has a URL for fetching and one for pushing.
    In this case, both actions are using the HTTPS protocol, which you can tell by the `https://`
    prefix.

Having both `origin` and `upstream` is common when you have a forked repository:

- You would use `origin` to push and pull changes to and from your fork.
- You would use `upstream` to fetch changes from the original repository (which you've forked) to
  keep your local repository and your fork up-to-date with the original.

You have what you need to interact with both your fork (`origin`) and the original repository
(`upstream`).

Source: ChatGPT

#### Add remote

If you want to track another repository in addition to the one you cloned from (for example, the
original repository when you're working on a fork), you can add a new remote:

```sh
git remote add <name> <url>
```

For example, I cloned the repository `wagmi-dev/viem` as `0xarthurxyz/viem` and I want to track the
original repository in my local repository. In that case I can add a new remote (arbitrarily called
`upstream`) that lives at the following URL `https://github.com/wagmi-dev/viem.git`

```sh
git remote add upstream git@github.com:wagmi-dev/viem.git
#              ^name    ^url
```

This command tells Git to add a new remote called `upstream` pointing to the URL of the original
repository. Remotes are like bookmarks for repositories that allow you to track the same project in
different locations.

Source: ChatGPT

#### Remove remote

To remove a remote, you use:

```sh
git remote remove <name>
```

For example, to remove a remote (arbitrarily called `upstream`), you run:

```sh
git remote remove upstream
#                 ^name
```

It's common to remove a remote when you no longer want to track a repository. It's

#### Fetching from a remote

To bring in the history and changes from a remote repository, you use:

```sh
git fetch {{repository}}
```

This command downloads objects and refs from the remote but doesn't merge any changes into your
working files.

For example, after adding the `wagmi-dev/viem` remote as "`upstream`", you fetch the changes from
the remote by running:

```sh
git fetch upstream
#         ^name
```

`git fetch` communicates with the upstream repository and downloads the commits, files, and refs to
your local repository. This step ensures that you have the latest history of the upstream project,
but it doesn't modify your working directory or your current branch. In other words, this command
downloads objects and refs from the remote but doesn't merge any changes into your working files.

Source: ChatGPT

#### Pushing to a remote

When you have commits in your local repository that you want to share with the remote, you use:

```sh
git push <name> <branch>
```

This command sends your commits to the remote repository so others can see and pull them.

Source: ChatGPT

#### Pulling from a remote

When you want to update your local branch with changes from the remote, you use:

```sh
git pull <name> <branch>
```

This is effectively a `git fetch` followed by a `git merge`.

### Inspection and Comparison

#### Visualising a commit graph

```sh
git log --oneline --graph --decorate --all
```

This command shows you the commit history with all branches and tags decorated with their names,
presented in a graph structure to illustrate forks and merges.

For example, in my `viem` fork I have the following commit graph:

```sh
~/Documents/0xarthurxyz/viem main $ git log --oneline --graph --decorate --all

*   721fc122 (HEAD -> main, origin/main, origin/HEAD) Merge branch 'wagmi-dev:main' into main
|\
| * 291e9ba2 chore: version package (#1459)
| * 46213902 fix: undefined nativeCurrency - fixes #1457
* | ec233f4b Merge branch 'wagmi-dev:main' into main
|\|
| * 6ae86cee chore: version package (#1456)
| * 880345ca chore: update snapshots
| * e40006aa feat: add Kava Testnet and edited Kava Mainnet RPC (#1453)
| * 95991301 fix: event name on `watchContractEvent` fallback
| * dd8bccc7 fix: celo rpc block type
| * e6796672 chore: version package (#1449)
| * 494dc538 chore: bump celo test coverage
| * c0da695a fix: celo-specific transaction (CIP64) regressions (#1434)
| * c2fab4a7 fix: zksync formatters (#1448)

```

## Typical workflows

### Update `feature-branch` with latest commits from the `main` branch

1. **Checkout the `main` branch**:

   ```sh
   $ git checkout main
   ```

2. **Pull the latest changes from the remote `main` branch**:

   ```sh
   $ git pull origin main
   ```

3. **Checkout your `feature-branch`**:

   ```sh
   $ git checkout {{feature-branch}}
   ```

4. **Merge the changes from the `main` branch into your `feature-branch`**:

   ```sh
   $ git merge main
   ```

5. **Resolve any merge conflicts if they occur**: If there are any merge conflicts, you will need to
   resolve them manually. Git will indicate which files have conflicts, and you can use a text
   editor or a merge tool to resolve them.

6. **Commit the merge changes**: After resolving any conflicts, you can commit the merge changes to
   your feature branch:

   ```sh
   $ git commit -am "Merge changes from main into feature branch"
   ```

7. **Push the changes to the remote feature branch**:

   ```sh
   $ git push origin {{feature-branch}}
   ```

By following these steps, your feature branch will be updated with the latest commits from the
master branch.

### Reset local branch to match remote branch

For example, I have a fork of `wagmi-dev/viem` at `0xarthurxyz/viem` and I want my `main` branch (in
`0xarthurxyz/viem`) to match the `main` branch (in `wagmi-dev/viem`) exactly. I want 0 files
changed, 0 insertions, 0 deletions, 0 commits ahead, 0 commits behind.

That way, I can create a new branch from `main` and work on it without having to worry about merging
changes from `wagmi-dev/viem`.

Step 1: Fetch the Upstream Repository

If you haven't already configured the upstream repository as a remote, you need to add it. The
upstream repository is the original repository from which you forked.

```sh
# Check if upstream is already configured
git remote -v

# If not add it
git remote add upstream git@github.com:wagmi-dev/viem.git
#              ^name    ^url
```

After adding the remote, you fetch the changes:

```sh
git fetch upstream
#         ^name
```

Step 2: Reset Your Main Branch

You need to make sure you're on your `main` branch:

```sh
git checkout main
#            ^branch
```

Then, you reset your `main` branch to the state of the `main` branch of the upstream repository:

```sh
git reset --hard upstream/main
#                ^name    ^branch
```

`git reset --hard` moves the current branch's tip to the specified commit, in this case, the tip of
the `upstream/main` branch. The `--hard` flag tells Git to also reset the staging area and the
working directory to match. This effectively discards all commits in your local `main` branch that
are not in the upstream `main` branch.

Step 3: Force Push to Your Fork

After resetting your local branch, you can now force push this state to your GitHub fork:

```sh
git push origin main --force
#        ^name  ^branch
```

`git push` updates the remote branch with your local branch's current state. The `--force` flag is
necessary here because you're rewriting the commit history of the `main` branch on the remote. This
can be destructive, as it will replace the history on GitHub with the history from the upstream
repository. Since you're working alone on this fork, it's safe to do so.

**Considerations**:

- **Be cautious with `--force`**: Force pushing is a powerful feature that should be used with
  caution. It can permanently erase commits if used incorrectly. Always ensure that you're force
  pushing to the correct branch and repository.

- **Backup your work**: Before performing operations that rewrite history, such as a hard reset,
  it's a good practice to create a backup branch of your current state, just in case you need to
  refer back to it.

- **Open Pull Requests**: If you have open pull requests from your fork, resetting branches can
  affect them. Since you're working alone, this may not be a concern, but it's something to keep in
  mind when collaborating with others.

### Re-writing commit history that has already been pushed to remote

Source: ChatGPT

If you want to rewrite the commit history of a branch, you can use an interactive rebase.

> [!WARNING] Rewriting commit history, especially on shared branches, can lead to complications for
> others who might have based their work on those commits.

1. Create a backup branch (optional):

   ```sh
   git checkout -b <backup-branch>
   ```

   It's a good practice to create a backup branch of your current state, so you can easily revert if
   something goes wrong.

1. Start the Interactive Rebase

   ```sh
   git rebase -i HEAD~5
   ```

   If you want to change the last 5 commits, you'll use the command `git rebase -i HEAD~5`. This
   command tells Git to rebase the last 5 commits interactively.

1. Select Commits to Edit

   Your default text editor opens with a list of the last 5 commits, each prefixed with the word
   pick. To change a commit message, replace pick with reword or simply r next to the commit you
   wish to update. Do this for all commits whose messages you want to change. Save and close the
   editor.

   ```sh
   pick e3a1b35 Commit message #5
   pick 7ac9a67 Commit message #4
   pick 1d2a3f4 Commit message #3
   pick 4b5c6d7 Commit message #2
   pick 8e9f0a1 Commit message #1
   ```

   Change to:

   ```sh
   reword e3a1b35 Commit message #5
   reword 7ac9a67 Commit message #4
   reword 1d2a3f4 Commit message #3
   reword 4b5c6d7 Commit message #2
   reword 8e9f0a1 Commit message #1
   ```

1. Reword Commit Messages

   After closing the editor, Git will pause at each commit you marked for rewording, allowing you to
   change its commit message. Your editor will open again for each commit, showing the current
   commit message.

   Edit the message as desired. Save and close the editor to move to the next commit. Repeat this
   process for each commit you've marked for rewording.

1. Finalizing the Rebase

   Once you've reworded all the commit messages, Git will finish the rebase. If there are no
   conflicts or issues, your branch's history now reflects the new commit messages.

1. Force Push the Changes to the Remote Repository

   Since you've altered the commit history, you'll need to force push these changes to your remote
   repository. Use the command:

   ```sh
   git push origin <your-branch-name> --force
   ```

1. Delete the Backup Branch (optional)

   If you created a backup branch, you can delete it after you've confirmed that the rebase was
   successful.

   ```sh
   git branch -D <backup-branch>
   ```

### Update fork with latest changes from `upstream` (incl. resolve conflicts)

Source: ChatGPT

1. **Fetch the latest changes from the upstream repository**:

   ```sh
   git checkout main
   git fetch upstream
   ```

2. **Merge or rebase your branch with the upstream's `main`**:

   - **Merge** (This keeps history; may introduce a merge commit if conflicts exist):
     ```bash
     git merge upstream/main  # This might prompt you to resolve conflicts
     ```
   - **Rebase** (This rewrites history to make it linear):
     ```bash
     git rebase upstream/main  # Resolve conflicts as they appear
     ```

3. **Resolve any conflicts that arise**:

   - Manually edit the files marked as conflicts.
   - After editing, mark each resolved file with:
     ```bash
     git add [file]
     ```

4. **Complete the merge or rebase**:

   - If merging:
     ```bash
     git commit  # This is needed only if there were conflicts
     ```
   - If rebasing:
     ```bash
     git rebase --continue  # Do this after each conflict resolution
     ```

5. **Push the changes to your fork**:

   ```sh
   git push origin main
   ```

### Move a commit from a `feature-branch` to a new `other-feature-branch`

Source: Warp AI

1.  Find the hash of the commit you'd like to move

    ```sh
    # Visualise the commit history
    git log --oneline --graph --decorate --all

    # Inspect the diff of a single commit
    git show {{hash}}
    ```

2.  Cut a `new-branch` branch (probably from `main`, but possibly also from the existing
    `feature-branch` from which we'll cherry pick)

    ```sh
    git checkout main
    # cut branch
    git checkout -b {{new-branch}}
    ```

3.  Add the single commit from the `feature-branch` to the `new-branch`

    ```sh
    git cherry-pick {{hash}}
    ```

### See stashed changes

Source: ChatGPT

To see all the stashes you have stored in your repository, you can use the command:

```sh
$ git stash list
```

This command will list all your stashes in a stack-like structure (last in, first out). Each entry
in the stash list is given an index, starting from 0, in the format: `stash@{index}`. For example:

```sh
stash@{0}: WIP on feature-branch: 3203abc Adding new features
stash@{1}: On master: 9fd5bca Fix something
```

If you want to see the detailed changes in a specific stash, you can use the `git stash show`
command with the `-p` (or `--patch`) option, which shows the diff of what’s in the stash as compared
to its original parent commit:

```sh
$ git stash show -p stash@{index}
```

Replace `index` with the number of the stash you want to inspect. For example:

```sh
$ git stash show -p stash@{0}
```

### Delete `feature-branch` on remote

Source: ChatGPT

1.  Delete local `feature-branch`

    ```sh
    $ git branch -D {{feature-branch}} # force delete
    ```

    To delete the branch locally, you can use the `git branch -d` command. If the branch has changes
    that haven't been merged, Git will prevent deletion to safeguard your work. If you're certain
    you want to delete it anyway, you can use `-D` which is a force delete.

1.  Push deletion to `remote`

    ```sh
    $ git push {{remote}} --delete {{feature-branch}}
    ```

    Replace `{{remote}}` with `origin` or another alias. `origin` is the default name for the
    primary remote. Replace `feature-branch` with the name of the branch you want to delete. This
    command tells Git to push a delete command for the specified branch to the remote repository.

1.  Confirm deletion by listing remote branches

    ```sh
    $ git fetch --prune
    $ git branch -r
    ```

    Here, `fetch --prune` cleans up any remote-tracking references that no longer exist on the
    remote. `git branch -r` lists all remote branches known to your local repository. These branch
    names are prefixed with the name of the remote repository, such as `origin/`, for example
    `origin/main` or `origin/feature-branch`.
