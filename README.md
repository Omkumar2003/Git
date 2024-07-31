# Git
To read this you should have the basic knowledge of git .This repo goes in advance topic 
All git important commands and information

# CH -1 ()
Git is the distributed version control system (VCS).

## Porcelain and Plumbing

In Git, commands are divided into high-level ("porcelain") commands and low-level ("plumbing") commands. The porcelain commands are the ones that you will use most often as a developer to interact with your code. Some porcelain commands are:

```git status
git add
git commit
git push
git pull
git log
```
Don't worry about what they do yet, we'll cover them in detail soon. Some examples of plumbing commands are:

```
git apply
git commit-tree
git hash-object
```
We'll focus on the high-level commands because that's what you use 99% of the time as a developer, but we'll dip down into the low-level commands occasionally to really understand how Git works.


# CH-2 (REPO)

#### Status
A file can be in one of several states in a Git repository. Here are a few important ones:

<li>untracked: Not being tracked by Git</li>
<li>staged: Marked for inclusion in the next commit</li>
<li>committed: Saved to the repository's history</li>
<li>The git status command shows you the current state of your repo. It will tell you which files are untracked, staged, and committed.</li>

```
git status
```

### Staging

```
git add .
```


### Commit 

A commit is a snapshot of the repository at a given point in time. It's a way to save the state of the repository, and it's how Git keeps track of changes to the project. A commit comes with a message that describes the changes made in the commit.

```
git commit -m "mujhe nhi pta kya likhuin"
```

If you screw up a commit message, you can change it with the --amend flag. For example:
Change the last commit message
```
git commit --amend -m "A: add contents.md"
```

## Half of Git
You've learned half of Git.

...well, not really. But kind of.

Half of your workflow as a developer will just be 3 simple commands:

git status
git add
git commit
It's most of what you need to work effectively as a solo developer. Another 40% of Git is about collaborating and storing your work on a remote server.

The last 10% is mostly about fixing mistakes, rolling back changes, and other advanced topics. Don't worry, we'll cover those too.

## Git Log
A Git repo is a (potentially very long) list of commits, where each commit represents the full state of the repository at a given point in time.

The git log command shows a history of the commits in a repository. This is what makes Git a version control system. You can see:
<li>Who made a commit</li>
<li>When the commit was made</li>
<li>What was changed</li>

we all have lisented about OBSERVABILITY in distrubuted system it can be done with help of logging(haan metrics se bhi kar skte hein)

Each commit has a unique identifier called a "commit hash". This is a long string of characters that uniquely identifies the commit. Here's an example of mine:

You can give whole hash or just fir 7 char =arctes to git .....it will work 


 use the -n and --no-pager options to limit the maximum number of commits shown, and more importantly, to run it without the interactive pager. E.g.:

 ```
git --no-pager log -n 10
```


