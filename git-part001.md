# An overly verbose yet incomplete introduction to Git (Part 1 of ?)

## Conventions for this document

```
# text following a '#' is a comment

# text after a '$' is command you type
$ echo "hello world!"
# text without anything in front of it is expected output
hello world!
```

## What is git?
Git is a version control system. 

Remember in school when you were working on that awful paper and you had like 8 different copies because you just couldn't make up your mind which version you liked? Remember how you totally submitted the wrong version for grading?

Git tries to keep you from getting your way like that. You get to have the millions of copies of the file on your computer without getting completely and utterly confused (at least in theory!).

Git also allows you to keep a history of your changes to your files and compare arbitrary versions with each other. You can even give different versions names.

## Sounds fantastic, how do I get started?
Well, I'm going to cheat and assume that you've already installed git at this point.
If you haven't, here's a good youtube video: https://youtube.com/watch?v=nbFwejIsHlY

I will also assume that you're using git-bash if you're on Windows.

Windows Users: To follow along in this tutorial, go to your Documents folder, right-click and then select "Git Bash Here."

There are also a few things you should set up before you start using git:
```
# like your identity!
$ git config --global user.name "Your Name or some Alias"
$ git config --global user.email "youremailOrsomefakeemail@somewhere.com"

# configure your text editor
# by default, git installs Vim as your text editor
# if you're just getting started, you probably don't want that
# I say this as I edit this document in Vim...
$ git config --global core.editor notepad.exe
```

## Can I get started for real now?

There are a number of concepts related to git, but let's start with the basics.

First you'll need to create a git repository (repo for short). You can think of a repository as a container that will hold all versions of your project.

### Creating a respository
To create a git repository, you:

```
# should create a folder for your project: 
$ mkdir mySuperAwesomeProject

# switch to that directory
$ cd mySuperAwesomeProject

# initialize the project repository
$ git init .
Initialized empty Git repository in /path/to/mySuperAwesomeProject/.git/
```

This will create a folder called `.git` in the `mySuperAwesomeProject` directory.

You can verify this by running the following command:

```
$ ls -a
.  ..  .git
```

*Trivia:* On *nix systems, a file or folder that begins with a '.' is hidden. If you're on Windows, it will also be created as a hidden folder, so if you want to see it in file explorer, change your settings so you can see hidden files.

Now that you have a container for the 100s of versions of your project, it's time to make some commits. You can think of a commit as a snapshot of your project in a certain state.

### Making commits
All right, so let's make some stuff to put in our project.

You woke up in the middle of the night, and you've decided that you wanted to be a hip-hop artist, but with a twist - you will write all your lyrics as haikus.

Create a new file in our project directory and call it `haiku.txt`.

Open the file and type the following:

```
Escargot, my car go
one sixty, swiftly
wreck it buy a new one
```

Save the file.

At the command line type:

```
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        haiku.txt

nothing added to commit but untracked files present (use "git add" to track)
```

`git status` tells us the current state of our repository.
Let's go over the output in sections:

```
On branch master
```
Git has something called branches - you can think of it like making a copy of your project to give a friend so that the two of you can work on it in parallel. There are more use cases for branches than that, but we won't get into that here.


```
No commits yet
```
This just means that we haven't saved any versions of our project yet.


```
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        haiku.txt
```
Untracked files...wuzzat? When you are working with git, you have to tell it which files to keep track of, otherwise it ignores them. As you can see, git will also let you know which files aren't tracked.

Notice that git tries to help you out too:
```
  (use "git add <file>..." to include in what will be committed)
# ...
nothing added to commit but untracked files present (use "git add" to track)
```

Let's follow git's advice.
```
$ git add haiku.txt
```

Check the status again.
```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   haiku.txt
```

Some of the output is the same, but now we have some new stuff:
```
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   haiku.txt
```

Git tells us which file have been changed since the last version. Also notice once again that git is trying to help us:
```
  (use "git rm --cached <file>..." to unstage)
```
In the event we didn't mean to add a file to this commit (version), we can use `git rm --cached filename` to get get rid of it.

In fact, let's do that.
```
$ git rm --cached haiku.txt
rm 'haiku.txt'
```

If you check the status again, you'll see it's the same as before we added `haiku.txt` before.

Sorry to jerk ya around, but let's add it once again.

```
$ git add haiku.txt
```

At long last, the moment you've been waiting for - time to commit!

```
$ git commit -m "Wrote the first few bars."
[master (root-commit) fc3b063] Wrote the first few bars.
 1 file changed, 3 insertions(+)
 create mode 100644 haiku.txt
```

the `-m` switch indicates that you're adding a message afterwards. If you just use `git commit` then git will open a text editor for you to write a commit message. After you save and close the file, git will process the commit.

I won't go over the output here because...well I haven't paid much attention to it before.

Now that you've written your first haiku you - wait a second! You realized that you don't have the right syllable count. It's supposed to be 7, 5, and 7, but yours are 6-5-6.

Let's edit the file again. Change the contents to:

```
Escargot, see my car go
one sixty, swiftly
wreck it, I'll buy a new one
```

Save and close the file.

Let's check the repo status.

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   haiku.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Looks like the output is a little bit different. There is also some terminology we haven't seen before - specifically `not staged for commit` and `working directory`.

**Short segue!**
There are 3 core parts of git:
- Working directory
- Staging area
- History

*Working directory:* the directory with the files you're working on.

*Staging area:* whenever you run `git add <file>`, that file is added here. Adding a file "stages it for commit" - which is just fancy way of saying that you want all changes to this file to be included in the upcoming commit. You say a file is "staged for commit". Again, adding a file tells git to pay attention should this file be changed in the future.

It might seem a bit unnecessary to have this staging area, but there may be times where you don't want to commit all the changes you've made. Don't worry about it too much, just keep it in mind - it may make sense later.

*History:* All the commits (versions) of your project along with whatever commit messages you add when you make a commit.
**End segue.**

So now that you're an expert, stage `haiku.txt` for commit and make a new commit, and include a nice commit message as well.

### What about this history thing?
If you want to see your commit (version) history:

```
$ git log
commit b78d04600895da87e24d29fa3695f957ad4d7d04 (HEAD -> master)
Author: Christopher Wallace <cwallace@notorious.com>
Date:   Mon Mar 23 21:42:01 2020 -0400

    Fixed the syllable count.

commit fc3b06378f67b5f95b5b704281108e3fbb366718
Author: Christopher Wallace <cwallace@notorious.com>
Date:   Mon Mar 23 20:45:13 2020 -0400

    Wrote the first few bars.

```

The really long letter/number combinations are SHA1 hashes - they are used as identifiers for specific commits.

Notice that the commits are displayed in reverse order.

This command line output isn't very fun to wade through. If you're using git-bash on windows, you may prefer to use `gitk` which will give you a useful though visually unappealing view of the history.

Ok, I think that's enough for you and for me. See ya next time!
