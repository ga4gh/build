# One Repo to Build them All #

## Introduction
This repo is a hub for those working on the GA4GH
[Reference Server](https://github.com/ga4gh/server),
[Schemas](https://github.com/ga4gh/schemas), and
[Compliance](https://github.com/ga4gh/compliance) projects.

## Setting it up
You must first install Google's `repo` tool.  See
[installing repo](https://source.android.com/source/downloading.html#installing-repo)
for the simple details of how to do this.

See
[Repo command reference](https://source.android.com/source/using-repo.html)
for more information on `repo`'s subcommands.  (Note that we are not
using the [Gerrit](https://android-review.googlesource.com/)
code-review tool, so please ignore the `repo` subcommands  `download` and
`upload`).

Create a new directory to server as the parent directory for the three
projects above.

```bash
$ mkdir MyBigBuild
```

Set your current directory:

```bash
$ cd MyBigBuild
```

Then set things up with `repo`. __The URL below will need to change
when we move this to the [GA4GH repository](https://github.com/ga4gh/).__

```bash
$ repo init -u https://github.com/hjellinek/ga4gh-build.git
```

Finally, synchronize your directories with the server:

```bash
$ repo sync
```

or

```bash
$ repo sync -b some_remote_branch
```

If you're using the normal GA4GH git flow, you'll want to set up an
`origin` git remote to which you will push your changes.  (`repo` will
have set up an upstream called `ga4gh` from which you can pull.)

Assuming
your GitHub user name is `myaccount`, do that
like so.

```bash
$ repo forall -p -c 'git remote -v add origin https://github.com/myaccount/$REPO_PATH'
```

Review your remotes like so:

```bash
$ repo forall -p -c 'git remote -v'
project compliance/
ga4gh   https://github.com/ga4gh/compliance (fetch)
ga4gh   https://github.com/ga4gh/compliance (push)
origin  https://github.com/myaccount/compliance (fetch)
origin  https://github.com/myaccount/compliance (push)

project schemas/
ga4gh   https://github.com/ga4gh/schemas (fetch)
ga4gh   https://github.com/ga4gh/schemas (push)
origin  https://github.com/myaccount/schemas (fetch)
origin  https://github.com/myaccount/schemas (push)

project server/
ga4gh   https://github.com/ga4gh/server (fetch)
ga4gh   https://github.com/ga4gh/server (push)
origin  https://github.com/myaccount/server (fetch)
origin  https://github.com/myaccount/server (push)
```

## Using it

### To start working on a new task (branch)

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
$ repo forall -p -c 'git diff .'
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
