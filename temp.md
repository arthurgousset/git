## Git

Here are a few resources and useful cheat sheets I found to learn Git:

- [Git docs](https://git-scm.com/doc)
- [github-git-cheat-sheet](https://training.github.com/downloads/github-git-cheat-sheet/)
- [joshnh/Git-Commands/README.md](https://github.com/joshnh/Git-Commands) (<- this one is very
  handy)
- [Git Cheat Sheet by GitHub](https://web.archive.org/web/20210413103359/https://education.github.com/git-cheat-sheet-education.pdf)
- [Step by step guide by GitHowTo](https://githowto.com/)

### Semantic commit messages

Source: Github gist by
[joshbuchea](https://gist.github.com/joshbuchea/6f47e86d2510bce28f8e7f42ae84c716)

Format: `<type>(<scope>): <subject>` (`<scope>` is optional)

Example:

```
feat: add hat wobble
^--^  ^------------^
|     |
|     +-> Summary in present tense.
|
+-------> Type: chore, docs, feat, fix, refactor, style, or test.
```

More Examples:

- `feat`: (new feature for the user, not a new feature for build script)
- `fix`: (bug fix for the user, not a fix to a build script)
- `docs`: (changes to the documentation)
- `style`: (formatting, missing semi colons, etc; no production code change)
- `refactor`: (refactoring production code, eg. renaming a variable)
- `test`: (adding missing tests, refactoring tests; no production code change)
- `chore`: (updating grunt tasks etc; no production code change)

References:

- https://www.conventionalcommits.org/
- https://seesparkbox.com/foundry/semantic_commit_messages
- http://karma-runner.github.io/1.0/dev/git-commit-msg.html

### Create new file

This command creates a file in the directory you are in.

```bash
touch test.py
```

If you want to create and edit a new file or simple edit an existing file in the terminal, you can
use `vim`:

```bash
vim test.py
```

This opens vim where you can edit the file), press `i` to enter the edit mode, type code, press
`esc` to enter command mode, type `: x` to save and exit the file.

### Create directory

```bash
mkdir [dir]
```

### Navigate to directory

```bash
cd /Users/USERNAME/FOLDER/FOLDER/
```

Easiest solution is find your folder path by right-clicking folder > press alt > //Copy "file.md" as
pathname// Type `cd`, paste pathname and press enter

### Clone git repository

```bash
git clone url
```

This creates a folder with the repo, so no need to `mkdir` beforehand.

### Pull updates from remote repo

```bash
git pull
```

### Check status of staged changes etc

```bash
git status
```

### Stage changes

```bash
git add [file-name.file-extension]
```

```bash
git add [folder]
```

```bash
git add -A
```

(to add all changes to files).

It's useful to know that Git works with the content of files and not files themselves. So when we
//add// changes to a file we deleted, we are not //adding// the file to the repo but //adding// the
changes to the file (i.e. deletions) to Git.

### Commit changes

```bash
git commit -m "[commit message]"
```

If you staged a change where the file was removed the resulting message after committing will look
something like this:

```bash
1 file changed, 28 deletions(-)
delete mode 100644 <FILENAME>.<EXTENSION>
```

### Push local branch to origin

> Don’t push your work until you’re happy with it One of the cardinal rules of Git is that, since so
> much work is local within your clone, you have a great deal of freedom to rewrite your history
> locally. However, once you push your work, it is a different story entirely, and you should
> **consider pushed work as final** unless you have good reason to change it. In short, you should
> avoid pushing your work until you’re happy with it and ready to share it with the rest of the
> world.

Source: [git-scm.com](http://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)

```bash
git push -u origin main
```

```bash
git push -u origin my-branch-name
```

The syntax is:

```bash
git push <remote> <branch>
```

where remote is `origin` by default and you current branch is used. If your current branch is
`main`, the command

```bash
git push
```

will supply the two default parameters—effectively running

```bash
git push origin main
```

Source:
[How to push a local Git branch to Origin](https://www.freecodecamp.org/news/git-push-to-remote-branch-how-to-push-a-local-branch-to-origin/#:~:text=By%20default%2C%20Git%20chooses%20origin,running%20git%20push%20origin%20main%20.)

Because my repositories are hosted on GitHub, the terminal will ask for your login credentials to
push the changes:

```bash
Username for 'https://github.com': <USERNAME>
Password for 'https://<USERNAME>@github.com': <personal_access_token>
```

Once entered, you can see the changes being pushed to the repo.

> About **personal access tokens**:
>
> In Dec 2020, GitHub
> [announced](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)
> their intent to require the use of token-based authentication (for example, a personal access,
> OAuth, or GitHub App installation token) for all authenticated Git operations. Beginning August
> 13, 2021, we will no longer accept account passwords when authenticating Git operations on
> GitHub.com.

In short, that means you cannot use your account password to authenticate yourself when pushing
changes to GitHub from the command line. Instead you will have to use a personal access token that
you can generate in Settings > Developer settings > Personal access tokens

Source:
[Creating a personal access token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)

I like to check the git status (`git status`) again to see what changes are left if I haven't
committed all changes.

### Rename repo

Source:
[Renaming a repository](https://docs.github.com/en/github/administering-a-repository/renaming-a-repository)

Go into 'settings' on GitHub and change the name there manually.

> In addition to redirecting web traffic, all git clone, git fetch, or git push operations targeting
> the previous location will continue to function as if made on the new location.
>
> However, to reduce confusion, we strongly recommend updating any existing local clones to point to
> the new repository URL. You can do this by using git remote on the command line:
>
> ```bash
> git remote set-url origin new_url
> ```

### Staging parts of a file

Source: [Stack Overflow](https://stackoverflow.com/a/1085191/21371720)

```zsh
git add -p <filename>
# or
git add --patch  <filename>
```

Git will break down your file into what it thinks are sensible "hunks" (portions of the file). It
will then prompt you with this question:

```zsh
Stage this hunk [y,n,q,a,d,/,j,J,g,s,e,?]?
```

Here is a description of each option:

- `y` stage this hunk for the next commit
- `n` do not stage this hunk for the next commit
- `q` quit; do not stage this hunk or any of the remaining hunks
- `a` stage this hunk and all later hunks in the file
- `d` do not stage this hunk or any of the later hunks in the file
- `g` select a hunk to go to
- `/` search for a hunk matching the given regex
- `j` leave this hunk undecided, see next undecided hunk
- `J` leave this hunk undecided, see next hunk
- `k` leave this hunk undecided, see previous undecided hunk
- `K` leave this hunk undecided, see previous hunk
- `s` split the current hunk into smaller hunks
- `e` manually edit the current hunk. You can then edit the hunk manually by replacing +/- by #
- `?` print hunk help

If the file is not in the repository yet, you can first do `git add -N <filename>`. Afterwards you
can go on with `git add -p <filename>`.

Afterwards, you can use:

- `git diff --staged` to check that you staged the correct changes
- `git reset -p` to unstage mistakenly added hunks
- `git commit -v` to view your commit while you edit the commit message.

### List local branches

```bash
git branch
```

### List remote branches

```bash
git branch -r
```

### List all local and remote branches

```bash
git branch -a
```

Source:
[Git Branches: List, Create, Switch to, Merge, Push, & Delete](https://www.nobledesktop.com/learn/git/git-branches)

### Create a new branch

```bash
git checkout -b my-branch-name
```

which is short for

```bash
git branch my-branch-name
git checkout my-branch-name
```

### Switch to a branch in your local repo

```bash
git checkout my-branch-name
```

Example

```bash
git checkout main
```

### Switch to a branch that came from a remote repo

To get a list of all branches from the remote, run this command:

```bash
git pull
```

Run this command to switch to the branch:

```bash
git checkout --track origin/my-branch-name
```

Source:
[Git Branches: List, Create, Switch to, Merge, Push, & Delete](https://www.nobledesktop.com/learn/git/git-branches)

### Delete local branch

```bash
git checkout main
git branch --delete branch_name
```

> You can’t delete the branch you’re currently on. First, switch to another branch and then delete

Source:
[How To Delete a Local and Remote Git Branch](https://linuxize.com/post/how-to-delete-local-and-remote-git-branch/)

### Delete remote branch

```bash
git push origin --delete branch_name
```

> In Git, local and remote branches are separate objects. Deleting a local branch doesn’t remove the
> remote branch.
>
> To delete a remote branch, use the git push command with the `-d` (`--delete`) option:

Source:
[How To Delete a Local and Remote Git Branch](https://linuxize.com/post/how-to-delete-local-and-remote-git-branch/)

### Rename branches (''`master`'' to'' `main`)

Rename master branch to newname (in this case 'main')

```bash
git branch -m master main
```

Push newname branch to origin and track origin/newname instead of origin/master

```bash
git push -u origin main
```

Change "Default branch" in Settings / Branches in Github

`Settings > Branches > click Switch sign > select new default`. Double check it worked on the code
page under branches (should show default main).

Delete origin/master

```bash
git push origin :master
```

Source:
[The easy way to rename "master" to "main" in git and GitHub](https://www.cds.caltech.edu/~mhucka/blog/posts/2020-07-19/renaming-your-master-branch-in-git/)
and
[GitHub Gist by Comevius](ttps://gist.github.com/ccopsey/9866a0bcb0b39ade04fe#gistcomment-3350164)

### Reset local branch to specific commit (e.g. HEAD)

Replacing all branch history/contents (undo commit):

```bash
git reset --hard HEAD^
```

Source: [Seth Robertson](https://sethrobertson.github.io/GitFixUm/fixup.html)

### Reset remote (origin) to a specific commit

Basically:

- go onto main branch
- reset that branch to a commit locally
- force push that commit to remote (origin)

```bash
git checkout master
git reset --hard e3f1e37
git push --force origin master
```

_Source: [Stack Overflow](https://stackoverflow.com/a/17667057)_

### Move commit from main branch to topic branch

```bash
git branch topic/wip          (1)
git reset --hard HEAD~3       (2)
git switch topic/wip          (3)
```

> 1. You have made some commits, but realize they were premature to be in the `master` branch. You
>    want to continue polishing them in a > topic branch, so create `topic/wip` branch off of the
>    current `HEAD`.
>
> 2. Rewind the `master` branch to get rid of those three commits.
>
> 3. Switch to `topic/wip` branch and keep working.

Source:
[git-scm.com](http://git-scm.com/docs/git-reset#Documentation/git-reset.txt-Undoacommitmakingitatopicbranch)

### Merge changes from origin/main into a branch

```bash
git checkout <your-branch>      # gets you on your branch
git fetch origin        # gets you up to date with origin
git merge origin/main  # merges changes from main with <your-branch>
```

Source: [Stack Overflow](https://stackoverflow.com/a/20103414)

### Add additional commit to existing PR

For example, after feedback or PR Review:

```bash
git commit -m "These changes are in response to PR comments"
git push -f origin HEAD
```

Source: [Stack Overflow](https://stackoverflow.com/a/61938071)

### Listing Your Tags

```bash
$ git tag
v1.0
v2.0
```

Source: [Git documentation](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

### Creating "lightweight" tags

> [A lightweight tag] is basically the commit checksum stored in a file — no other information is
> kept. To create a lightweight tag, don’t supply any of the -a, -s, or -m options, just provide a
> tag name:

```bash
git tag <tagname>
```

> This time, if you run git show on the tag, you don’t see the extra tag information. The command
> just shows the commit:

```bash
$ git show v1.4-lw
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    Change version number
```

Source: [Git documentation](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

### Tagging Later

> You can also tag commits after you’ve moved past them. Suppose your commit history looks like
> this:

```bash
$ git log --pretty=oneline
15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
a6b4c97498bd301d84096da251c98a07c7723e65 Create write support
0d52aaab4479697da7686c15f77a3d64d9165190 One more thing
6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc Add commit function
4682c3261057305bdd616e23b64b0857d832627b Add todo file
166ae0c4d3f420721acbb115cc33848dfcc2121a Create write support
9fceb02d0ae598e95dc970b74767f19372d61af8 Update rakefile
964f16d36dfccde844893cac5b347e7b3d44abbc Commit the todo
8a5cbc430f1a9c3d00faaeffd07798508422908a Update readme
```

> Now, suppose you forgot to tag the project at v1.2, which was at the “Update rakefile” commit. You
> can add it after the fact. To tag that commit, you specify the commit checksum (or part of it) at
> the end of the command:

```bash
git tag -a v1.2 9fceb02
```

> You can see that you’ve tagged the commit:

```bash
$ git tag
v0.1
v1.2
v1.3
v1.4
v1.4-lw
v1.5

$ git show v1.2
tag v1.2
Tagger: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Feb 9 15:32:16 2009 -0800

version 1.2
commit 9fceb02d0ae598e95dc970b74767f19372d61af8
Author: Magnus Chacon <mchacon@gee-mail.com>
Date:   Sun Apr 27 20:43:35 2008 -0700

    Update rakefile
...
```

Source: [Git documentation](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

### Sharing Tags

> By default, the `git push` command doesn’t transfer tags to remote servers.
>
> You will have to explicitly push tags to a shared server after you have created them. This process
> is just like sharing remote branches — you can run `git push origin <tagname>`.

```bash
$ git push origin v1.5
Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
Total 14 (delta 3), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.5 -> v1.5
```

Source: [Git documentation](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

### Deleting Tags

> To delete a tag on your local repository, you can use `git tag -d <tagname>`. For example, we
> could remove our lightweight tag above as follows:

```bash
$ git tag -d v1.4-lw
Deleted tag 'v1.4-lw' (was e7d5add)
```

> Note that this does not remove the tag from any remote servers. There are two common variations
> for deleting a tag from a remote server.

The second (and more intuitive) way to delete a remote tag is with:

```bash
git push origin --delete <tagname>
```

Source: [Git documentation](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

### Checking out Tags

> If you want to view the versions of files a tag is pointing to, you can do a `git checkout` of
> that tag, although this puts your repository in “detached HEAD” state, which has some ill side
> effects:

```bash
$ git checkout v2.0.0
Note: switching to 'v2.0.0'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 99ada87... Merge pull request #89 from schacon/appendix-final

$ git checkout v2.0-beta-0.1
Previous HEAD position was 99ada87... Merge pull request #89 from schacon/appendix-final
HEAD is now at df3f601... Add atlas.json and cover image
```

> In “detached HEAD” state, if you make changes and then create a commit, the tag will stay the
> same, but your new commit won’t belong to any branch and will be unreachable, except by the exact
> commit hash. Thus, if you need to make changes — say you’re fixing a bug on an older version, for
> instance — you will generally want to create a branch:

```bash
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```

> If you do this and make a commit, your `version2` branch will be slightly different than your
> `v2.0.0` tag since it will move forward with your new changes, so do be careful.

Source: [Git documentation](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

### Updating existing tags

```bash
git tag <tag_name_to_update> <hash_code_new_commit>
git push --force origin <tag_name_to_update>
```

`--force` because the commit already exists at remote. You can also delete the tag and recreate it
to avoid `--force`.

Source: [ToolsQA](https://www.toolsqa.com/git/git-delete-tag/)

### Add submodule to repo

```bash
git submodule add <URL>
```

### Add submodule to repo in certain location

```zsh
git submodule add <git@github ...> <folder-structure>/<directory-name>/
```

Source: [Stackoverflow](https://stackoverflow.com/a/9035930)

### Remove submodule from repo

- **Run `git rm <path-to-submodule>`, and commit**.

This removes the filetree at `<path-to-submodule>`, and the submodule's entry in the `.gitmodules` 
file. *I.e.* all traces of the submodule in your repository proper are removed.

As [the docs note](https://git-scm.com/docs/gitsubmodules) however, the `.git` dir of the submodule
is kept around (in the `modules/` directory of the main project's `.git` dir), "**to make it
possible to checkout past commits without requiring fetching from another repository**". If you
nonetheless want to remove this info, manually delete the submodule's directory in `.git/modules/`,
and remove the submodule's entry in the file `.git/config`.

These steps can be automated using the commands

- `rm -rf .git/modules/<path-to-submodule>`, and
- `git config --remove-section submodule.<path-to-submodule>`.

_Source: [Stack Overflow](https://stackoverflow.com/a/1260982)_ (linked from
[Atlassian](https://www.atlassian.com/git/articles/core-concept-workflows-and-tips))

### Signing commits (Github `verified`)

> To configure your Git client to sign commits by default for a local repository, in Git versions
> 2.0.0 and above, run `git config commit.gpgsign true`. To sign all commits by default in any local
> repository on your computer, run `git config --global commit.gpgsign true`.
>
> To store your GPG key passphrase so you don't have to enter it every time you sign a commit, we
> recommend using the following tools:
>
> - For Mac users, the [GPG Suite](https://gpgtools.org/) allows you to store your GPG key
>   passphrase in the Mac OS Keychain.
> - For Windows users, the [Gpg4win](https://www.gpg4win.org/) integrates with other Windows tools.

Source:
[Github docs](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)

Further instructions:

- [About commit signature verification](https://docs.github.com/en/authentication/managing-commit-signature-verification/about-commit-signature-verification)
- [Displaying verification statuses for all of your commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/displaying-verification-statuses-for-all-of-your-commits)
- [Checking for existing GPG keys](https://docs.github.com/en/authentication/managing-commit-signature-verification/checking-for-existing-gpg-keys)
- [Generating a new GPG key](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)
- [Adding a GPG key to your GitHub account](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)
- [Telling Git about your signing key](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key)
- [Associating an email with your GPG key](https://docs.github.com/en/authentication/managing-commit-signature-verification/associating-an-email-with-your-gpg-key)
- [Signing commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)
- [Signing tags](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-tags)

### List files tracked by git

```sh
git ls-tree --full-tree --name-only -r HEAD
```

- `--full-tree` makes the command run as if you were in the repo's root directory.
- `-r` recurses into subdirectories. Combined with `--full-tree` this gives you all committed,
  tracked files.
- `--name-only` removes SHA / permission info for when you just want the file paths.
- `HEAD` specifies which branch you want the list of tracked, committed files for. `HEAD` is the
  pointer for the commit you have checked out currently.

Source: [dev.to](https://dev.to/serhatteker/list-files-tracked-by-git-5gcb)