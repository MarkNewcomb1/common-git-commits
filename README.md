# common-git-commits
A breakdown of the different version control situations at work and git commits for them
> I need to rebase my branch from our team's develop branch again. What's the order again for doing this?


1. Switch to the develop branch and pull the latest changes.

`git checkout develop`
`git pull`

2. Switch back to your branch.

`git checkout bug/STUFF-000/input_field_not_saving`

3. Rebase your branch from develop.

`git rebase develop`

4. Now you need to force push your changes.  

`git push --force`

####Why force push? 
Because when you perform git rebase, new commits are created and the remote branch can't be fast-forwarded to your local. So `--force` ignores the state of the remote branch and sets it to the commit you're pushing to it. So `--force` overrides remote to be like your local.
####So what does rebase do?
**rebase** takes changes from one branch into another. If you've made your branch off of develop some time ago, chances are your branch is out-of-date with develop. You want your changes to be integrated into the latest version of the team's develop branch. 
