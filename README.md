UCSB ARIT • Git Cheat Sheet
===================


    ---------------------------------------------------------------------------------------------------------------
                                                  |
      Topic                                       |  Basic instructions (otherwise see gitref.org || git-scm.com/docs)
                                                  |
    ----------------------------------------------+----------------------------------------------------------------
                                                  |
    Configuration                                 |  git config --global                            < to make changes to 'core.excludes' file and more
                                                  |
    Password caching using SSH                    |  git config --global credential.helper wincred  < needs more documentation
                                                  |
    ----------------------------------------------+----------------------------------------------------------------
                                                  |
    Start a new git repository from shell         |  Use: git init                                  < manually creates the .git folder and its contents
                                                  |       git add .                                 < add all files to the stage
                                                  |       git commit -m  "first commit"
                                                  |   or: git commit -am "first commit"             < add files to stage and commit in one step
                                                  |
    Create a README.md file                       |  Use: touch README.md                           < 'touch' command creates a file if it doesn't exist already
                                                  |                                                   'touch' command also modifies file dates if it does exist
                                                  |
    ----------------------------------------------+----------------------------------------------------------------
                                                  |
    Add a remote for the repository               |  Use: git remote add                            < [-t <branch>] [-m <master>] [-f] [--[no-]tags] [--mirror=<fetch|push>] <name> <url>
                                                  |
    Get list of remote paths ("remotes")          |  Use: git remote -v                             < -v = verbose; shows where push/fetch goes to/comes from
                                                  |
    Pull down a full repository                   |  Use: git clone <repo> <directory>              < 'repo' is remote repo; 'directory' is local directory
                                                  |                                                 < note that 'git pull' = 'git fetch' + 'git merge'
                                                  |
    ----------------------------------------------+----------------------------------------------------------------
                                                  |
    Branch > Get a list of branches >             |  Use: git branch
                                                  |
    Branch > Create a local branch >              |  Use: git branch <branch name>                  < cannot have pending commits to do this
                                                  |
    Branch > Create a remote branch >             |  Use: git push -u origin <branch name>          < 1) create   local branch    : git branch dev
                                                  |                                                   2) checkout local branch    : git checkout dev
                                                  |                                                   3) push branch up to remote : git push -u origin dev (-u = set upstream)
                                                  |
    Branch > Rename a branch >                    |  Use: git branch -m <oldname> <newname>
                                                  |       git branch -m <newname>                   < if renaming current branch
                                                  |
                                                  |
    Branch > Delete a branch >                    |  Use: git push origin --delete <branch name>    < only deletes remote branch
                                                  |       git branch -d <branch name>               < warns if commits not in other branches/won't delete
                                                  |       git branch -D <branch name>               < no warning - hard delete
                                                  |
    Branch > Delete a remote branch >             |       git push origin :<branch name>            < note the space before the colon means push nothing to remote branch
                                                  |                                                   effectively deleting the remote branch
                                                  |       or
                                                  |       git push origin --delete <branch name>    
                                                  |
                                                  |
    Branch > Pull down a remote branch 1st time > |  Use: git checkout    <remote branch name> --track origin/<remote branch name>
                                                  |
                                                  |       git checkout -b <remote branch name> --track origin/<remote branch name>
                                                  |
                                                  |                    -b parameter means create the branch if is doesn't already exist
                                                  |
                                                  |
    Branch > Switch to another branch >           |  Use: git checkout <branch name>                < if you have uncommitted work, you will need to 'stash' those files. 
                                                  |                                                   See section on 'Stash'
                                                  |
                                                  |
    Branch > compare local to remote >            |  Use: git status
                                                  |       git status -s                             < to make the output short and easy to read
                                                  |
                                                  |
    Branch > update local cached copy of remote   |  Use: git fetch                                 < used by git for comparison
                                                  |
                                                  |
    Branch > push local to remote >               |  Use: git push {origin is implied... to be pushed to remote}
                                                  |
    Branch > merge one into another >             |    An example is best - to merge dev into master:
                                                  |                    1) switch to master   Use: git checkout master
                                                  |                    2) then               Use: git merge dev
                                                  |                    2) or                 Use: git merge --no-ff dev   < ff = "fast forward"
                                                  |
                                                  |   --no-ff avoids losing information about the historical existence of a branch - see below
                                                  |
                                                  |   --ff-only merges only if a ff merge can be done
                                                  |
                                                  |
                                                  |       with fast forward:          x---x---x----x----x---x---x---x
                                                  |
                                                  |    without fast forward (no-ff):  x---x---x         x---x---x---x
                                                  |                                            \x--x--x/
                                                  |
                                                  |
    Branch > See the last commit on each branch   |  Use: git branch -v                             < shows one line for each branch with last commit and message
                                                  |
                                                  |
    Branch > Which branches are already merged?   |  Use: git branch --merged                       < branches merged into the branch you’re on (contain every commit in common)
                                                  |
                                                  |
    ----------------------------------------------+----------------------------------------------------------------
                                                  |
    Commit > Commit some work                     |  Use: git commit -m "First commit"
                                                  |
    Commit > Add tags                             |  Use: git tag -a <the tag> -m <message>
                                                  |
    Commit > Add tag to older commit              |  e.g. git tag -a v1.2 9fceb02 -m "Message"      < Where 9fceb02 is the beginning part of the commit id hash
                                                  |       git push --tags origin master             < You can then push them up using this 
                                                  |
    Commit > Diffs                                |  Use: git diff                                  < to see the changes in your working folder vs the last commit
                                                  |  Use: git diff <some hash>                      < to see the difference between that commit and the current state
                                                  |  Use: git diff <hash1> <hash2>                  < to compare between two different commits
                                                  |  Use: git diff --cached syn: git diff --staged  < to compare staged to last commit
                                                  |
                                                  |  Use: git diff --stat --summary                 < good options to view summary of diff
                                                  |       git log -p                                < summarize what has happened on a branch or between commits 
                                                  |                                                   or between local and remote branches
                                                  |
    Commit > Get a list of commits                |  Use: git log
                                                  |       git log -n 10                             < for 10 latest commits
                                                  |       git log --since "2014-08-15"              < commits since a date
                                                  |       git log --since 1.month                   < commits in last month
                                                  |       git log --until "2014-08-15"              < commits back until a date
                                                  |       git log --author "UCSBMike"               < commits by an author
                                                  |
                                                  |
    Commit > See changes from any commit >        |  Use: git show <some hash>                           < will show add/del for line items in files
                                                  |
    Commit > Find commit with criteria            |    Command below will show commits that mention "v2.0" and written by "UCSBMike":
                                                  |       git log --all-match --grep=v2.0 --author=UCSBMike
                                                  |
                                                  |
    Commit > Suggested cmd for helpful view       |  Alias this: git log --oneline --graph --source --all --decorate
                                                  |
                                                  |              git config --global  alias.lg 'log --oneline --graph --source --all --decorate'
                                                  |           or git config --global --edit
                                                  |           or git config --global --list
                                                  |
                                                  |
    Commit > Show                                 |  Use: git show                                  < shows various types of objects: For commits it shows the log message and textual diff. It also 
                                                  |                                                                                     presents the merge commit in a special format as produced by git diff-tree --cc.
                                                  |                                                                                   For tags, it shows the tag message and the referenced objects.
                                                  |                                                                                   For trees, it shows the names.
                                                  |                                                                                   For plain blobs, it shows the plain contents.
                                                  |                                                                                   
                                                  |
    ----------------------------------------------+----------------------------------------------------------------
                                                  |
                                                  |
    Stashing uncommited files >                   |  Use: git stash save "..."                      < no "-m" option needed for message/note
                                                  |
    Stash > To see which stashes you’ve stored >  |  Use: git stash list
                                                  |
    Stash > Get list of stash contents >          |  Use: git stash show stash@{##}                 < where ## is the stash index you want to see
                                                  |
    Stash > Remove a stash >                      |  Use: git stash drop stash@{##}                 < where ## is the stash index you want to remove
                                                  |
    Stash > Remove all stashes >                  |  Use: git stash clear                           < where ## is the stash index you want to remove
                                                  |
    Stash > Apply a stash >                       |  Use: git stash apply stash@{##}                < where ## is the stash index you want to apply
                                                  |    or git stash pop   stash@{##}                < where ## is the stash index you want to apply
                                                  |                                                   note that 'pop' then immediately drops it from your stack
                                                  |
    Stash > Unapply a stash >                     |  Use: git stash show -p stash@{##}              < the -p option applies a stash in reverse
                                                  |    or git apply -R                              < does the same thing
                                                  |
    Stash > Create a branch from a stach >        |  Use: git stash branch <branch name>            < checks out the commit you were on when the stash was created
                                                  |
    ----------------------------------------------+----------------------------------------------------------------
                                                  |
                                                  |
    HEAD > Reset                                  |  Use: git reset                                 < Reset current HEAD to the specified state; is like rewinding a tape recorder to a point
                                                  |                                                   to begin recording again
                                                  |       --soft                                    < keeps changes and staged files
                                                  |       --mixed                                   < keeps changes and unstaged files
                                                  |       --hard                                    < removes changes and working directory clean
                                                  |
                                                  |  HEAD: pointer to tip of current branch
                                                  |        i.e. what was last checked out
                                                  |        i.e. where it starts tracking changes from last time
                                                  |
    Undo and Rollback                             |
                                                  |  Use: git checkout <filename>                   < to undo the changes of a single file. This will pull latest version of the
                                                  |                                                   file and overwrite any changes to this file in your working area.
                                                  |
                                                  |  Use: git checkout <SHA>                        < to rollback to a commit referred to by the hash
                                                  |
                                                  |  Use: git reset <tag/branch/commit/file>        < to roll back to a previous commit . This will erase all the commits up to
                                                  |                                                   that point as they never happened. Any file changes will not be lost, but will
                                                  |                                                   be left in the working directory as modified. Using the "--hard" switch will also
                                                  |                                                   delete all the changed files and reset them as well.
                                                  |
                                                  |  Use: git commit --amend -m                     < allows changes to *last* commit
                                                  |
    Revert is a way to go back to a previous      |  Use: git revert <tag/branch/commit>            < makes a new commit, basically moving the files from that commit up to the top
    commit, but to keep the history of commits    |
    between that commit and now.                  |
                                                  |  Use: git revert <SHA>                          < to rollback to a commit referred to by the hash
                                                  |
                                                  |
    Remove <file> from version control            |  Use: git rm --cached <file>                    < remove <file> from version control, while keeping it in the working repository
                                                  |  Use: git rm <file>                             < remove <file> from version control and file system; adds to stage in one step
                                                  |
    Remove untracked files                        |  Use:git-clean                                  < Remove untracked files from the working tree
                                                  |
    Keep an empty directory                       |  Create a .gitkeep file (empty file) so that Git can track the directory
                                                  |
    ----------------------------------------------+----------------------------------------------------------------
    
    
    Fork A Repo (https://help.github.com/articles/fork-a-repo/)
    
    A fork is a copy of a repository. Forking a repository allows you to freely experiment with changes without 
    affecting the original project.
    
    Most commonly, forks are used to either propose changes to someone else's project or to use someone else's 
    project as a starting point for your own idea.
    
    Propose changes to someone else's project
    
    A great example of using forks to propose changes is for bug fixes. Rather than logging an issue for a 
    bug you've found, you can:
    
    Fork the repository.
    Make the fix.
    Submit a pull request to the project owner.
    If the project owner likes your work, they might pull your fix into the original repository!
    
    Use someone else's project as a starting point for your own idea.
    
    At the heart of open source is the idea that by sharing code, we can make better, more reliable software. 
    In fact, when you create a repository on GitHub, you have a choice of automatically including a license 
    file, which determines how you want your project to be shared with others.
    
    ---------------------------------------------------------------------------------------------------------------
    
    What's the difference between git reflog and log?
    
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
