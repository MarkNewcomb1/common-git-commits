# Common git commits
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


> I need to squash all of my multiple commits into one commit, with the commit message being the ticket number.

Assuming you've pushed all the commits to remote (do that if you haven't), 
look at the remote branch on github.com to see how many commits you've pushed. _make note of this number; this number is important_.


Let's suppose it's 3 commits. On your branch:

`git rebase -i HEAD~3` 

This will bring you a list of commits in VIM:

```
   1 pick 01mn9h78 The oldest commit
   2 pick a2b6pcfr A commit before the latest
   3 pick 093479uf The latest commit
```

You need to pick the oldest commit, and squash all the newer ones. 
So type `i` to enter insert mode and replace the word pick with `squash` on all the newer ones.

In this case, it should look like:
```
   1 pick 01mn9h78 The oldest commit
   2 s a2b6pcfr A commit before the latest
   3 s 093479uf The latest commit
```

Once you're done doing that, hit Escape to exit insert mode, and save this file by typing `:wq!`. That's a forceful save and exit vim command, by the way.

You should see a new VIM screen, like this:

```
# This is a combination of 3 commits.
# This is the 1st commit message:

The oldest commit

# This is the commit message #2:

A commit before the latest

# This is the commit message #3:

The latest commit

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.

```

Again, type `i` to enter Insert mode, but now add a `#` in front of the commits you don't want to keep. Escape, `:wq!` again.

Your interactive rebase is done - now just push this change:

`git push --force`


####Why `git rebase -i HEAD~3` ?

Remember that git rebase re-applies commits, one by one, in order. the `-i` option opens an editor with the commits that are about to be changed, with an option to edit the list. Here, we're editing the list by squashing them, and also we have an opportunity to re-word a particular commit message. 
And of course, `HEAD` is a reference to the last commit in the currently checked out branch. `~` points to a position relative to a specific commit. In this case, 3 back from the current one. 


> I just need to add something to my last commit, but I don't want to create another commit and have to squash it.

`git commit --amend --no-edit`

###Why?

`--amend` replaces the tip of the current branch by creating a new commit. The original commit is used as a starting point, and `--no-edit` uses the selected commit message without launching the vim editor. 
