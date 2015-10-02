# One Repo to Build Them All

## Introduction
This repo is a hub for those working on the GA4GH
[Reference Server](https://github.com/ga4gh/server),
[Schemas](https://github.com/ga4gh/schemas), and
[Compliance](https://github.com/ga4gh/compliance) projects.

Using the instructions below and, optionally, the scripts in the
[bin](bin) directory, you can make synchronized changes across GA4GH
repos.

## Setting it up
You must first install Google's `repo` tool.  See
[installing repo](https://source.android.com/source/downloading.html#installing-repo)
for the simple details of how to do this.

### Getting help
See
[Repo command reference](https://source.android.com/source/using-repo.html)
for more information on `repo`'s subcommands.  (Note that we are not
using the [Gerrit](https://android-review.googlesource.com/)
code-review tool, so please ignore the `repo` subcommands  `download` and
`upload`).

You can also get help from `repo` itself.  Typing `repo help`
_(subcommand)_ will display information on the given `repo` 
subcommand.  For example:

```bash
$ repo help init
Summary
-------
Initialize repo in the current directory

Usage: repo init [options]
...
```

### Get started

Clone this repo.  By default it will be called `build`:

```bash
$ git clone https://github.com/ga4gh/build.git
```

If that's too generic, you can give it a different name, like `ga4gh-build`:

```bash
$ git clone https://github.com/ga4gh/build.git ga4gh-build
```

Add the included scripts to your path (the exact syntax will depend on the
shell you use and the location of your local copy of this repo):

```bash
$ export PATH=$PATH:~/ga4gh-build/bin
```

*That takes care of the build tool itself.  Now that it's set up, we
can use it to work on the GA4GH projects themselves.*

Create a new directory to serve as the parent directory for the three GA4GH
projects above.

```bash
$ mkdir MyBigBuild
```

Set your current directory to the one you just created:

```bash
$ cd MyBigBuild
```

Then set things up with `repo`.

```bash
$ repo init -u https://github.com/ga4gh/build.git
```

Synchronize your directories with the server:

```bash
$ repo sync
```

or, to use a specific branch,

```bash
$ repo sync -b some_remote_branch
```

If you're using the normal GA4GH Git flow, you'll want to set up an
`origin` Git remote to which you will normally push your changes.

Assuming your GitHub user name is `myaccount`, do that like this using
the `add-origin` script:

```bash
$ add-origin myaccount
```

Use `show-remotes` to review your remotes to make sure they're
correct.  They should look something like this:

```bash
$ show-remotes
project compliance/
upstream   https://github.com/ga4gh/compliance (fetch)
upstream   https://github.com/ga4gh/compliance (push)
origin  https://github.com/myaccount/compliance (fetch)
origin  https://github.com/myaccount/compliance (push)

project schemas/
upstream   https://github.com/ga4gh/schemas (fetch)
upstream   https://github.com/ga4gh/schemas (push)
origin  https://github.com/myaccount/schemas (fetch)
origin  https://github.com/myaccount/schemas (push)

project server/
upstream   https://github.com/ga4gh/server (fetch)
upstream   https://github.com/ga4gh/server (push)
origin  https://github.com/myaccount/server (fetch)
origin  https://github.com/myaccount/server (push)
```

## Using it

### To start working on a new task (topic branch)

```bash
$ repo start fix_bug_112 compliance server schemas
```

Then make your changes.  Within each of the subdirectories (`compliance`,
`server`, etc. -- `repo` calls these "projects"), use `git` as you normally would.

Let's say you made coordinated changes to the `compliance` and
`schemas` projects.  You can see your changes with respect to each project with this
command, analogous to `git status`:

```bash
$ repo status
project schemas/                                branch fix_bug_112
 -m     src/main/resources/avro/reads.avdl
project compliance/                             branch fix_bug_112
 -m     cts-java/src/test/java/org/ga4gh/cts/api/TestData.java
project server/                                 branch fix_bug_112

```

The status codes (`-m` in the example above) are explained
[here](https://source.android.com/source/using-repo.html#status).

You can get a more detailed view of things with a command like this:

```bash
$ multi-diff
project compliance/
diff --git a/cts-java/src/test/java/org/ga4gh/cts/api/TestData.java b/cts-java/s
(details omitted)

project schemas/
diff --git a/src/main/resources/avro/reads.avdl b/src/main/resources/avro/reads.
(details omitted) 
```

Assuming you're satisfied with your changes, you change directories
into each affected project and use `git add` and
`git commit` as normal, then push the changes to your private repo
(`origin`).

Check that you have no uncommitted files:
```bash
$ repo status
project compliance/                             branch fix_bug_112
project schemas/                                branch fix_bug_112
project server/                                 branch fix_bug_112
```

It looks clean, so it's time to push:

```bash
$ repo forall -p -c 'git push origin fix_bug_112'
```

Continuing on GitHub, you issue pull requests for the changes you made
to each project.
