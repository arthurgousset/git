# Git (Cheat Sheet)

Previous version: [gist.github.com](https://gist.github.com/0xarthurxyz/8ba2eefd99c8f72ec50684605fb08be5)

## Typical workflows

### Reset local branch to match remote branch

For example, I have a fork of `wagmi-dev/viem` at `0xarthurxyz/viem` and I want my `main`
branch (in `0xarthurxyz/viem`) to match the `main` branch (in `wagmi-dev/viem`) exactly.
I want 0 files changed, 0 insertions, 0 deletions, 0 commits ahead, 0 commits behind.

That way, I can create a new branch from `main` and work on it without having to worry about
merging changes from `wagmi-dev/viem`.

Step 1: Fetch the Upstream Repository

If you haven't already configured the upstream repository as a remote, you need to add it. 
The upstream repository is the original repository from which you forked.

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

`git reset --hard` moves the current branch's tip to the specified commit, in this case, 
the tip of the `upstream/main` branch. The `--hard` flag tells Git to also reset the staging area 
and the working directory to match. This effectively discards all commits in your local `main` 
branch that are not in the upstream `main` branch.

Step 3: Force Push to Your Fork

After resetting your local branch, you can now force push this state to your GitHub fork:

```sh
git push origin main --force
#        ^name  ^branch
```

`git push` updates the remote branch with your local branch's current state. The `--force` flag 
is necessary here because you're rewriting the commit history of the `main` branch on the remote. 
This can be destructive, as it will replace the history on GitHub with the history from the 
upstream repository. Since you're working alone on this fork, it's safe to do so.

**Considerations**:

-   **Be cautious with `--force`**: Force pushing is a powerful feature that should be used with 
    caution. It can permanently erase commits if used incorrectly. Always ensure that you're force 
    pushing to the correct branch and repository.

-   **Backup your work**: Before performing operations that rewrite history, such as a hard reset, 
    it's a good practice to create a backup branch of your current state, just in case you need to 
    refer back to it.

-   **Open Pull Requests**: If you have open pull requests from your fork, resetting branches can 
    affect them. Since you're working alone, this may not be a concern, but it's something to keep 
    in mind when collaborating with others.

## Branches

### Open branch from remote repository

```sh
git checkout --track origin/my-branch-name
```

Source: 
[Git Branches: List, Create, Switch to, Merge, Push, & Delete](https://www.nobledesktop.com/learn/git/git-branches)

## Remotes

In Git, "remotes" are pointers to versions of your repository that are hosted on the internet or 
on a network somewhere, as opposed to being on your local machine.

When you work with Git, you have a local repository that lives on your computer. This repository 
contains all of your project's files and the history of changes made to those files. However, 
when you want to share your code with others or synchronize it across multiple computers, you 
need a way to connect your local repository to a remote location.

Under the hood:

-   **Configuration Files**: Remotes are configured in the `.git/config` file of your local 
    repository. This file contains the URLs and settings for your remotes.

-   **References**: Remotes work with references, which are pointers to commits. These are stored 
    in a subdirectory of the `.git` directory called `refs/remotes/`.

-   **Network Protocols**: Git supports several protocols for communicating with remotes, 
    including HTTP, HTTPS, SSH, and Git's own protocol.

-   **Security**: When you push to or pull from a remote, Git ensures the integrity of the data 
    transfer. With SSH, for example, the data is encrypted.

-   **Pruning**: Over time, a remote repository may have branches deleted. To clean up these stale 
    references, you can use `git fetch` with the `--prune` option.

-   **Tags**: Remotes also keep track of tags, which are like snapshots of your repository at a 
    certain point in time. You can push tags to remotes with `git push --tags`.

-   **Branch Tracking**: Local branches can track remote branches. This means changes in the 
    remote branch can be pulled into the local branch with a simple `git pull`, without specifying 
    the remote name and branch.

Source: ChatGPT

### Existing remotes

When you clone a repository, Git automatically adds a remote called `origin`. 
This points to the location from which you cloned the repository. 
You can see which remotes are configured with:

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

The output is expected and does not indicate duplicate remotes. In Git, `remote` refers to a 
remote repository that your local repository can track. Each remote can have multiple URLs 
associated with it for different actions---typically at least for fetching and pushing changes.

In the output above, there are two remotes configured for your local repository:

1.  **origin**: This is the default name Git gives to the remote from which you cloned your repo. 
    It has two URLs listed, one for fetching (`git fetch`, `git pull`) and one for pushing 
    (`git push`). Both actions are using the SSH protocol, as indicated by the `git@github.com` 
    syntax.

2.  **upstream**: This is the remote you've added manually, which points to the original repository 
    (often the one you forked from). Like `origin`, it has a URL for fetching and one for pushing. 
    In this case, both actions are using the HTTPS protocol, which you can tell by the `https://` 
    prefix.


Having both `origin` and `upstream` is common when you have a forked repository:

-   You would use `origin` to push and pull changes to and from your fork.
-   You would use `upstream` to fetch changes from the original repository (which you've forked) 
    to keep your local repository and your fork up-to-date with the original.

You have what you need to interact with both your fork (`origin`) and the original repository 
(`upstream`).

Source: ChatGPT

### Add remote


If you want to track another repository in addition to the one you cloned from (for example, 
the original repository when you're working on a fork), you can add a new remote:

```sh
git remote add <name> <url>
```

For example, I cloned the repository `wagmi-dev/viem` as `0xarthurxyz/viem` 
and I want to track the original repository in my local repository. In that case I can add a new
remote (arbitrarily called `upstream`) that lives at the following URL `https://github.com/wagmi-dev/viem.git`

```sh
git remote add upstream git@github.com:wagmi-dev/viem.git
#              ^name    ^url
```

This command tells Git to add a new remote called `upstream` pointing to the URL of the original 
repository. Remotes are like bookmarks for repositories that allow you to track the same project in 
different locations.

Source: ChatGPT

### Remove remote

To remove a remote, you use:

```sh
git remote remove <name>
```

For example, to remove a remote (arbitrarily called `upstream`), you run:

```sh
git remote remove upstream
#                 ^name
```

It's common to remove a remote when you no longer want to track a repository.
It's 

### Fetching from a remote

To bring in the history and changes from a remote repository, you use:

```sh
git fetch <name>
```

This command downloads objects and refs from the remote but doesn't merge any changes into your 
working files.

For example, after adding the `wagmi-dev/viem` remote as "`upstream`", you fetch the changes
from the remote by running:

```sh
git fetch upstream
#         ^name
```

`git fetch` communicates with the upstream repository and downloads the commits, files, and refs 
to your local repository. This step ensures that you have the latest history of the upstream 
project, but it doesn't modify your working directory or your current branch.
In other words, this command downloads objects and refs from the remote but doesn't merge 
any changes into your working files.

Source: ChatGPT


### Pushing to a remote

When you have commits in your local repository that you want to share with the remote, you use:

```sh
git push <name> <branch>
```

This command sends your commits to the remote repository so others can see and pull them.

Source: ChatGPT

### Pulling from a remote

When you want to update your local branch with changes from the remote, you use:

```sh
git pull <name> <branch>
```

This is effectively a `git fetch` followed by a `git merge`.

## Logs

### Commit graph

```sh
git log --oneline --graph --decorate --all
```

This command shows you the commit history with all branches and tags decorated with their 
names, presented in a graph structure to illustrate forks and merges.

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

## Commits

### Changing the last commit messsage

If you simply want to modify your last commit message, that's easy:

```sh
git commit --amend
```

The command above loads the previous commit message into an editor session, where you can make 
changes to the message, save those changes and exit. When you save and close the editor, the 
editor writes a new commit containing that updated commit message and makes it your new last commit.

Source: [git-scm.com](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)

### Changing the last commit content

If you want to change the actual *content* of your last commit, the process works basically the 
same way:

```sh
git add file # stage the changes you want to add
#       ^file name
git commit --amend # replace the last commit with the staged changes
```

First make the changes you think you forgot, stage those changes, and the subsequent 
`git commit --amend` *replaces* that last commit with your new, improved commit.

> **NOTE** 
> You need to be careful with this technique because amending changes the SHA-1 of the commit. 
> It's like a very small rebase --- don't amend your last commit if you've already pushed it.

Source: [git-scm.com](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)