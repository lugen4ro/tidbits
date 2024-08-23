# Git

Sections

- Creating snapshots
- Browsing history
- Branching & merging
- Collaboration using Git & GitHub
- Rewriting history

## Commit messages

https://gist.github.com/joshbuchea/6f47e86d2510bce28f8e7f42ae84c716

feat: (new feature for the user, not a new feature for build script)
fix: (bug fix for the user, not a fix to a build script)
docs: (changes to the documentation)
style: (formatting, missing semi colons, etc; no production code change)
refactor: (refactoring production code, eg. renaming a variable)
test: (adding missing tests, refactoring tests; no production code change)
chore: (updating grunt tasks etc; no production code change)

## Basics

Git objects are

- Commits
- Blobs (Files)
- Trees (Directories)
- Tags

Available commands

- git reset: Reset HEAD to the specified state (specify working dir / staging area state with soft,mixed,hard)
- git revert: Add a new commit(s) that revert previous commit(s)
- git reflog: Show history of a pointer (Chagnes made by checkout, reset, commit, merge...)
- git clean: Remove untracked files

## Other

```bash
git config --list # show git configuration

git update-index --assume-unchanged path/to/file # Ignore changes to a specific file
git update-index --no-assume-unchanged path/to/file # Start tracking again
git ls-files -v | grep "^[a-z]" # Show all assume-unchanged files

# Delete untracked files & directories
git clean -fd
```

## Creating Snapshots

### Initializing a repository

```bash
git init
```

### Staging files

```bash
git add file1.js # Stages a single file
git add file1.js file2.js # Stages multiple files
git add \*.js # Stages with a pattern
git add . # Stages the current directory and all its content
```

### Viewing the status

```bash
git status # Full status
git status -s # Short status
```

### Committing the staged files

```bash
git commit -m “Message” # Commits with a one-line message
git commit # Opens the default editor to type a long message
```

### Skipping the staging area

```bash
git commit -am “Message”
```

### Removing files

```bash
git rm file1.js # Removes from working directory and staging area
git rm --cached file1.js # Removes from staging area only
```

### Renaming or moving files

```bash
git mv file1.js file1.txt
```

### Viewing the staged/unstaged changes

```bash
git diff # Shows unstaged changes
git diff --staged # Shows staged changes
git diff --cached # Same as the above
```

### Viewing the history

```bash
git log # Full history
git log --oneline # Summary
git log --reverse # Lists the commits from the oldest to the newest
```

### Viewing a commit

```bash
git show 921a2ff # Shows the given commit
git show HEAD # Shows the last commit
git show HEAD~2 # Two steps before the last commit
git show HEAD:file.js # Shows the version of file.js stored in the last commit
git ls-tree HEAD # Shows all files in commit pointed to by HEAD
```

### Unstaging files (undoing git add)

```bash
git restore --staged file.js # Copies the last version of file.js from repo to index
```

### Discarding local changes

```bash
git restore file.js # Copies file.js from index to working directory
git restore file1.js file2.js # Restores multiple files in working directory
git restore . # Discards all local changes (except untracked files)
git clean -fd # Removes all untracked files
```

### Restoring an earlier version of a file

```bash
git restore --source=HEAD~2 file.js
```

## Browsing History

### Viewing the history

```bash
git log --stat # Shows the list of modified files
git log --patch # Shows the actual changes (patches)
```

### Filtering the history

```bash
git log -3 # Shows the last 3 entries
git log --author=“Mosh”
git log --before=“2020-08-17”
git log --after=“one week ago”
git log --grep=“GUI” # Commits with “GUI” in their message
git log -S“GUI” # Commits with “GUI” in their patches
git log hash1..hash2 # Range of commits
git log file.txt # Commits that touched file.txt
```

### Formatting the log output

```bash
git log --pretty=format:”%an committed %H”
```

### Creating an alias

```bash
git config --global alias.lg “log --oneline"
```

### Viewing a commit

```bash
git show HEAD~2
git show HEAD~2:file1.txt # Shows the version of file stored in this commit
```

### Comparing commits

```bash
git diff HEAD~2 HEAD # Shows the changes between two commits
git diff HEAD~2 HEAD file.txt # Changes to file.txt only
```

### Checking out a commit

```bash
git checkout dad47ed # Checks out the given commit
git checkout master # Checks out the master branch
```

### Finding a bad commit

```bash
git bisect start
git bisect bad # Marks the current commit as a bad commit
git bisect good ca49180 # Marks the given commit as a good commit
git bisect reset # Terminates the bisect session
```

### Finding contributors

```bash
git shortlog
```

### Viewing the history of a file

```bash
git log file.txt # Shows the commits that touched file.txt
git log --stat file.txt # Shows statistics (the number of changes) for file.txt
git log --patch file.txt # Shows the patches (changes) applied to file.txt
```

### Finding the author of lines

```bash
git blame file.txt # Shows the author of each line in file.txt
```

### Tagging

```bash
git tag v1.0 # Tags the last commit as v1.0
git tag v1.0 5e7a828 # Tags an earlier commit
git tag # Lists all the tags
git tag -d v1.0 # Deletes the given tag
git tag -a v1.1 -m "Can annotate the tag with this message"
git tag -n # Show tags and their messages (By default it's the commit message)
```

## Branching & Merging

### Managing branches

```bash
git branch bugfix # Creates a new branch called bugfix
git checkout bugfix # Switches to the bugfix branch
git switch bugfix # Same as the above
git switch -C bugfix # Creates and switches
git branch -d bugfix # Deletes the bugfix branch
git branch -m bugfix bufix/signup-form # Rename branch
```

### Comparing branches

```bash
git log master..bugfix # Lists the commits in the bugfix branch not in master
git diff master..bugfix # Shows the summary of changes
```

### Stashing

When switching branches while having modification, to keep those modifications, we stash and store them temporarily.

```bash
git stash push -m “New tax rules” # Creates a new stash
git stash --all # Stash all files including new untracked files
git stash list # Lists all the stashes
git stash show stash@{1} # Shows the given stash
git stash show 1 # shortcut for stash@{1}
git stash apply 1 # Applies the given stash to the working dir
git stash drop 1 # Deletes the given stash
git stash clear # Deletes all the stashes
git stash pop # Apply newest stash and delete it
```

### Merging

Two types of merges

- Fast-forward merge (if branches have not diverged, one parent merge)
- 3-way merge (if branches have diverged, two parent merge)

```bash
git merge bugfix # Merges the bugfix branch into the current branch
git merge --no-ff bugfix # Creates a merge commit even if FF is possible

git config ff no # Disables ff merges for current repository
git config --global ff no # Disables ff merges for all repositories
git config --global merge.tool p4merge # Settings the global merge tool
git config --global mergetool.p4merge.path "C:\...." # Setting the path to the tool
git config --global mergetool.keepBackup false # disables mergetool from creating backupfile by default

git merge --squash bugfix # Performs a squash merge (combines multiple changes into one linear commit)
git merge --abort # Aborts the merge
```

### Undoing a merge commit

```bash
git revert -m 1 HEAD # Adds a new commit undoing the changes. Reverts to parent 1 which is the one on the same branch
```

### Viewing the merged branches

```bash
git branch --merged # Shows the merged branches
git branch --no-merged # Shows the unmerged branches
```

### Rebasing

Only use with fully local branches.
Change the base of the branch to make a fast-forward merge

```bash
git rebase master # Changes the base of the current branch
```

### Cherry picking

Cherry pick commits from the branch to merge to master

```bash
git cherry-pick dad47ed # Applies the given commit on the current branch
```

## Collaboration

### Cloning a repository

```bash
git clone url
```

### Syncing with remotes

```bash
git fetch origin master # Fetches master from origin
git fetch origin # Fetches all objects from origin
git fetch # Shortcut for “git fetch origin”
git pull # Fetch + merge
git pull --rebase # Ends up with linear history
git push origin master # Pushes master to origin
git push # Shortcut for “git push origin master”
```

### Sharing tags

```bash
git push origin v1.0 # Pushes tag v1.0 to origin
git push origin —delete v1.0
```

### Sharing branches

```bash
git branch -r # Shows remote tracking branches
git branch -vv # Shows local & remote tracking branches
git push -u origin bugfix # Pushes bugfix to origin
git push -d origin bugfix # Removes bugfix from origin
```

### Managing remotes

```bash
git remote -v # Shows remote repos
git remote add upstream <url_here> # Adds a new remote called upstream (this name is often used for the base repository when forking)
git remote rename upstream base # Renaming a remote repository
git remote rm upstream # Removes upstream
git remote prune origin # Prunes remote repositories (Not automatically deleted with pull?)
```

## Rewriting History

### Undoing commits

```bash
git reset HEAD~ # ~ is same as ~1 (default mode is mixed)
git reset --soft HEAD^ # Removes the last commit, keeps changes staged
git reset --mixed HEAD^ # Unstages the changes as well
git reset --hard HEAD^ # Discards local changes
```

### Reverting commits

```bash
git revert 72856ea # Reverts the given commit
git revert HEAD~3.. # Reverts the last three commits (HEAD~3 itself will not be reverted)
git revert --no-commit HEAD~3.. # Revert without commit first, then use --continue to add in one commit
```

### Recovering lost commits

Show history of a pointer

```bash
git reflog # Shows the history of HEAD
git reflog show bugfix # Shows the history of bugfix pointer
```

### Amending the last commit

```bash
git commit --amend
```

### Interactive rebasing

Specify Commit before the first one we want to change.

```bash
git rebase -i HEAD~5
```
