## How do I update a GitHub forked repository?
https://stackoverflow.com/a/7244456/747748


#### Change remote host [(more info)](https://help.github.com/articles/changing-a-remote-s-url/)
* First list remotes `git remote -v`
* Then use `git remote set-url`  which takes two arguments (Remote name and New URL)
* for example:
`git remote set-url origin https://github.com/USERNAME/REPOSITORY.git`