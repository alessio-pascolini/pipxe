# Contributing to piPXE

Thank you for your interest in contributing to piPXE! There are several ways to
contribute, and we appreciate all of them.

* [What Goes Where](#what-goes-where)
* [Bug Reports](#bug-reports)
* [Pull Requests](#pull-requests)

## What Goes Where

This repository contains the application binaries and libraries source code.

If you are porting piPXE to a different target, that code should go in a
seperate repository. Of course, if building your application or port requires
changes in this code base, we encourage you to contribute them here.

## Bug Reports

While the piPXE is designed to be a reliable, it is not bug free, and we can't
fix what we don't know, so please report liberally. If you're not sure if
something is a bug or not, feel free to file a bug anyway.

If you have the chance, before reporting a bug, please search existing issues,
as it's possible that someone else has already reported your error. This doesn't
always work, and sometimes it's hard to know what to search for, so consider
this extra credit. We won't mind if you accidentally file a duplicate report.

Opening an issue is as easy writing an e-mail to one of the contributors and
filling out the fields. Here's a template that you can use to file a bug, though
it's not necessary to use it exactly:

```txt
<short summary of the bug>

I tried this code:

<code sample that causes the bug>

I expected to see this happen: <explanation>

Instead, this happened: <explanation>
```

## Meta

environment:

All three components are important: what you did, what you expected, what
happened instead. Please include the environment you tested on as well as any
steps required to reproduce the error.

If an application faults, piPXE should output a detailed error message.
Including this error message in its entirety can be very helpful.

## Pull Requests

### Step 1: Fork

Fork the project [on GitHub](https://github.com/raballew/pipxe) and checkout
your copy locally.

```bash
git clone git@github.com:username/pipxe.git
cd pipxe
git remote add upstream git://github.com/raballew/pipxe.git
```

### Step 2: Branch

Create a feature branch and start hacking:

```bash
git checkout -b my-branch -t origin/main
```

You should always start new feature branches from `upstream/main`. Try not to
stack up changes on local branches. We try to resolve pull requests in a timely
manner so you shouldn't have too many outstanding changes in flight.

### Step 3: Commit

Make sure git knows your name and email address:

```bash
git config --global user.name "J. Random User"
git config --global user.email "j.random.user@example.com"
```

Add and commit:

```bash
git add my/changed/files
git commit
```

Writing good commit logs is important. A commit log should describe what changed
and why. Follow these guidelines when writing one:

1. The first line should be 50 characters or less and contain a short
   description of the change. All words in the description should be in
   lowercase with the exception of proper nouns, acronyms, and the ones that
   refer to code, like function/variable names. The description can be prefixed
   with the name of the changed environment or part of the app and should start
   with an imperative verb. Example: "target: support x86_64 architecture"
2. Keep the second line blank.
3. Wrap all other lines at 72 columns.
4. Any quoted text or code should be indented four spaces

A good commit log can look something like this:

```txt
environment: explain the commit in one line

Body of commit message is a few lines of text, explaining things
in more detail, possibly giving some background about the issue
being fixed, etc.

If you quote a discussion or some code it should be intended:

    Should we have an example of indentation? Yes I think so.

The body of the commit message can be several paragraphs, and
please do proper word-wrap and keep columns shorter than about
72 characters or so. That way, `git log` will show things
nicely even when it is indented.
```

The header line should be meaningful; it is what other people see when they run
`git shortlog` or `git log --oneline`.

Check the output of `git log --oneline files_that_you_changed` to find out what
subsystem (or subsystems) your changes touch.

### Step 4: Rebase

Use `git rebase` (not `git merge`) to sync your work from time to time.

```bash
git fetch upstream
git rebase upstream/main
```

### Step 5: Push

```bash
git push origin my-branch
```

Go to `https://github.com/<yourusername>/pipxe` and select your branch. Click
the 'Pull Request' button and fill out the form.

### Step 6: Discuss and update

You will probably get feedback or requests for changes to your Pull Request.
This is a big part of the submission process so don't be disheartened!

In general the relevant team members will review a pull request, ask for any
changes, and get support from the larger team before merging a PR. If you are
curious about the specifics, feel free to read through the specific process.

To make changes to an existing Pull Request, make the changes to your branch.
When you push that branch to your fork, GitHub will automatically update the
Pull Request.

You can push more commits to your branch:

```bash
git add my/changed/files
git commit
git push origin my-branch
```

Feel free to post a comment in the Pull Request to ping reviewers if you are
awaiting an answer on something.

Before its ready to merge, your Pull Request should contain a minimal number of
commits (see notes about [rewriting-history](#rewriting-history)).

### Step 7: Landing

In order to land, a Pull Request needs to be reviewed and approved by at least
one person with commit access to the piPXE repository and pass the continuous
integration tests. After that, as long as there are no objections, the Pull
Request can be merged.

## Issue Triage

Sometimes, an issue will stay open, even though the bug has been fixed. And
sometimes, the original bug may go stale because something has changed in the
meantime.

It can be helpful to go through older bug reports and make sure that they are
still valid. Load up an older issue, double check that it's still true, and
leave a comment letting us know if it is or is not. The least recently updated
sort is good for finding issues like this.

## Notes

### Rewriting History

Once the reviewer approves your Pull Request, they might ask you to clean up the
commits. There are a lot of reasons for this. If you have a lot of fixup
commits, and you merge all of them directly, the git history will be bloated.
Or, if your recent commit fixes your previous commit in the same PR, then you
could simply rebase it.

To achieve this, you can use the `git rebase -i` command.

Run `git rebase -i`, which will bring up your default text editor with a content
like:

```txt
pick 02e5bd1 Y

# Rebase 170afb6..02e5bd1 onto 170afb6 (1 command(s))
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

Keep the first commit as pick, and change any commits you'd like to collapse in
the previous commit with either `squash` (or s for short) or `fixup` (or f for
short).

```txt
pick 02e5bd1 target: support x86_64 architecture"

# Rebase 170afb6..02e5bd1 onto 170afb6 (1 command(s))
...
```

Now save and quit the text editor, the rebase will run until the end.

After the rebase is finished, the editor will pop-up again, now you can write
the commit message for the new commit.

`git push -f` to push the squashed commit to GitHub (and update the PR).
