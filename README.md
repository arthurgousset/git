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

### Add remote

Source: ChatGPT

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

### Fetching from a remote

Source: ChatGPT

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