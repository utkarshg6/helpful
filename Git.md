# Git Guide

This is a guide for using Git. If you are in a hurry and you want a brief explanation of the commands you must use, this is a good place for you. You can also use it as a Git Cheatsheet.

## Global commands

### Unix Commands

Print Working Directory

```zsh
pwd
```

List all files even the hidden ones

```zsh
ls -all
```

### Configuring Git

Edit Global Git Config File

```zsh
git config --global -e
```

Edit Global Git Config File by specifying the address

```zsh
code ~/.gitconfig
```

Define Global user for git

```zsh
git config --global user.name {username}
git config --global user.email {email-id}
```

Set Default Text Editor

```zsh
git config --system core.editor {command-that-launches-text-editor}
```

### Asking for help

The help command shows the manual of a particular command

```zsh
git help {any-git-command}
```

Note: In Windows, it will open a new tab in the browser with the command's documentation.

## Git in a Project

### Git Init

To initialize `git` to an existing folder

```zsh
git init
```

To create a new `git` Directory

```zsh
git init {new-directory-name}
```

### Listing tracked files

List files that `git` is tracking

```zsh
git ls-files
```

## Types of Changes in Git

| Type                          | Color | Identifier | Understanding                |
| :---------------------------- | :---- | :--------: | :--------------------------- |
| Changes to be committed       | Green |  modified  | Files Added for staging      |
| Changes not staged for commit | Red   |  modified  | New changes in tracked files |
| Untracked files               | Red   |     -      | Newly created Files          |

## Committing changes

- Commits are the snapshots of how your project looked at a particular point in time.
- Commits can only be made locally and then can be pushed to the remote.
- You can not commit to a remote branch. To add a new commit to remote, git has the `push` command.

What is this weird looking string beside a commit?

- It is the SHA-1.
- It is the unique identifier for each commit.
- It is a hexadecimal code of 40 digits.

### Creating a commit

The basic steps to commit changes

```zsh
git add {. (recursive) or a file-name}
git commit -m "{The Commit Message}"
```

If you want to add all files and commit using one command

```zsh
git commit -am "{The Commit Message}"
```

### Amending a commit

You can make any changes to the working directory and add it to the last commit. You can

- Change the commit message
- Modify the files
- Include new files

```zsh
git commit --amend
```

It is possible to amend uncommitted changes to previous commit using the following command:

```zsh
git commit --amend --no-edit
```

This command doesn’t alters the name of the previous commit. Make sure to stage the files before running this command.

## Repositories

### Explanation

**What is a repository?**

_A repository is a central location in which data is stored and managed._

**What is a git repository?**

_A Git repository is the `.git/` folder inside a project._

### Types of repositories

| Name              | Type   | Operation                  |
| :---------------- | :----- | :------------------------- |
| Upstream          | Remote | Global Version             |
| Origin            | Remote | Forked copy of an upstream |
| Working Directory | Local  | Cloned copy of origin      |

## Syncing Changes

The `remote` can be `origin` or `upstream`

### Cloning a repository

- Creates a local repository of the remote repository.
- It is preferred to fork a project first and then clone it from `origin`.

```zsh
git clone {https://github.com/user/repo.git}
```

### Remotes in the Project

List all the remotes

```zsh
git remote -v
```

Add a new remote to a project

```zsh
git remote add {remote} {https://github.com/user/repo.git}
```

Change the URL of a remote

```zsh
git remote set-url {remote} {https://github.com/user/repo.git}
```

### Fetch

- This command simply updates the reference between local and remote repository.
- It downloads the new commits of the `remote` repository but doesn't merges them to the local branch.
- It is a non-destructive command unlike `pull`.

```zsh
git fetch {remote} {branch-name}
```

Note: The default branch for `fetch` is `origin`, in case not specified.

### Pull

- Pulls or downloads the commits from the `remote` repository and merges them into your local branch.
- It is two commands in one `fetch` and `merge`.

```zsh
git pull {remote} {branch-name}
```

### Push

- Pushes or uploads the commits to your `remote` repository.

```zsh
git push {remote} {branch-name}
```

### Create a PR using branch

First push the branch to the origin

```zsh
git push origin {branch-name}
```

Then create a PR using GitHub or other Git-based repository hosting platform.

## Branching

### Basic Operations

List local branches

```zsh
git branch
```

List all branches

- The `-all` option is an alias of `-a`.

```zsh
git branch -a
```

Create a new branch

```zsh
git branch {new-branch-name}
```

Switch branches

```zsh
git checkout {branch-name}
```

Create a new branch and checkout

```zsh
git checkout -b {new-branch-name}
```

Rename a branch

- Here `-m` flag represents move.

```zsh
git branch -m {old-name-of-branch} {new-name-of-branch}
```

Move branch to a Particular Commit

- Use it with care because the `-f` flag which means forcefully.

```zsh
git branch -f {branch-name} {commit-hash}
```

### Deleting Branches

Deleting a local branch

- The `-d` option is an alias for --delete.
- It only deletes the branch if it has already been fully merged in its upstream branch.
- You can not delete the branch you're currently checked out to.

```zsh
git branch -d {branch-name}
```

Deleting a local branch irrespective of it's merge status

- The `-D` option is an alias for `--delete --force`.

```zsh
git branch -D {branch-name}
```

Deleting a remote branch

```zsh
git push {remote} --delete {branch-name}
```

### Pushing Branches

```zsh
git push {remote} --all
```

## Merging

### Fast Forward Merging

Merge a branch in the current branch

```zsh
git merge {branch-to-be-merged}
```

### No Fast Forward Merging

- This method adds a new commit with the message **Merge branch "branch-you-merged"**.
- It preserves the Graphline.

```zsh
git merge {branch-to-be-merged} --no-ff
```

## Referencing

### Types of Referencing

| Type             | Meaning                                               | Example                                     |
| :--------------- | :---------------------------------------------------- | :------------------------------------------ |
| The Head         | It is a reference to the currently checked out commit | `HEAD^^`, `HEAD~12`, `HEAD^2`               |
| Branch Reference | It refers to the last commit of that branch           | `master^^^`, `bugFix~1`, `feature-branch^1` |
| Commit Hash      | The SHA-1 code that is unique to each commit          | `3b49920effa455cbafe8f2d4344e27b9cda09671`  |

### Referencing via commit hash

This method is used to uniquely identify a commit. 7 characters are enough for identification.

```zsh
3b49920effa455cbafe8f2d4344e27b9cda09671  # Full Commit Hash
3b49920  # Preferred length of Commit Hash
3b4  # Not preferred but may work to uniquely identify
```

### Referencing to previous commits

The total number of `^` specifies the total number of commits behind.

```zsh
HEAD^^^     # 3 commits behind HEAD
bugfix^     # 1 commit behind bugfix branch's last commit
master^^^^^ # 5 commits behind master branch's last commit.
```

The `~` followed by a number, the number specifies total commits behind.

```zsh
HEAD~3      # 3 commits behind HEAD
bugfix~     # 1 commit behind bugfix branch's last commit
master~5    # 5 commits behind master branch's last commit.
```

### Referencing to a parent

In case a commit has multiple parents then the number represents the parent.

```zsh
HEAD^2      # 1 commit behind but 2nd parent
bugfix^^^1  # 3 commit behind branch bugfix's last commit but 1st parent
master^^2   # 2 commits behind master branch's last commit but 2nd parent
```

### Nested Referencing

It may contain `~`, any number of `^` or a number

```zsh
HEAD~1^2        # 1 commit behind, 1 more commit behind and 2nd parent
bugfix^3~5^1    # 1 commit behind, 3rd parent, five commit behind, 1st parent
master~1^2~1    # 1 commit behind, 2nd parent, then one commit behind
```

## Difference

### Between Uncommitted Changes

- For difftool replace `diff` to `difftool`.
- Difftools like p4merge shows left and right changes, instead of previous or newer.

To see differences b/w Modified (red) to Staged (green)

```zsh
git diff
```

To see differences b/w Modified (red) to Committed

```zsh
git diff HEAD
```

To see differences b/w Staged (green) to Committed

```zsh
git diff --staged HEAD
```

To see differences in one file only

```zsh
git diff -- {file-name}
```

### Between different commits

To see the difference b/w two commits

```zsh
git diff {new-commit-hash} {old-commit-hash}
```

To see the difference b/w two commits using p4merge

```zsh
git difftool {left-side-commit-hash} {right-side-commit-hash}
```

To see the difference b/w last and second last commit

```zsh
git diff HEAD HEAD^
```

### Between branches

To see the difference b/w two local branches

```zsh
git diff {working-branch} {branch-with changes}
```

To see the difference b/w local and remote `master` branch

```zsh
git diff master origin/master
```

## Backing out Changes (Oops! I did a mistake.)

### Unstaging

Unstage a Staged File

```zsh
git reset HEAD {staged-file-name}
```

### Clear unstaged changes

Remove modifications of Unstaged File

```zsh
git checkout -- {unstaged-file-name}
```

## Renaming, Moving & Deleting

- Use `git mv` over `mv`, this allows `git` to see the changes easily.
- `mv` can be used for moving or renaming.

### Renaming

Renaming a file

```zsh
git mv {old-file-name} {new-file-name}
```

Renaming a file using bash and committing

```zsh
mv {old-file-name} {new-file-name}
git commit -A
```

Reversing the Rename operation

```zsh
git mv {new-file-name} {old-file-name}
```

Tell git that you've renamed a file using explorer

```zsh
git add -u
```

By doing so, git understands that file has been renamed instead of deleted and created.

### Moving

Move a file to one level down

```zsh
git mv {file-name} {folder-name}
```

Move a file to one level up

```zsh
git mv {file-name} ..
```

### Deleting

Deleting an untracked file

- `git rm` will fail because git can't delete a file that it isn't tracking.

```zsh
rm {untracked-file-name}
```

Reversing the staged delete operation

```zsh
git reset HEAD {staged-deleted-file}
```

Reversing the unstaged delete operation

```zsh
git checkout -- {unstaged-deleted-file}
```

Staging changes after deleting through bash

```zsh
git add -A
```

This command adds and updates any change to the working directory.

## Logging or Checking History

Basic Logging

```zsh
git log
```

Note: `git log -5` will limit to 5 commits.

Beautiful Logging

```zsh
git log --oneline --graph --decorate --all
```

Logging Date Wise

```zsh
git log --since="3 days ago"
```

Following all the changes to one file-name

```zsh
git log --follow -- {name-of-the-file}
```

Show all details of a particular commit

```zsh
git show {commit-hash}
```

## Describe

This command describes where you are relative to the closest tag.

```zsh
git describe {ref}
```

Where `{ref}` is anything git can resolve into a commit.
If not specified, git just uses where you're checked out right now (HEAD).

The output of the command looks like

```zsh
{tag}_{numCommits}_g{hash}
```

| Element    | Meaning                              |
| :--------- | :----------------------------------- |
| tag        | Closest ancestor tag in history      |
| numCommits | Number of commits away that tag is   |
| hash       | Hash of the commit used as reference |

## Git Alias

Generalized Command for Alias

```zsh
git config --global alias.{new-command-name} "{previous-command}"
```

A Useful Example for Alias

```zsh
git config --global alias.history "log --all --graph --decorate --oneline"
```

Git Ignore (inside a project)

```zsh
code .gitignore
```

The accepted formats for git ignore are

```git
MyFile.ext
*.ext
my-folder/
```

## Stashing

- Stash saves the uncommitted working state as a temporary commit.
- These changes can be recovered accordingly.
- The stashes are stored in form of a stack.

### Simple Stash

- While on a branch which has uncommitted changes.
- Stash only stashes the tracked files.

```zsh
git stash
```

Stash untracked files also

- Here `-u` is an alias of `--include-untracked`.

```zsh
git stash -u
```

Naming a Stash

- You can save the stash with a stash message.
- It is similar to a commit message.

```zsh
git stash save "{stash-message}"
```

### List Stashes

```zsh
git stash list
```

### Applying Stashes

- This command makes stashed changes appear again, ready to be further modified and committed.
- After running this command you can make new modifications if necessary and then commit.

```zsh
git stash apply {stash-code}
```

### Removing Stashes

Remove Temporary Stash Commits

```zsh
git stash drop {stash-code}
```

Remove all Temporary Stash Commits

```zsh
git stash clear
```

**Note: If you have a single stash then you don't need to provide a stash-code.**

### Popping a Stash

- This command performs `apply` and `drop` in the same order.
- Thus, applying the stash and deleting it.

```zsh
git stash pop
```

### Show changes of a Stash

- Perform `git stash list` to see all the stashes.
- Curly braces are used in this command.

```zsh
git stash show stash@{<num>}
```

### Migrate stash to the new branch

- The changes of the stash is migrated to a new branch.

```zsh
git stash branch {new-branch-name}
```

## Tagging

Uses of Tags:

- Add identification to a commit.
- Mark major milestones like a version number. For Example, `v-1.0`
- Used as a Release on GitHub.

### Types of Tags

| Type        | Operation                     | Command                    |
| :---------- | :---------------------------- | :------------------------- |
| Lightweight | Adds tag name beside a commit | `git tag {name-of-tag}`    |
| Annotated   | It also contains a message    | `git tag -a {name-of-tag}` |

### Lightweight Tag

Add a Lightweight Tag

```zsh
git tag {name-of-tag}
```

Add a Lightweight Tag to a particular commit

```zsh
git tag {name-of-tag} {commit-hash}
```

### Annotated Tag

Add an Annotated Tag

- This command opens a text editor to add a message.

```zsh
git tag -a {name-of-annotated-tag}
```

Add an Annotated Tag with a message through the command line

```zsh
git tag -a {name-of-annotated-tag} -m "{tag-message}"
```

Add an Annotated Tag to a particular commit

```zsh
git tag -a {name-of-annotated-tag} {commit-hash}
```

### Migrate an Annotated Tag

- The annotated tag can be migrated to a particular commit.

```zsh
git tag -a {name-of-annotated-tag} -f {commit-hash}
```

### List Tags

```zsh
git tag --list
```

### Show Tag

- It shows the details of the commit to which the particular tag points.

```zsh
git show {name-of-tag}
```

### Delete Tag

Remove a tag locally

```zsh
git tag --delete {name-of-tag}
```

Remove a tag from remote

- You are pushing **nothing** to the destination tag, hereby removing it from the remote.
- The tag will be removed from the remote but will remain in the local repository.

```zsh
git push {remote} :{tag-name}
```

### Push Tags

To push a single tag to remote

- If an unpublished commit is included in the tag, then this command will also push the commit.

```zsh
git push {remote} {name-of-tag}
```

Push all the tags

```zsh
git push {remote} {branch} --tags
```

**Note: The command `git push {remote} {master}` won't automatically push the tags. Thus `--tags` flag is necessary.**

## Reflog

- It provides the history of all the git operations that had been executed.
- The topmost row provides the latest git operation performed.
- It only saves the history of the last 60 days.

```zsh
git reflog
```

## Rebase

- Rebase adds the commits from other branches into the current branch and the current branch's commits on top of it.
- The commits of the currently active branch will appear on top.
- The other branch's commits will be added below.

### Rebasing branches

To rebase a different branch into the current branch

```zsh
git rebase {other-branch-name}
```

To rebase one branch into another

```zsh
git rebase {branch-to-appear-below} {branch-to-appear-above}
```

Note: After rebasing you'll be checked out to the above branch.

### Use Case 1

- You are working on a feature using branch `myfeature`.
- A new commit appeared on the `master` branch.
- You want to add this new commit to your `myfeature` branch.
- You want your branch commits on top of the `master` branch's new commit.

Either use this command, if you're checked out to `myfeature` branch

```zsh
git rebase master
```

Or you can use this command, you'll be checked out to `myfeature` after that

```zsh
git rebase master myfeature
```

### Use Case 2

- Rebase when branches diverge due to fetching.

If you receive a message after `git fetch`, that

```zsh
Your branch and 'origin/master' have diverged,
and have x and y different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)
```

Then rebase must be done using this command.

```zsh
git pull --rebase origin master
```

Note: Its pulling the commits of `origin/master` and will put them on top of your local branch's commits.

### Stop a rebase

```zsh
git rebase --abort
```

### What to do when a conflict appears

```zsh
git rebase {conflicting-branch}
```

Now, the conflicts will appear.

Then solve the rebase similar to the merge conflicts.

```zsh
git mergetool
```

After applying your desired changes.

```zsh
git add .
git rebase --continue
```

## Interactive Rebase

- An interactive form of rebase in which you can edit previous commits.
- It is used for rewriting history.

```zsh
git rebase -i {ref}
```

- The reference commit acts as an anchor.
- Provide the reference to that commit which you don't want to get touched.
- The commits on top of it becomes open for alteration.
- A text file will list all the commits in the inverted order of `git log`.
- It appears inverted because this text file is a script and runs from top to bottom, thus the oldest commit appears on top.
- From here various operations can be performed on the commits.

### Some useful operations

| Name     | Acronym | Action                                   |
| :------- | :------ | :--------------------------------------- |
| `pick`   | `p`     | Include this commit for rebasing         |
| `drop`   | `d`     | Remove this commit for rebasing          |
| `reword` | `r`     | Change the commit message                |
| `edit`   | `e`     | Stop at this commit for amending         |
| `squash` | `s`     | Combine this commit into previous commit |

### Renaming commit messages

- Use `reword` before the commits in the text file, after that save and close it.

Let's consider an example

The text-editor opens up displaying the following lines

```zsh
pick f7f3f6d Commit 1
reword 310154e Commit 2
reword a5f4a0d Commit 3
```

- New text editor will open for every commit that is asked for `reword`.
- Now edit the old name to a new one, after that save and close it one by one.

### Editing the previous commits

- For fixing very small changes in previous commits, `edit` is used.
- Inside the text editor, add `edit` before the commit in which you want to include your small changes.

Let's consider an example

The text-editor opens up displaying the following lines

```zsh
pick f7f3f6d Commit 1
edit 310154e Commit 2
pick a5f4a0d Commit 3
```

- The rebase will pause at that commit, now you can make your changes and amend them using this command.

```zsh
git commit --amend
```

- It can also be used for renaming commit message through `git commit --amend -m "{new-commit-message}"`.
- After editing you can resume the rebase using this command.

```zsh
git rebase --continue
```

### Reordering Commits

- The reordering can be performed by reordering the lines of an individual commit in the text editor.

Let's consider an example

The text-editor opens up displaying the following lines

```zsh
pick f7f3f6d Commit 1
pick 310154e Commit 2
pick a5f4a0d Commit 3
```

The changes that you make

```zsh
pick a5f4a0d Commit 3
pick f7f3f6d Commit 1
```

- If the commit line won't be included, it will be dropped.
- After saving the file, the rebase will start recreating this new commits sequence.
- The conflicts may appear and can be resolved as described above.

### Squashing

- Combining multiple commits or squashing them into one commit.
- The newer commits are squashed into the oldest commit, inside the text editor the oldest commit is listed on top.
- Use `pick` for the top commit which will be used as the face of other commits.
- Put `squash` beside all the subsequent commits that you want to combine.

Let's consider an example

The text-editor opens up displaying the following lines

```zsh
pick f7f3f6d Commit 1
squash 310154e Commit 2
squash a5f4a0d Commit 3
```

- After that save and close it.
- A new text editor will open which will contain all the commit messages that will be squashed into one.
- The heading will contain the commit message of the oldest commit.
- You can insert a new heading to provide a different name to this commit, if not provided the oldest commit name will be used.

#### Squashing Commits after they've been pushed to remote

- Squash commits locally with

```zsh
git rebase -i origin/master~4 master
```

- Then force push with

```zsh
git push origin +master
```

| --force                                                                        | +                                                                           |
| :----------------------------------------------------------------------------- | :-------------------------------------------------------------------------- |
| It may overwrite refs other than the current branch.                           | To force a push to only one branch, use a + in front of the refspec to push |
| It includes local refs that are even strictly behind their remote counterpart. | It only force pushes to the selected branch.                                |

### Splitting a Commit

- Splitting a commit undoes a commit and then partially stages and commits as many times as commits you want to end up with.

Let's consider an example

The text-editor opens up displaying the following lines

```zsh
pick f7f3f6d Commit before
edit 310154e Commit middle
pick a5f4a0d Commit after
```

The rebasing will have paused on the middle commit. Now perform series of commands to break it into multiple commits.

```zsh
git reset HEAD^                 # Soft reset HEAD, allowing you to open the commit's contents
git add file1.ext               # Add file(s) for the first part of the commit
git commit -m 'middle part one' # A new commit for the first part of the commit
git add file2.ext               # Add file(s) for the second part of the commit
git commit -m 'middle part two' # A new commit for the second part of the commit
git rebase --continue           # Resume rebase
```

The logs of the last four commits.

```zsh
1c002dd Commit after
9b29157 Commit middle part two
35cfb2b Commit middle part one
f3cc40e Commit before
```

- The `SHA-1` of all the rebased commits will get changed.

## Reset

- It resets the changes to the selected commit.
- By default, the last commit is used as {ref} if none provided.
- Both `HEAD` and branch reference moves to a different commit.

### Soft Reset

It simply makes all committed changes as uncommitted and sometimes results into two branches:

- Main Branch has a working directory same as before reset but as uncommitted changes.
- The other unnamed branch contains all the commits that won't be the part anymore.

```zsh
git reset {ref}
```

For Example, to reset the last commit use `git reset HEAD^`.

### Hard Reset

It resets to the particular commit and overwrites all changes in the working directory.

```zsh
git reset --hard {ref}
```

### Differences b/w Soft & Hard Reset

| State             | Soft Reset                 | Hard Reset              |
| :---------------- | :------------------------- | :---------------------- |
| Staging Area      | Resets to Unstaged         | All changes are removed |
| Working Directory | No changes will be removed | All changes are removed |

### In case everything is messed up due to `git reset`

You can resolve it through two methods:

Method 1:

- See logs using `git log --all --oneline`.
- Choose the commit and copy the commit hash.
- Perform `git reset {commit-hash}`.

Method 2:

- See previous git operations using `git reflog`.
- Select the state from where everything start going wrong.
- Copy the desired commit hash and use `git reset {commit-hash}` or copy the head state and perform `git reset HEAD@{<num>}`

## Checkout to a Commit

- Only Moves the `HEAD` to a different commit and not the branch reference.
- It results in the detached `HEAD`, which means `HEAD` is detached from the branch reference.
- You can make a new branch by using `git branch {branch-name}`. Now HEAD won't be detached anymore.
- `git checkout` is about updating the working tree (to the index or the specified tree).

```zsh
git checkout {commit-hash}
```

**Why is it called `HEAD` detached even though we can see `HEAD` pointing to a commit similar to `reset`?**

- `HEAD` is like a shadow that follows you everywhere.
- What it means by detach is that the `HEAD` is detached from the branch reference.
- Unlike `reset`, `checkout` doesn't updates the branch reference.

### Create a new branch using the checkout

You can create a new branch from the newly checked out commit

```zsh
git switch -c {new-branch-name}
```

### In case everything is messed up due to `git checkout`

You can undo the operation using `git switch -` or update the branch reference using `git checkout {branch-name}`.

## Differences between `reset` and `checkout`

- There is a very good explanation on [stackoverflow.](https://stackoverflow.com/questions/3639342/whats-the-difference-between-git-reset-and-git-checkout)

### `reset`

1. The `git reset {commit-hash}` moves the `HEAD` as well as the branch reference to a particular commit.
2. All the changes of commits on top of the current commit are reflected as the uncommitted changes.
3. To revert it, we can find the correct `HEAD` state and reset it using `git reset HEAD@{<num>}`.
4. It is used to go back in time to the point the bug carried on.

### `checkout`

1. The `git checkout {commit-hash}` only moves the `HEAD` and thus results in a detached `HEAD` from the branch reference.
2. It doesn't results in uncommitted changes.
3. To revert it we only need to update the `HEAD` to branch reference, we can use `git checkout {branch-name}`.
4. It is used to create a new branch.

## Revert

- It simply creates a **new commit** that is the opposite of an existing commit.
- It leaves the files in the same state as if the commit that has been reverted never existed.
- The new commits can be pushed to `remote` easily thus reverting commits over there also.
- In `reset` there isn't a new commit, it simply resets the `HEAD` and branch reference to a particular commit.

To revert the last commit on the branch

```zsh
git revert HEAD
```

To revert a specific commit

```zsh
git revert {bad-commit-hash}
```

To revert multiple commits

```zsh
git revert {bad-commit-hash1} {bad-commit-hash2}...{bad-commit-hash-n}
```

Note: Every bad commit will have a new revert commit and its message.

To revert a commit and provide the message inside the command line

```zsh
git revert --no-commit {bad-commit-hash}
git commit -m "{message}"
```

## Cherry-pick

- It is used to merge any number of commits in the working tree into the current branch,
- It is similar to picking commits like a cherry from a table.

Cherry-pick only one Commit

```zsh
git cherry-pick {commit-hash}
```

Cherry-pick multiple Commits

```zsh
git cherry-pick {commit-hash-1} {commit-hash-1} ...{commit-hash-3}
```

## Merging vs Rebasing after fetching Remote

| Merging                                                      | Rebasing                                                                                  |
| :----------------------------------------------------------- | :---------------------------------------------------------------------------------------- |
| The remote and local branch gives birth to a new commit.     | The local commits appear on top of the commits of the remote branch.                      |
| In Merging, the commits have two parents local and remote.   | Rebasing makes the remote branch as the parent and local branch as the orphan parent.     |
| It prevents the confusion of which commit was created first. | It may create confusion that which commit was created first due to the rewritten history. |
| The Graphline is preserved.                                  | The Graphline changes.                                                                    |
| The tree becomes non-linear.                                 | The tree stays linear.                                                                    |
| It is preferred when you want to preserve complex Graphline. | It is preferred when you want a clean linear Graphline.                                   |

## Remote Tracking

- You can make any arbitrary local branch track any remote branch.
- For example, you can make a branch named `foo` track `origin/master`.
- `git pull` will pull changes form `remote/branch`
- `git push` will push new commits on this branch to `remote/branch`

Checkout to a new branch specifying the reference to a remote branch

```zsh
git checkout -b {totally-not-master} remote/branch
```

Specify remote reference to the current branch

```zsh
git branch -u remote/branch
```

Specify remote reference to an old branch

```zsh
git branch -u remote/branch {totally-not-master}
```

Note: Here `-u` means upstream.

## Advanced Pushing

### Basics of Pushing

Pushing commits without specifying anything

- It pushes the code according to it's remote reference.
- Default remote is set to `origin`.

```zsh
git push
```

Pushing commits specifying both remote and branch

```zsh
git push {remote} {branch}
```

An example

```zsh
git push origin master
```

- Go to the local `master` branch, grab all the commits, and then go to the branch named `master` on `origin`.
- Place whatever commits are missing on that branch and then tell me when you're done.
- By specifying master as the "place" argument, we told git where the commits will come from and where the commits will go.
- Since we specified both arguments, it totally ignores where we are checked out!

### Pushing commits using the Refspec method

- You can push commits specifying remote, source and destination

```zsh
git push {remote} {source}:{destination}
```

- Here the source is any {ref} like branch reference or commit hash etc. of the local branch.
- Here destination is the remote branch.
- An example, `git push origin feature-branch^:master`.
- If the destination branch is not present on the remote, git will create a new remote branch.

### Deleting a branch from remote

- Pushing **nothing** to remote, means deleting it.

```zsh
git push origin :{destination}
```

- For example, the command `git push origin :bugFix` deletes the branch bugFix from the origin and it makes sense.

## Advanced Fetching

### Basics of Fetching

Fetching commits without specifying anything

- If git fetch receives no arguments, it just downloads all the commits from the remote onto all the remote branches.

```zsh
git fetch
```

Fetching commits specifying both remote and branch

- Downloads all the commits from the remote branch to the local `remote/branch`.

```zsh
git fetch {remote} {branch}
```

Note: The commits won't be included to the local `branch` but to the local `remote/branch`. To merge it to the local `branch`, use `git pull`.

### Fetching commits using the Refspec method

- You can fetch commits specifying remote, source and destination.

```zsh
git fetch {remote} {source}:{destination}
```

- Here the source is any {ref} like branch reference or commit hash etc. of the remote branch.
- Here destination is the local branch.
- An example, `git push origin master~1:feature-branch`.
- If the destination branch is not present on the remote, git will create a new local branch.

### Creating a new **local** branch

- Fetching **nothing** from remote means creating a new local branch.

```zsh
git fetch origin :{destination}
```

For example, the command `git fetch origin :bugFix` creates the branch `bugFix` **locally**.

## Advanced Pulling

### Basics of Pulling

- `pull` is simply two commands in one, `fetch` and `merge`.

Pulling commits specifying both remote and branch

- It pulls commits from remote's `branch` and will merge `remote/branch` to checked out branch.

```zsh
git pull {remote} {branch}
```

The above command can be written as

```zsh
git fetch {remote} {branch}
git merge {branch}
```

### Pulling commits using the Refspec method

- You can pull commits specifying remote, source and destination.
- It is similar to fetching commits by providing all the arguments and then merging `remote/destination` to checked out branch.

```zsh
git pull {remote} {source}:{destination}
```

The above command can be written as

```zsh
git fetch {remote} {source}:{destination}
git merge {destination}
```

- Here the source is any {ref} like branch reference or commit hash etc. of the remote branch.
- Here destination is the local branch.
- An example, `git pull origin master:feature-branch`.
- If the destination branch is not present on the remote, git will create a new local branch.

## Continue Learning

- [Learn Git Branching JS](https://learngitbranching.js.org/)
- [Git from the Bottom Up _by john Wiegley_](https://jwiegley.github.io/git-from-the-bottom-up/)

## The End

![Frustated You][logo]

[logo]: https://media1.tenor.com/images/121665f28c40b10a035373f8f4769706/tenor.gif
