**********************************
GA4GH API Proposed Release Process
**********************************

========
Repos
========
* server - the GA4GH reference server, written in Python
* compliance - the server compliance test suite, written in Java
* schemas - Object definitions and documentation for the API
* (future) Java client
* (future) Python client
* build - home of `repo` tool configuration	

====================
Git Model - Branches
====================
Three types of branches:
* master: 
* release
* topic

=====================
Release Process Rules
=====================
All HEADs must pass Continuous Integration (CI)
* CI: all masters always build and pass tests
* Release: Release tags/branches always consistent across all repos
* Develop: Automatic creation of topic branches across all repos
* Topic CI: A PR against a topic branch must pass CI to be merged

=========================
Suggested Release Cadence
=========================
* Aim for new release every 3 months
* Any topic branches to be included in a release should be merged into master 2 weeks before planned release date, to allow for testing and bugfixes on master
* A regular release cadence will switch the discussion from "when should the next release be" to "what features are mature enough to be included in the next release" - if a feature isn't yet ready, it will simply be delayed until the next regular release
* To help with this aggressive release cadence, topic branches should
  be limited in scope, with the aim of merging into master as soon as feasible

===============
Lifecycle Rules
===============
* Releases designated as "alpha" receive no official support - any bug reports received on them will be addressed in the next beta or stable release.
* Releases designated as "beta" receive support until the next non-beta release is ready
* Stable (non alpha or beta) releases will be supported for a period of ???.

============
Deliverables
============
* Schemas will be released as a tarball containing object definitions in an appropriate IDL
* HTML Documentation will be released on ???
* A version of the server will be released on PyPi
* A JAR of the compliance suite will be created for download from GitHub

-------------------------
A note on version numbers
-------------------------
The reference server and other components are free to use their own version numbering
scheme, but cross-repo releases must be tagged with cross-repo version
numbers.

For example: the same Ref Server can have a Ref Server-local tag
calling it e.g. version `0.2.1abedfc`, but it and its corresponding
`schemas` commit would also be tagged as e.g. `ga4gh_release_0.6.0`.

