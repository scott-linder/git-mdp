%title: git for the uninitiated
%author: stringy

# Git

[Git][1] is a distributed version control system.

It helps you manage arbitrary versions of files under a given directory.

[1]: http://git-scm.com/

---

# Version control

A "version" is probably more aptly called a "revision", because it is *not*
like the release schemes used by software packages (although git can help you
organize those as well).

You can track all sorts of things, all without any complicated server setup or
network connection.

---

# Why is useful?

Also phrased as "I just make a new copy of my project directory when I want to
save a version".

This technique covers the first advantage of version control (albeit somewhat
poorly), which is:

* _History_ - the ability to effortlessly roll back to any point in time in the
  life of a project.

Version control offers more than just a static archive of past versions, though:

* _Verifiability_ - a means to verify the integrity of all past versions under
  control (against both corruption and even some forms of malicious tampering).
* _Collaboration_ - a way to effortlessly share code with others in a team
  environment.
    * Sharing patches (or worse: entire trees) with others does not scale.
* _Stability_ - the freedom to safely work on a large or unstable feature
  without worrying at all about the overall stability of the project and its
  release builds.
    * As a corollary, this is extremely useful in a classroom setting where
    it is imperative to always have *something* to turn in which builds.

---

# How do you use git?

There are a lot of tools to learn, but the basic structure is always:

    $ git COMMAND [OPTIONS...]

Where COMMAND is usually a verb, like 'clone', 'add', 'commit', 'push', etc.

The workflow is usually:

* Work
    * `$EDITOR $FILES`
* Commit
    * `git add $FILES`
    * `git commit -m "$MESSAGE"`
* Repeat

---

# A local example

    $ git init project
    $ cd project
    $ echo 'echo Hello git!' >> main.sh
    $ git add main.sh
    $ git commit -m "add main script"
    $ chmod +x main.sh
    $ git add main.sh
    $ git commit -m "make main script executable"

---

# The three stores

Your work is somewhere, but *where*, you might ask?

There are three places where objects exist at any given time:

* The *working directory* (where you do your work)
* The *cache/index* (where you stage changes to be committed)
* The *repository* (committed revisions)

You can move objects between these three stores in various ways:

    (Working Directory) (Staging Area) (Repository)
            |               |               |
            <-------------Checkout----------+
            |               |               |
            +-----Stage----->               |
                            |               |
                            +----Commit----->

---

# Distributed

Git is *distributed*, which means that most of the operations you do are on
a *local copy* of the project tree.

There are only a handful of git commands which communicate over the network,
including things like `clone`, `push`, `pull`, `fetch`, etc.

This means you can work from *anywhere*, and not need to worry about
synchronizing changes with remotes until you get back on the network.

---

# Remotes

Because most operations are local, there is a notion of *remotes* to help you
manage other copies of your local tree.

When you `clone` a repository, it will not only fetch a copy of the tree, but
also add a remote called *origin* which points back at the place you cloned
from.

This is why a common workflow is:

    $ git clone git@some.remote.host:/path/to/repo
    $ # work here
    $ git push origin master

Here, `origin` in the push command is actually a nicer name for
`git@some.remote.host:/path/to/repo`

You can have as many remotes as you like, but commonly you will just have
*origin*, as it is created for you when you clone from, for example, GitHub.

But what about that bit about `master`? That is a *branch*.

---

# A remote example

    $ git clone https://github.com/example/example
    $ cd example
    $ echo 'echo Hello git!' >> main.sh
    $ git add main.sh
    $ git commit -m "add main script"
    $ git push origin master

---

# Branches

A branch is just a pointer to a specific commit in your tree.

When you make a new commit, the pointer for whatever branch you are currently
on is updated to point to that new commit, but all other branch pointers are
left alone.

Typically there is at least one branch, called *master*, which is usually
where the "release" version of your project (or equivalent) is.

It is *very common* to branch away from master in git, typically to do work
which may break master (i.e. just about anything).

It is so common that hosting services like GitHub support a notion of
"pull requests" between branches.

---

# Branches

Let's say you are on branch "master" and have an initial commit "A"

    $ git checkout master
    $ git commit -m "A"

    master
    |
    V
    A

---

# Branches

If you now commit something, you create commit "B", and "master" follows suit
by updating to point to it:

    $ git commit -m "B"

        master
        |
        V
    A<--B

---

# Branches

You might now want to work on a new feature, but don't want to disturb "master"
while you work on it. You might checkout a new branch called "feature", which
will point to the same commit as "master" for the time being.

    $ git checkout -b "feature" # the -b flag creates the branch as it checks it out

        master
        |
        V
    A<--B
        ^
        |
        feature

---

# Branches

If you now make a commit "C", you will update the "feature" branch, but not
"master", even though they both share the same "B" commit.

    $ git commit -m "C"

        master
        |
        V
    A<--B<--C
            ^
            |
            feature

---

# Branches

Now lets say there is a massive security vulnerability in your project, and
you are are willing to throw all caution to the wind and make a hotfix on
master to get it patched as soon as possible:

    $ git checkout master
    $ git commit -m "D"

            master
            |
            V
    A<--B<--D
        ^
        |
        C
        ^
        |
        feature

---

# Branches

Now you go back to working on the feature:

    $ git checkout feature
    $ git commit -m "E"

            master
            |
            V
    A<--B<--D
        ^
        |
        C<--E
            ^
            |
            feature

The two branches have now diverged, but share a common parent tree up to and
including commit "B"

---

# Merging

Now consider that your work on feature is complete and you want to *merge*
it into master.

    $ git checkout master
    $ git merge --no-ff feature -m "F"

                master and feature
                |
                V
    A<--B<--D<--F
        ^       |
        |       |
        C<--E<--+

You create a *new* commit F which merges the changes made in both D *and* E

---

# Merging

It is now safe to delete the feature branch, as it is the same as master:

    $ git branch -d feature

                master
                |
                V
    A<--B<--D<--F
        ^       |
        |       |
        C<--E<--+

---

# This is a just an introduction

There are still a great many more things you can do with git: I have only tried
to cover some of the essentials.

There are a number of great resources, including the [official website][2]
, [cheat sheets][3], and just the man pages.

Note: for a given command, the man page for it has a prefix of "git-", e.g.:

    $ man git-clone
    $ man git-push
    $ # etc.

[2]: http://git-scm.com/
[3]: https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf
