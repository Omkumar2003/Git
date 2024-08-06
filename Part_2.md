# CH-1 (FORK)
- https://youtu.be/GfdhCaLy8rM
- A fork is a copy of a repository. Forking a repository allows you to freely experiment with changes without affecting the original project.

## Why Fork?

Forking is not a Git operation, but it is a feature offered by many Git hosting services such as GitHub, GitLab, and Bitbucket.

Those services "fork" a repo by creating a new copy of the repo and associating it as a "fork" of the original. It's quite literally just a copy that is linked to the original via some metadata.


## PRs From a Fork
When you fork someone's repository on a platform like GitHub, you get a copy of the repository in your account. This is the standard way to contribute to someone else's open-source project. The steps are typically:
```
Fork their repo into your account
Clone your fork to your local machine
Create a new branch (let's call it your_feature)
Make changes
Commit and push changes to your fork's remote your_feature branch
Create a pull request to original_owner/repo main from your_username/repo your_feature
```
Then the original owner can review your changes. If they like them, they can merge the changes straight from your fork into their repository.

- https://youtu.be/rxh6MhK6Tbs

# CH- 2 (REFLOG)

basically log sirf commit ki jankari deta hai , agar commit humne delete kr diya to ```git log ``` humein sirf , jo bachee hue commit hein unki jankari dega , but humein jo commit delete hogye wo kese pta chlenge
hum ```git reflog ``` se jahan jahan head rha hai uski jaankari lelenge .

The git reflog command (pronounced ref-log, not re-flog) is kinda like git log but stands for "reference log" and specifically logs the changes to a "reference" that have happened over time.

Reflog uses a different format to show the history of a branch or HEAD: one that's more concerned with the number of steps back in time

## Merge
Using Git internals is exceptionally inconvenient. We had to copy/paste and use the cat-file command 3 times!

I would not recommend doing it that way, but I wanted to drive home the point that you can always drop down to the plumbing commands if needed.

Luckily, there is a better way. The git merge command actually takes a "commitish" as an argument:
```
git merge <commitish>
```
A "commitish" is something that looks like a commit (branch, tag, commit, HEAD@{1})

In other words instead of:
```
git reflog (find the commit sha at HEAD@{1})
git cat-file -p <commit sha>
git cat-file -p <tree sha>
git cat-file -p <blob sha>
git add .
git commit -m "B: recovery"
```
We could have just done:
```
git merge HEAD@{1}
```


# CH-3 (MERGE CONFLICTS)

Working with Git is a dream when all of the developers on a project are committing changes to different lines of code. Things get a little hairy when changes to the same lines are made at the same time (e.g. one commit isn't the parent of another).

A merge conflict occurs when two commits modify the same line and Git can't automatically decide which change to keep and which change to discard.

If we merge feature into main, Git will detect that the return line was changed in both branches independently: which creates a conflict.

When a conflict happens (usually as the result of a merge or rebase) Git will prompt you to manually decide which change to keep. It's okay when the same line is modified in one commit, and then again in a later commit. The problem arises when the same line is modified in two commits that aren't in a parent-child relationship.

Conflicting changes on two different branches is not a problem. The problem only arises when you try to merge those branches. When you do, Git will detect the conflict and ask you to resolve it.

## Edit the file
Resolving conflicts is a manual process. When they happen, Git marks the conflicted files and asks you to resolve the conflict by editing the files in your editor.

```
abcde
<<<<<<< HEAD
fgh
=======
ijk
>>>>>>> main
```
fgh wala apna commit hai ......aur ijk wala dusra ka commit 

Your editor might even highlight the conflict markers to make it easier to see, but at the end of the day, it's just text. The top section, between the <<<<<<< HEAD and ======= lines, is our branch's version of the file. The bottom section, between the ======= and >>>>>>> main lines, is the version of the file that's on the main branch ("theirs" or as I say "Stupid Greg's").

In many cases, you might want to keep one change and discard the other. That's common when you're dealing with code changes. In this case, we're dealing with content, so we want to keep both changes.

## RESOLUTION
After manually editing any conflicting files (sometimes it's more than one file or more than one section of a file) you need to simply add and commit the changes. This tells Git that you've resolved the conflict and it can continue with the merge.

Note: Git will let you commit conflict markers, so check you removed them all before you continue with a merge.


## Ours and Theirs
In a merge conflict:
- ```Ours ```refers to the branch you are on (merging into)
- ```Theirs``` refers to the branch being merged
```
# you're on the "ours" branch, the "theirs" branch is "their_branch"
git merge their_branch
```

##  Checkout Conflict
We've manually edited files to resolve conflicts, but it turns out Git has some built-in tools to help us out.

The git checkout command can checkout the individual changes during a merge conflict using the --theirs or --ours flags.

- --ours will overwrite the file with the changes from the branch you are currently on and merging into
- --theirs will overwrite the file with the changes from the branch you are merging into the current branch
 ```
git checkout --theirs path/to/file
```

# CH- 4 (REBASE CONFLICTS)

Full disclosure: a lot of rebase's bad rep comes from conflicts! Fear not. It's nothing you can't handle with just a smidge of practice.

Rebasing feels scarier because it rewrites Git history, which means if you're not careful, you can lose work in an unrecoverable way. But as long as you understand what's going on, it will make your (and your team's) Git history cleaner and easier to understand.

In the "real world," what happens most often is:
```
You switch to a new branch, say fix_bug, which is a copy of main.
While you're fixing the bug, someone else merges their changes into main.
You fix the bug, and it so happens that you edited the same files (and lines) that the other person did.
You open a Pull Request to merge (or rebase) fix_bug into main, then Git tells you there's a conflict.
You resolve the conflict on your branch.
You complete the Pull Request with the conflict resolved.
```

The same git checkout --theirs and git checkout --ours commands we used with merge can be used to resolve conflicts during a rebase.

"Accept incoming change" is the same as git checkout --theirs, and "Accept current change" is the same as git checkout --ours.



## Repeat Resolution Setup
A common complaint about rebase is that there are times when you may have to manually resolve the same conflicts over and over again. This is especially prevalent when you have a long-running feature branch, or even more likely, multiple feature branches that are being rebased onto main.

### RERERE to the Rescue
- https://youtu.be/k-4zAoQHKcY?si=LvZbUCn7WV46ZlG9
- The git rerere functionality is a bit of a hidden feature. The name stands for “reuse recorded resolution” and, as the name implies, it allows you to ask Git to remember how you’ve resolved a hunk conflict so that the next time it sees the same conflict, Git can resolve it for you automatically.

In other words, if enabled, rerere will remember how you resolved a conflict (applies to rebasing but also merging) and will automatically apply that resolution the next time it sees the same conflict. Pretty neat, right?


# CH-5 (SQUASH)
Every dev team will have different standards and opinions on how to use Git. Some teams require all pull requests to contain a single commit, while others prefer to see a series of small, focused commits.
If you join a team that prefers a single commit, you will need to know how to "squash" your commits together. To be fair, even if you don't need a single commit, squashing is useful to keep your commit history clean.

![squash](./squash.png)

## How to Squash
Perhaps confusingly, squashing is done with the git rebase command! Here are the steps to squash the last n commits:

<li>Start an interactive rebase with the command git rebase -i HEAD~n, where n is the number of commits you want to squash.</li>
<li>Git will open your default editor with a list of commits. Change the word pick to squash for all but the first commit.</li>
<li>Save and close the editor.</li>
The -i flag stands for "interactive," and it allows us to edit the commit history before Git applies the changes. HEAD~n is how we reference the last n commits. HEAD points to the current commit (as long as we're in a clean state) and ~n means "n commits before HEAD."

## Why does rebase squash?
Remember, rebase is all about replaying changes. When we rebase onto a specific commit (HEAD~n), we're telling Git to replay all the changes from the current branch on top of that commit. Then we use the interactive flag to "squash" those changes so that Rebase will apply them all in a single commit.

## Force Push
So we did some naughty stuff. We squashed main which means that because our remote main branch on GitHub has commits that we removed, git won't allow us to push our changes because they're totally out of sync.

The git push command has a --force flag that allows us to overwrite the remote branch with our local branch. It's a very dangerous (but useful) flag that overrides the safeguards and just says "make that branch the same as this branch."
```
git push origin main --force
```

## Squashing PR
So we did a weird thing (squash commits on main), now let's do the common thing: squash all the commits on a feature branch for a pull request. If your team prefers single-commit-pull-requests, this will likely be your workflow:
```
Create a new branch off of main.
Go about your work on the feature branch making commits as you go.
When you're ready to get your code into main, squash all your commits into a single commit.
Push your branch to the remote repository.
Open a pull request from the feature branch into main.
Merge the pull request once it's approved.
```


# Ch-6 (STASH)

Git Stash
The git stash command records the current state of your working directory and the index (staging area). It's kinda like your computer's copy/paste clipboard. It records those changes in a safe place and reverts the working directory to match the HEAD commit (the last commit on your current branch).

To stash your current changes and revert the working directory to match HEAD:
```
git stash
```
To list your stashes:
```
git stash list
```

https://youtu.be/_02z3tvMr4I

## POP

```
git stash
git stash pop
git stash list
```

The pop command will (by default) apply your most recent stash entry to your working directory and remove it from the stash list. It effectively undoes the git stash command. It gets you back to where you were.

## What is the Stash
The git stash command stores your changes in a stack (FILO) data structure. That means that when you retrieve your changes from the stash, you'll always get the most recent changes first.
```
# 'git stash' pushes a change onto the stash
git stash
```
```
# 'git stash pop' pops a change off the stash
# and applies it to the working directory
git stash pop
```

https://youtu.be/zSbF3xmSrMg

![stash](./stash.png)


A "stash" is a collection of changes that you've not yet committed. They might just be "raw" working directory changes, or they might be staged changes. Both can be stashed. So, for example, you can:
```
Make some changes to your working directory
Stage those changes
Make some more changes without staging them
Stash all of that
```
When you do, the "stash entry" will contain both the staged and unstaged changes and both your working directory and index will be reverted to the state of the last commit. It's a very convenient way to "pause" your work and come back to it later.


## Multiple Stashes
possible but not recommended


## Pop Again
Now that you've solved the CEO's problem, time to get back to your own work. Here are a few more useful options when it comes to git stash.

## Apply Without Removing From Stash
This will apply the most recent stash changes, just like pop, but it will keep the stash in the stash list.
```
git stash apply
```
## Remove a Stash Without Applying
This will remove the most recent stash from the stash list without applying it to your working directory.
```
git stash drop
```

## Reference a Specific Stash
Most stash commands allow you to reference a specific stash by its index.
```
# Apply the third (0, 1, 2) most recent stash
git stash apply stash@{2}

# Remove the third most recent stash
git stash drop stash@{2}
```
