# Git
To read this you should have the basic knowledge of git .This repo goes in advance topic 
All git important commands and information

# CH -1 (Intro)
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


# Ch-3 (INTERNALS)

## Different Hashes

 While commit hashes are derived from their content changes, there's also some other stuff that affects the end hash. For example:

<li>The commit message</li>
<li>The author's name and email</li>
<li>The date and time</li>
<li>Parent (previous) commit hashes</li>

### Note: Hash = SHA
Git uses a cryptographic hash function called SHA-1 to generate commit hashes. We won't go into the details of how SHA-1 works in this course, but it's important to know because you might also hear commit hashes referred to as "SHAs".

## The Plumbing

HAR EK CHIZ FILE HAI 

https://www.youtube.com/watch?v=MyvyqdQ3OjI&t

It's just files all the way down
All the data in a Git repository is stored directly in the (hidden) .git directory. That includes all the commits, branches, tags, and other objects we'll learn about later.

Git is made up of objects that are stored in the .git/objects directory. A commit is just a type of object.


GO EXPLORE THIS AND FIND PATTERNS ...........YOU WILL KNOW WHAT I AM TALKING ABOUT

```
git cat-file -p <hash>
```
commit hash daalega to tree milega .....tree hash daalega to to blob milega ......blob ko agar cat krega to saare diffrences milenge 


## Storing Data
Git stores an entire snapshot of files on a per-commit level. This was a surprise to me! I always assumed each commit only stored the changes made in that commit.

### Optimization
While it's true that Git stores entire snapshots, it does have some performance optimizations so that your .git directory doesn't get too unbearably large.

Git compresses and packs files to store them more efficiently.
Git deduplicates files that are the same across different commits. If a file doesn't change between commits, Git will only store it once.

# CH- 4 (CONFIG)
![ConfigScope](/configScope.png)

There are several locations where Git can be configured. From more general to more specific, they are:

<li>system: /etc/gitconfig, a file that configures Git for all users on the system</li>
<li>global: ~/.gitconfig, a file that configures Git for all projects of a user</li>
<li>local: .git/config, a file that configures Git for a specific project</li>
<li>worktree: .git/config.worktree, a file that configures Git for part of a project</li>

90% of the time you will be using --global to set things like your username and email. The other 9% of the time you will be using --local to set project-specific configurations. The last 1% of the time you might need to futz with system and worktree configurations, but it's extremely rare.

```
git config --add --global user.name "naam"
git config --add --global user.email "email@dalde.bhai"
```

AGAR UPAR WALI CMD MEIN GLOBAL NHI LIKHEGA TO WO LOCAL SCOPE KI SET KREGI 

Let's take the command apart:

<li>git config: The command to interact with your Git configuration.</li>
<li>--add: Flag stating you want to add a configuration.<li>
<li>--global: Flag stating you want this configuration to be stored globally in your ~/.gitconfig. The opposite is "local", which stores the configuration in the current repository<li>

COMMAND RUN KRTE WAQAT KUCH AISA HOGA ....SABSE PHELA WORKTREE MEIN SEARCH KREGA .....AGAR NHI MILA TO LOCAL MEIN ...USMEIN NHI MILA TO GLOBAL MEIN.....AGAR USMEIN NHI MILA TO SYSTEM MEIN.

this will show local scope config
```
cat .git/config
```
this will show global level config
```
cat ~/.gitconfig
```

How to remove Section in git config
```
git config --remove-section section
```


how to remove a key 
```
git config --unset <key>
```


# CH -5 (BRANCH)

### TIP (TRY THESE)
https://git-school.github.io/visualizing-git/
https://learngitbranching.js.org/
#### vs code mein ye extension
git-log--graph

*********************************************************
<b>
 <i>
A branch is just a named pointer to a specific commit.
 </i></b>
*****************************************************
### Tip
Remember, you should be on master because we set init.defaultBranch to master 

## How to rename a branch
```
git branch -m oldname newname
```

## New Branch
You should already be on the main branch: your "default" branch. You can always check with git branch.

Two Ways to Create a Branch

```
git branch my_new_branch
```

Switch is the prefered version
```
git switch -c my_new_branch
```


# CH -6 (MERGE)

https://youtu.be/BAtW9n5FgLM

Run git log ```--oneline --graph --all``` and you should see a nice ASCII art representation of your commit history.

```
git merge anotherBranchName
```


YAAD RKHNA HAI KI AGAR TUNE EK BRANCH "A" SE DUSRI BRANCH "B" NIKALI AUR "A" MEIN TUNE KOI ABHI TAK COMMIT NHI KRA BUT TU "B" MEIN COMMIT KI BARSAAT KR RHA HAI TO GIT AUTOMATICALLY "A" JAHAN BRANCH NIKLI THI THA USS JAGAH KO CHANGE KR KE "B" KE HEAD PE LE AAYEGA


## Fast Forward Merge
Because "B" has all the commits that "A" has, Git automatically does a fast-forward merge. It just moves the pointer of the "base" branch to the tip of the "feature" branch:
