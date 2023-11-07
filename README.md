# Git (Cheat Sheet)

Previous version: [gist.github.com](https://gist.github.com/0xarthurxyz/8ba2eefd99c8f72ec50684605fb08be5)

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
git remote add upstream https://github.com/wagmi-dev/viem.git
#              ^name    ^url
```

This command tells Git to add a new remote called `upstream` pointing to the URL of the original 
repository. Remotes are like bookmarks for repositories that allow you to track the same project in 
different locations.

Source: ChatGPT

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
