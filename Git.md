# Git Course Notes

## Global commands

### List all files even the hidden ones

```zsh
ls -all
```

### Edit Global Git Config File

```zsh
git config --global -e
```

or

```zsh
code ~/.gitconfig
```

### Help command

```zsh
git help {any-git-command}
```

Note: In Windows, it will open browser with the command's documentation.

## Git in a Project

### Initialize `git` (in a folder)

This command can be used:

- To create an empty `git` project and start working.
- To add `git` to an existing project.

```zsh
git init
```

### List files that `git` is tracking

```zsh
git ls-files
```

## Types of Changes in Git

| Type                          | Color | Identifier | Understanding                |
| :---------------------------- | :---- | :--------: | :--------------------------- |
| Changes to be commited        | Green |  modified  | Files Added for staging      |
| Changes not staged for commit | Red   |  modified  | New changes in tracked files |
| Untracked files               | Red   |     -      | Newly created Files          |

## Commiting changes

### Basic Steps to commit changes

```zsh
git add {. (recursive) or a file-name}
git commit -m "{The Commit Message}"
```

### If you want to add all files and commit using one command, try

```zsh
git commit -am "{The Commit Message}"
```

### Change commit message if alerady commited

```zsh
git commit --amend
```

What is this weird looking string beside a commit?

- It is the SHA-1.
- It is the unique identifier for each commit.

## Repositories

**What is a repository?**

_A repository is a central location in which data is stored and managed._

**What is a git repository?**

_A Git repository is the `.git/` folder inside a project._

### Different type of repositories while working with `git`

| Name              | Type   | Operation                  |
| :---------------- | :----- | :------------------------- |
| Upstream          | Remote | Global Version             |
| Origin            | Remote | Forked copy of an upstream |
| Working Directory | Local  | Cloned copy of origin      |

## Syncing Changes

The `remote` can be `origin` or `upstream`

### Listing Remotes

```zsh
git remote -v
```

### Add the remote to a project

```zsh
git remote add {remote} {https://github.com/user/repo.git}
```

### Fetch

- This command simply updates the refernce between local and remote repository.
- It downloads the new commits of `remote` repository but doesn't merges them to the local branch.
- It is a non destructive command unlike `pull`.

```zsh
git fetch {remote} {branch-name}
```

Note: The default branch for `fetch` is `origin`, in case not specified.

### Pull

- Pulls or downloads the commits from `remote` repository and merges them into your local branch.

```zsh
git pull {remote} {branch-name}
```

### Push

- Pushes or uploads the commits to your `remote` repository.

```zsh
git push {remote} {branch-name}
```

### Create a PR using branch

First push the branch to origin

```zsh
git push origin {branch-name}
```

Then create a PR using GitHub or other Git-based repository hosting platform.

## Backing out Changes (Oops! I did a mistake.)

### Unstage a Staged File

```zsh
git reset HEAD {staged-file-name}
```

### Remove modifications of Unstaged File

```zsh
git checkout -- {unstaged-file-name}
```

## Renaming and Moving files (Prefer `git mv` over `mv`)

### Renaming a file

```zsh
git mv {old-file-name} {new-file-name}
```

### Renaming a file using bash and commiting

```zsh
mv {old-file-name} {new-file-name}
git commit -A
```

### Reversing the Rename operation

```zsh
git mv {new-file-name} {old-file-name}
```

### Move a file to one level down

```zsh
git mv {file-name} {folder-name}
```

### Move a file to one level up

```zsh
git mv {file-name} ..
```

### Renaming a file using explorer (after renaming)

```zsh
git add -u
```

So, that the git understands that file has been renamed instead of deleted and created.

## Deleting a file

### Deleting an untracked file (`git rm` will fail)

```zsh
rm {untracked-file-name}
```

### Reversing the staged delete operation

```zsh
git reset HEAD {staged-deleted-file}
```

### Reversing the unstaged delete operation

```zsh
git checkout -- {unstaged-deleted-file}
```

### Staging changes after deleting through bash

```zsh
git add -A
```

This command adds and updates any change to the working directory.

## Logging or Checking History

### Basic Logging

```zsh
git log
```

### Beautiful Logging

```zsh
git log --oneline --graph --decorate
```

### Logging Date Wise

```zsh
git log --since="3 days ago"
```

### Following all the changes to one file-name

```zsh
git log --follow -- {name-of-the-file}
```

### Show all details of particular commit

```zsh
git show {commit-hash}
```

## Git Alias

### Generalised Command for Alias

```zsh
git config --global alias.{new-command-name} "{previous-command}"
```

### An Useful Example for Alias

```zsh
git config --global alias.history "log --all --graph --decorate --oneline"
```

## Git Ignore (inside a project)

```zsh
code .gitignore
```

### Accepted formats

```git
MyFile.ext
*.ext
my-folder/
```

## References

### Types of Referencing

| Type             | Meaning                                               | Example                                     |
| :--------------- | :---------------------------------------------------- | :------------------------------------------ |
| The Head         | It is a reference to the currently checked out commit | `HEAD^^`, `HEAD~12`, `HEAD^2`               |
| Branch Reference | It refers to last commit of that branch               | `master^^^`, `bugFix~1`, `feature-branch^1` |
| Commit Hash      | The SHA-1 code that is unique to each commit          | `3b49920effa455cbafe8f2d4344e27b9cda09671`  |

### Reference through commit hash

This method is used to uniquely identify a commit. 7 characters are enough for identification.

```zsh
3b49920effa455cbafe8f2d4344e27b9cda09671  # Full Commit Hash
3b49920  # Preferred length of Commit Hash
3b4  # Not preferred but may work to uniquely identify
```

### Referencing to number of commits behind

The total number of `^` specifies total number of commits behind.

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

### Nested

It may contain `~`, any number of `^` or a number

```zsh
HEAD~1^2        # 1 commit behind, 1 more commit behind and 2nd parent
bugfix^3~5^1    # 1 commit behind, 3rd parent, five commit behind, 1st parent
master~1^2~1    # 1 commit behind, 2nd parent, then one commit behind
```

## Difference (for difftool replace `diff` to `difftool`)

### To see differences b/w Modified (red) to Staged (green)

```zsh
git diff
```

### To see differences b/w Modified (red) to Commited

```zsh
git diff HEAD
```

### To see differences b/w Staged (green) to Commited

```zsh
git diff --staged HEAD
```

### To see differences in one file only

```zsh
git diff -- {file-name}
```

### To see difference b/w two commits

```zsh
git diff {new-commit-hash} {old-commit-hash}
```

### To see difference b/w two commits using p4merge

```zsh
git difftool {left-side-commit-hash} {right-side-commit-hash}
```

Note: `HEAD` points to the last commit, which can also be used.

### To see difference b/w `HEAD` and `HEAD - 1`

```zsh
git diff HEAD HEAD^
```

### To see difference b/w two Branches

```zsh
git diff {working-branch} {branch-with changes}
```

### To see difference between local and remote `master` branch

```zsh
git diff master origin/master
```

## Branching

### Check Local Branches

```zsh
git branch
```

### Check all Branches

```zsh
git branch -a
```

or

```zsh
git branch -all
```

Note: `*` represents current active branch.

### Create new branch

```zsh
git branch {new-branch-name}
```

### Switch branches

```zsh
git checkout {branch-name}
```

### Create new branch and checkout

```zsh
git checkout -b {new-branch-name}
```

### Rename branch

```zsh
git branch -m {old-name-of-branch} {new-name-of-branch}
```

Note: Here `-m` flag represents move.

### Move branch to a Particular Commit

```zsh
git branch -f {branch-name} {commit-hash}
```

Note: Use it with care because `-f` flag which means forcefully.

### Deleting the branch

```zsh
git branch -d {branch-name}
```

Note: You can't delete the branch you're currently checked out to.

## Merging

### Merge a branch in current branch

```zsh
git merge {branch-to-be-merged}
```

### Merge a branch in current branch with NO FAST FORWARD

```zsh
git merge {branch-to-be-merged} --no-ff
```

Note: This method preserves the Graphline and adds a new commit with message **Merge branch "branch-you-merged"**

### Merge a branch in current branch with a commit meesage

```zsh
git merge -m "{merge-branch-message}"
```

## Rebase

Rebase adds the commits from other branch on top of your current branch.

### Simple Rebase

```zsh
git rebase {branch-name}
```

Note: The changes will be added on top of the branch you are currently active.

A use case:

- You are working on a feature using branch `myfeature`.
- A new commit appeared on the `master` branch.
- You want to add this new commit to your `myfeature` branch.
- You want your branch commits on top of the `master` branch's new commit.

```zsh
git checkout myfeature
git rebase master
```

### Rebase branch below another branch

After rebasing you'll be checked out to the above branch.

```zsh
git rebase {branch-to-appear-below} {branch-to-appear-above}
```

### Stop a rebase

```zsh
git rebase --abort
```

### What to do when a conflict appears

```zsh
git rebase {conflicting-branch}
```

Then solve the rebase similar to the merge conflicts.

```zsh
git mergetool
```

After applying your desired changes.

```zsh
git add .
git rebase --continue
```

### Rebase when branches diverge

If you recieve a message after `git fetch`, that

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

## Stashing

- Stash saves the uncommited working state as a temporary commit.
- These changes can be recovered accordingly.

### Simple Stash

While on a branch which has uncommited changes.

```zsh
git stash
```

Note: Stash only stashes the tracked files.

### Stash untracked files also

```zsh
git stash -u
```

Note: `-u` is same as `--include-untracked`.

### Naming a Stash

You can save the stash with a stash message.

```zsh
git stash save "{stash-message}"
```

### List Temporary Stash Commits

```zsh
git stash list
```

### Return the Stashed Changes

```zsh
git stash apply {stash-code}
```

Then, the stashed changes will appear, ready to be further modified and commited.
Make new modifications if necessary and then commit.

### Remove Temporary Stash Commits

```zsh
git stash drop {stash-code}
```

### Remove all Temporary Stash Commits

```zsh
git stash clear
```

Note: In case of single stash we don't need to provide stash-code.

### Pop a stash commit

This command performs `apply` and `drop` in the same order.

```zsh
git stash pop
```

### Show changes of a Stash

Perform `git stash list` to see all the stashes.

```zsh
git stash show stash@{<num>}
```

Note: Curly brances are used in this command.

### Migrate the latest stash changes to the new branch

```zsh
git stash branch {new-branch-name}
```

## Tagging

Uses of Tags:

- Add identification to a commit.
- Mark major milestone like a version number. For Example, `v-1.0`
- Used as a Release on GitHub.

| Type        | Operation                     | Command                    |
| :---------- | :---------------------------- | :------------------------- |
| Lightweight | Adds tag name beside a commit | `git tag {name-of-tag}`    |
| Annotated   | It also contains a message    | `git tag -a {name-of-tag}` |

### Add a Lightweight Tag

```zsh
git tag {name-of-tag}
```

### Add a Lightweight Tag to a particular commit

```zsh
git tag {name-of-tag} {commit-hash}
```

### Add an Annotated Tag

```zsh
git tag -a {name-of-annotated-tag}
```

This opens a text editor to add a message.

### Add an Annotated Tag with message through command line

```zsh
git tag -a {name-of-annotated-tag} -m "{tag-message}"
```

### Add an Annotated Tag to a particular commit

```zsh
git tag -a {name-of-annotated-tag} {commit-hash}
```

### Migrate an Annotated Tag to a particular commit

```zsh
git tag -a {name-of-annotated-tag} -f {commit-hash}
```

### Show list of tags

```zsh
git tag --list
```

### Show the commit to which the `tag` points

```zsh
git show {name-of-tag}
```

### Delete a tag

```zsh
git tag --delete {name-of-tag}
```

### Pushing the Tag

```zsh
git push {remote} {name-of-tag}
```

Note: If an unpublished commit is included in the tag, then this command will also push the commit.

### Push all the tags

```zsh
git push {remote} {branch} --tags
```

Note: The command `git push {remote} {master}` won't automatically push the tags along with commits without `--tags` argument.

### Remove a tag from remote

```zsh
git push {remote} :{tag-name}
```

Note: After using this command the tag will be removed from remote but will remain in the local repository.

## Describe

This command describes where you are relative to the closest tag.

```zsh
git describe {ref}
```

Where `{ref}` is anything git can resolve into a commit.
If not specified, git just uses where you're checked out right now (HEAD).

### The output of the command looks like

```zsh
{tag}_{numCommits}_g{hash}
```

| Element    | Meaning                              |
| :--------- | :----------------------------------- |
| tag        | Closest ancestor tag in history      |
| numCommits | Number of commits away that tag is   |
| hash       | Hash of the commit used as reference |

## Reflog

- It provides the history of all the git operations that had been executed.
- The topmost row provides the latest git operation performed.
- It only saves the history of last 60 days.

## Reset

- It resets the changes to the selected commit.
- By default last commit is used as {ref}, if none provided.
- Both `HEAD` and branch reference moves to a different commit.

### Soft Reset

It simply makes all commited changes as uncommited and sometimes results into two branches:

- Main Branch has working directory same as before reset but as uncommited changes.
- The other unnamed branch contains all the commits that won't be the part anymore.

```zsh
git reset {ref}
```

For Example, to reset last commit use `git reset HEAD^`.

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

### Create a new branch from the newly checked out commit

```zsh
git switch -c {new-branch-name}
```

### In case everything is messed up due to `git checkout` and you want to return back to natural form

You can undo the operation using `git switch -` or update the branch reference using `git checkout {branch-name}`.

## Differences between `reset` and `checkout`

### `reset`

1. The `git reset {commit-hash}` moves the `HEAD` as well as the the branch reference to a particualar commit.
2. All the changes of commits on top of the current commit are reflected as the uncommited changes.
3. To revert it, we can find correct `HEAD` state and reset it using `git reset HEAD@{<num>}`.
4. It is used to go back in time, in case from some point the bug carried on.

### `checkout`

1. The `git checkout {commit-hash}` only moves the `HEAD` and thus results in a detached `HEAD` from the branch reference.
2. It doesn't results in uncommited changes.
3. To revert it we only need to update the `HEAD` to branch reference, we can use `git checkout {branch-name}`.
4. It is used to create a new branch.

## Revert

- It simply creates a **new commit** that is the opposite of an existing commit.
- It leaves the files in the same state as if the commit that has been reverted never existed.
- The new commits can be pushed to `remote` easily thus reverting commits over there also.
- In `reset` there isn't new commit, it simply resets the `HEAD` and branch reference to a particualar commmit.

### To revert last commit on branch

```zsh
git revert HEAD
```

### To revert a specific commit

```zsh
git revert {bad-commit-hash}
```

### To revert multiple commits also HEAD

```zsh
git commit {bad-commit-hash1} {bad-commit-hash2}...{bad-commit-hash-n}
```

Note: Every bad commit will have a new revert commit and its message.

### To revert a commit and provide message in command line

```zsh
git revert --no-commit {bad-commit-hash}
git commit -m "{message}"
```

## Cherry-pick

- It is used to merge any number of commits in th working tree into the current branch,
- It is similar to picking commits like a cherry from the table.

### Cherry-pick only one Commit

```zsh
git cherry-pick {commit-hash}
```

### Cherry-pick multiple Commits

```zsh
git cherry-pick {commit-hash-1} {commit-hash-1} ...{commit-hash-3}
```
