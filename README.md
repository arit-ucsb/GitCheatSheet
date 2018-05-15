# UCSB ARIT • Git Cheat Sheet
For more in-depth instructions see [gitref.org](https://services.github.com/) or [git-scm.com/docs](https://git-scm.com/docs)

### Configuration   
* To make changes to 'core.excludes' file and more:
  `git config --global`
* Password caching using SSH:
`git config --global credential.helper wincred`

### Start a new git repository from shell
* `git init` manually creates the .git folder and its contents
* `git add .` adds all files to the stage
* `git commit -m  "first commit"` **or** `git commit -am "first commit"` adds files to stage and commit in one step
* `touch README.md` creates a README.md file (the 'touch' command creates a file if it doesn't exist already and modifies file dates if it does exist)

### Remote repository  [(more info)](https://help.github.com/articles/adding-a-remote/)
* `git remote add` adds a remote for the repository
 * `[-t <branch>] [-m <master>] [-f] [--[no-]tags] [--mirror=<fetch|push>] <name> <url>`
* Get list of remote paths ("remotes"):
`git remote -v`  (The v for verbose shows where push/fetch goes to/comes from)

* Pull down a full repository
`git clone <repo> <directory>` ('repo' is remote repo; 'directory' is local directory; note that 'git pull' = 'git fetch' + 'git merge')

###### Move a repository from one web host to another
This method should maintain your commit history but that will depend on your chosen host
* `cd \local\repository\directory`
* show current remote and the name of the remotes:  `git remote -v`
* take the name of the remote you'd like to remove (e.g. origin or destination) and plug it into the next command
* `git remote rm origin`  will remove a remote named 'origin' but substitute your remote name here
* Push the repo to new host:
 * `git remote add origin https://user@example.com/userName/remoteRepoName.git`
 * `git push -u origin master`



### Branch
* Get a list of branches:
`git branch`
* Create a local branch:
`git branch <branch name>` (cannot have pending commits to do this)
* Create a remote branch:
  1. create  local branch: `git branch dev`
  2. checkout local branch: `git checkout dev`
  3. push branch up to remote: `git push -u origin dev` (-u = set upstream)
* Rename a branch:
`git branch -m <oldname> <newname>`
* Rename the current branch:
`git branch -m <newname>`
* Delete a branch:
  * Only deletes remote branch:
  `git push origin --delete <branch name>`
  * warns if commits not in other branches/won't delete:
  `git branch -d <branch name>`
  * no warning - hard delete:
  `git branch -D <branch name>`
  * Delete a remote branch:
  `git push origin :<branch name>` (note the space before the colon means push nothing to remote branch effectively deleting the remote branch)
  **OR** `git push origin --delete <branch name>`
* Pull down a remote branch 1st time:
`git checkout -b <remote branch name> --track origin/<remote branch name>` (-b parameter means create the branch if is doesn't already exist)
* Switch to another branch (if you have uncommitted work, you will need to 'stash' those files):
`git checkout <branch name>`
* compare local to remote:
`git status` (add `-s` to make the output short and easy to read )
* update local cached copy of remote (used by git for comparison):
`git fetch`
* push local to remote:
`git push {origin is implied... to be pushed to remote}`
* merge one into another (e.g. to merge dev into master ):
  1. switch to master: `git checkout master`
  2. then: `git merge dev`
  3. or: `git merge --no-ff dev` (ff = fast forward)
    * `--no-ff` avoids losing information about the historical existence of a branch
    * `--ff-only`  merges only if a ff merge can be done
    * with fast forward:          `x---x---x----x----x---x---x---x`
    * without fast forward (no-ff):  `---x---x---x----\___.____.___.__/----x-----x---`
* See the last commit on each branch: `git branch -v` (shows one line for each branch with last commit and message)
* Which branches are already merged?:
`git branch --merged` <-  branches merged into the branch you’re on (contain every commit in common)

### Commits
###### Make Some
* Commit some work
`git commit -m "First commit"`

###### Tags
* Add tags
`git tag -a <the tag> -m <message>`
* Add tag to older commit
  * e.g. `git tag -a v1.2 9fceb02 -m "Message"` Where 9fceb02 is the beginning part of the commit id hash
  * You can then push them up using this: `git push --tags origin master`

###### Diffs
 * to see the changes in your working folder vs the last commit: `git diff`
 * to see the difference between that commit and the current state: `git diff <some hash>`
 * to compare between two different commits: `git diff <hash1> <hash2>`
 * to compare staged to last commit: `git diff --cached syn: git diff --staged`
 * good options to view summary of diff: `git diff --stat --summary`

###### Logs
 * summarize what has happened on a branch or between commits or between local and remote branches: `git log -p `
* Get a list of commits: `git log`
 * for 10 latest commits: `git log -n 10`
 * commits since a date: `git log --since "2014-08-15"`
 * commits in last month: `git log --since 1.month`
 * commits back until a date: `git log --until "2014-08-15"`
 * commits by an author: `git log --author "UCSBMike"`
 * groups commits by user, showing just the subject line: `git shortlog`

###### Show
* See changes from any commit (will show add/del for line items in files):
`git show <some hash>`
* Show: `git show` shows various types of objects: For commits it shows the log message and textual diff. It also presents the merge commit in a special format as produced by `git diff-tree --cc`. For tags, it shows the tag message and the referenced objects. For trees, it shows the names. For plain blobs, it shows the plain contents.

###### Find
* Find commit with criteria:
 * e.g. show commits that mention "v2.0" and written by "UCSBMike": `git log --all-match --grep=v2.0 --author=UCSBMike`

### Suggested commands for helpful views
 * Alias this:
  * `git log --oneline --graph --source --all --decorate`
  * `git config --global  alias.lg 'log --oneline --graph --source --all --decorate'`
  * or `git config --global --edit`
  * or `git config --global --list`

### Stashing uncommited files:
* `git stash save "..."` no "-m" option needed for message/note
* To see which stashes you’ve stored: `git stash list`
* Get list of stash contents: `git stash show stash@{##}` where ## is the stash index you want to see
* Remove a stash: `git stash drop stash@{##}` where ## is the stash index you want to remove
* Remove all stashes: git stash clear
* Apply a stash:
 * `git stash apply stash@{##}` where ## is the stash index you want to apply
 * **OR** `git stash pop   stash@{##}` where ## is the stash index you want to apply
* Un-apply a stash:
 * `git stash show -p stash@{##}` the `-p` option applies a stash in reverse
 * **OR** `git apply -R` does the same thing
* Create a branch from a stash: `git stash branch <branch name>` checks out the commit you were on when the stash was created

### HEAD Reset
* *HEAD* of a repository:
 * A pointer to tip of current branch.
 * i.e. what was last checked out.
 * i.e. where it starts tracking changes from last time
* `git reset` Reset current HEAD to the specified state; is like rewinding a tape recorder to a point to begin recording again
 * `--soft` keeps changes and staged files
 * `--mixed` keeps changes and unstaged files
 * `--hard` removes changes and working directory clean

### Undo and Rollback
 * `git checkout <filename>`
 * `git checkout <SHA>`
 * `git reset <tag/branch/commit/file>`
 * To rollback a commit pushed to remote `Git reset –hard <sha>` **THEN** `Git push –f origin master (note: -f means ‘force’)`
 * To abort a MERGE: `git merge --abort` **OR** `git reset --merge`
 * `git commit --amend -m`

### Revert
*Revert* is a way to go back to a previous commit, but to keep the history of commits between that commit and now.
* `git revert <tag/branch/commit>`
* `git revert <SHA>`

### Organize
###### Remove <file> from version control
 * `git rm --cached <file>` remove <file> from version control, while keeping it in the working repository
 * `git rm <file>` remove <file> from version control and file system; adds to stage in one step

###### Remove untracked files:
 * `git-clean` Remove untracked files from the working tree

###### Keep an empty directory:
 * Create a .gitkeep file (empty file) so that Git can track the directory

### Fork A Repo  [more info](https://help.github.com/articles/fork-a-repo/)
A fork is a copy of a repository. Forking a repository allows you to freely experiment with changes without affecting the original project.

Most commonly, forks are used to either propose changes to someone else's project or to use someone else's project as a starting point for your own idea.

### Propose changes to someone else's project

A great example of using forks to propose changes is for bug fixes. Rather than logging an issue for a
bug you've found, you can:
* Fork the repository.
* Make the fix.
* Submit a pull request to the project owner.

If the project owner likes your work, they might pull your fix into the original repository!

### Use someone else's project as a starting point for your own idea.

At the heart of open source is the idea that by sharing code, we can make better, more reliable software.
In fact, when you create a repository on GitHub, you have a choice of automatically including a license
file, which determines how you want your project to be shared with others.

### What's the difference between git reflog and log?

FROM: http://stackoverflow.com/questions/17857723/whats-the-difference-between-git-reflog-and-log

git log shows the current HEAD and its ancestry. That is, it prints the commit HEAD points to, then its parent, its parent, and so on.
It traverses back through the repo's ancestry, by recursively looking up each commit's parent.

git reflog doesn't traverse HEAD's ancestry at all. The reflog is an ordered list of the commits that HEAD has pointed to: it's undo
history for your repo. The reflog isn't part of the repo itself (it's stored separately to the commits themselves) and isn't included
in pushes, fetches or clones; it's purely local.

Aside: understanding the reflog means you can't really lose data from your repo once it's been committed. If you accidentally reset to
an older commit, or rebase wrongly, or any other operation that visually "removes" commits, you can use the reflog to see where you were
before and git reset --hard back to that ref to restore your previous state. Remember, refs imply not just the commit but the entire
history behind it.
