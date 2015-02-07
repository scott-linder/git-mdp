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

# An example

    $ git init project
    $ cd project
    $ echo 'echo Hello git!' >> main.sh
    $ git add main.sh
    $ git commit -m "add main script"
    $ chmod +x main.sh
    $ git add main.sh
    $ git commit -m "make main script executable"

---

# Distributed

Git is *distributed*, which means that most of the operations you do are on
a *local copy* of the project tree.

There are only a handful of git commands which communicate over the network,
including things like `clone`, `push`, `pull`, `fetch`, etc.

This means you can work from *anywhere*, and not need to worry about
synchronizing changes with remotes until you get back on the network.
