# One Repo to Build them All #

## Introduction
This repo is a hub for those working on the GA4GH
[Reference Server](https://github.com/ga4gh/server),
[Schemas](https://github.com/ga4gh/schemas), and
[Compliance](https://github.com/ga4gh/compliance) projects.

## Using it
You must first install Google's `repo` tool.  See
[installing repo](https://source.android.com/source/downloading.html#installing-repo)
for details.

Create a new directory to server as the parent directory for the three
projects above.

```bash
$ mkdir MyBigBuild
```

Set your current directory:

```bash
$ cd MyBigBuild
```

Then set things up with `repo`.

```bash
$ repo init -u https://github.com/hjellinek/ga4gh-build.git
```

Finally, synchronize your directories with the server:

```bash
$ repo sync
```

or

```bash
$ repo sync -b some_branch
```
