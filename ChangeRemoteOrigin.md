### Move a repository from one web host to another
* `cd \local\repository\directory`
* show current remote and the name of the remotes:  `git remote -v`
* take the name of the remote you'd like to remove (e.g. origin or destination) and plug it into the next command
* `git remote rm origin`  will remove a remote named 'origin' but substitute your remote name here
 
### Push the repo to new host:
* `git remote add origin https://user@example.com/userName/remoteRepoName.git`
* `git push -u origin master`

This method should maintain your commit history but that will depend on your chosen host