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

# CH -7 (REBASE) ***MOST IMPORTANT***

https://youtu.be/nHcfoHOW4uA

basically  Fast Forward Merge hi hai but ismein hum explicitly bol rhe hein ki fast forward wala diagram lga 

```
git rebase main
```

![Rebase](/Rebase.png)

## When to Rebase
git rebase and git merge are different tools.

An advantage of merge is that it preserves the true history of the project. It shows when branches were merged and where. One disadvantage is that it can create a lot of merge commits, which can make the history harder to read and understand.

A linear history is generally easier to read, understand, and work with. Some teams enforce the usage of one or the other on their main branch, but generally speaking, you'll be able to do whatever you want with your own branches.

## Warning
You should never rebase a public branch (like main) onto anything else. Other developers have it checked out, and if you change its history, you'll cause a lot of problems for them.

However, with your own branch, you can rebase onto other branches (including a public branch like main) as much as you want.

# CH -8 (RESET)

## Git Reset Soft
The git reset command can be used to undo the last commit(s) or any changes in the index (staged but not committed changes) and the worktree (unstaged and not committed changes).
```
git reset --soft HEAD~1
```
The --soft option is useful if you just want to go back to a previous commit, but keep all of your changes. Committed changes will be uncommitted and staged, while uncommitted changes will remain staged or unstaged as before. HEAD~1 refers to the commit 1 before the current commit.

```
git reset --soft <hash>
```

## Git Reset Hard .... *** DANGER *** 

### Danger
I want to stress how dangerous this command can be. When you deleted the file, because it was tracked in Git, it was trivially easy to recover. However, if you have some changes that you do want to keep, running git reset --hard will delete them for good.

Always be careful when using git reset --hard. It's a powerful tool, but it's also a dangerous one.

### Reset to a Specific Commit
If you want to reset back to a specific commit, you can use the git reset --hard command and provide a commit hash. For example:
```
git reset --hard a1b2c3d
```
This will reset your working directory and index to the state of that commit, and all the changes made after that commit are lost forever.



# CH -9 (REMOTE)

This is where the "distributed" in "distributed version control system" comes from. We can have "remotes", which are just external repos with mostly the same Git history as our local repo.

https://youtu.be/lR_hYwCAaH4?si=8jW29vUCh_nJ9n8F

When it comes to Git (the CLI tool), there really isn't a "central" repo. GitHub is just someone else's repo. Only by convention and convenience have we, as developers, started to use GitHub as a "source of truth" for our code.

## ADDING REMOTE

In git, another repo is called a "remote." The standard convention is that when you're treating the remote as the "authoritative source of truth" (such as GitHub) you would name it the "origin".

By "authoritative source of truth" we mean that it's the one you and your team treat as the "true" repo. It's the one that contains the most up-to-date version of the accepted code.

Command Syntax
```
git remote add <name> <uri>
```
ye yaad rkhna hai ki uri location hai yaani online bhi ho skti hai ya to aisi bhi C:/users/om/demo

## Fetch 
Adding a remote to our Git repo does not mean that we automagically have all the contents of the remote. First, we need to fetch the contents.
***************************************************
fetch ka matlab hai ki hmare paas saara metadata aajeyga ...........but sirf iss command ko use krne se kuch nhi hoga worktree ko
This downloads copies of all the contents of the .git/objects directory (and other book-keeping information) from the remote repository into your current one.
to iska faayda kya hai ???
git log will not show commits until we explicitly say ``` git log remote/branch ``` , ye kyuin chal paa rha hai because we downloaded meta data with the help of fetch command 
***********************************************
https://youtu.be/5o9ltH6YmtM?si=PhAENEXjwjMWA1P8

## merge 

To actually get the chages in our repo , we use 
```git merge remote/branch```


# CH -10 (GITHUB)

GitHub is the most popular website for Git repositories (projects) online. That is, for hosting "remotes" on a central website. GitHub serves several purposes:

As a backup of all your code on the cloud in case something happens to your computer
As a central place to share your code and collaborate on it with others
As a public portfolio for your coding projects

## Git != GitHub
It's important to understand that Git and GitHub are not the same! Git is an open-source command line tool for managing code files. GitHub and its primary competitors, GitLab and Bitbucket, are commercial web products that use Git. Their websites give us a way to store our code that's managed by Git.


## Github Repo 
https://cli.github.com/

https://youtu.be/5o9ltH6YmtM?si=HCbwtDM3KH-obxQv

## Git push
```
git push origin main
```

You can also push a local branch to a remote with a different name:
```
git push origin <localbranch>:<remotebranch>
```

It's less common to do this, but nice to know.

You can also delete a remote branch by pushing an empty branch to it:

```
git push origin :<remotebranch>
```


## PULL
Fetching is nice, but most of the time we want the actual file changes from a remote repo, not just the metadata.

```
git pull [<remote>/<branch>]
```

The syntax [...] means that the bracketed remote and branch are optional. If you execute git pull without anything specified it will pull your current branch from the remote repo.


## Pull Requests
On GitHub, a pull request is a way to propose some changes. It's very common to use pull requests if you want to make changes to an open source project, or if you're working on a team.

Pull requests allow team members to see what changes are being proposed and to discuss them before they are merged into the main codebase.


## My Workflow
We've covered a lot of Git basics. There's certainly more you can learn, but this will give you a solid foundation to work with as a developer.

All that said, I want to leave you with a note on my simple workflow. 90% Of the time, you're only using a handful of git commands to get your coding work done.

### Keep stuff on GitHub
I keep all my serious projects on GitHub. That way if my computer explodes, I have a backup, and if I'm ever on another computer, I can just clone the repo and get back to work.

### Rebase vs Merge
I've configured Git to rebase by default on pull, rather than merge so I keep a linear history. If you want to do the same, you can add this to your global Git config:

```
git config --global pull.rebase true
```
### My Solo Workflow
When I'm working by myself, I usually stick to a single branch, main. I mostly use Git on solo projects to keep a backup remotely and to keep a history of my changes. I only rarely use separate branches.

Make changes to files
git add . (or git add <files> if I only want to add specific files)
git commit -m "a message describing the changes"
git push origin main
It really is that simple for most solo work. git log, git reset, and some others are of course useful from time to time, but the above is the core of what I do day-to-day.

## My Team Workflow
When you're working with a team Git gets a bit more involved (and we'll cover more of this in part 2 of this course). Here's what I do:

Update my local main branch with git pull origin main
Checkout a new branch for the changes I want to make with git switch -c <branchname>
Make changes to files
git add .
git commit -m "a message describing the changes"
git push origin <branchname> (I push to the new branch name, not main)
Open a pull request on GitHub to merge my changes into main
Ask a team member to review my pull request
Once approved, click the "Merge" button on GitHub to merge my changes into main
Delete my feature branch, and repeat with a new branch for the next set of changes

