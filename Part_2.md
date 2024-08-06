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



