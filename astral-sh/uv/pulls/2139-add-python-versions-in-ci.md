```yaml
number: 2139
title: add python versions in CI
type: pull_request
state: closed
author: ChannyClaus
labels: []
assignees: []
draft: true
base: main
head: ci-multiple-python-versions
created_at: 2024-03-03T17:05:13Z
updated_at: 2024-03-09T23:50:34Z
url: https://github.com/astral-sh/uv/pull/2139
synced_at: 2026-01-12T16:04:53Z
```

# add python versions in CI

---

_@ChannyClaus_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Resolves https://github.com/astral-sh/uv/issues/2126 (work in progress)

## Test Plan

<!-- How was it tested? -->


---

_@konstin reviewed on 2024-03-03 18:58_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:56 on 2024-03-03 18:58_

All these test should run on a single runner in our case since building uv (as a rust project) and running tests with a lot of integration test is much more expensive than for virtualenv, a small scoped python project.

---

_@ChannyClaus reviewed on 2024-03-03 19:07_

---

_Review comment by @ChannyClaus on `.github/workflows/ci.yml`:56 on 2024-03-03 19:07_

😭 makes sense - i'll look into that maybe later today. thanks for pointing it out early

---

_@charliermarsh reviewed on 2024-03-03 19:24_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:56 on 2024-03-03 19:24_

I believe that the Python version here is only used to bootstrap Python itself. We then have code that downloads specific pinned Python versions for the test suites.

---

_@ChannyClaus reviewed on 2024-03-03 22:52_

---

_Review comment by @ChannyClaus on `.github/workflows/ci.yml`:56 on 2024-03-03 22:52_

just to clarify - so the preferred solution here is to set up all of the python versions on a single runner (for each os) and vary the versions _only_ for the venv test? it does seem more common to use a runner per each runtime version in other open source projects i'm familiar with, but also i agree that it's less wasteful with a single runner for all versions if we are varying the version only for the venv tests.

---

_Review comment by @konstin on `.github/workflows/ci.yml`:56 on 2024-03-04 09:39_

We had problems where e.g. python-build-standalone would fail when creating a venv from another venv, that the paths of debian/ubuntu pythons were different than those elsewhere, windows store python failing on canonicalize, pypy has a different venv structure, etc. It would be great if we could test as many of these scenarios on CI to avoid regressions, and because we can't install them automatically for our local test suite (e.g. the `apt install python` isn't available on fedora and we can't modify a developers system by adding extra pythons to the registry). How exactly to best implement this - i'm not sure! Maybe something like installing all pythons and passing them to the venv tests as an env var, or maybe we have to run the venv tests multiple times, this requires some experimentation.

For the majority of our integration tests, we run with python 3.12 from python-build-standalone from the bootstrap script, some use different versions to check we handle markers, builds and caching correctly. We don't want to them multiple times since they are slow (lots of network, decompression and io) and the snapshots expect python 3.12.

---

_@konstin reviewed on 2024-03-04 09:39_

---

_@ChannyClaus reviewed on 2024-03-06 03:14_

---

_Review comment by @ChannyClaus on `.github/workflows/ci.yml`:56 on 2024-03-06 03:14_

Ah the joys of supporting multiple platforms 😆 - I'll try to take a look this coming weekend, thanks for the detailed context!

---

_Comment by @ChannyClaus on 2024-03-09 21:21_

might not be able to get to this this weekend - if anyone else is interested in taking this, feel free to take over this / close this and start a new one; sorry about that! 😭 

---

_Comment by @zanieb on 2024-03-09 23:50_

@ChannyClaus I'll look into this one. Thank you!

---

_Closed by @zanieb on 2024-03-09 23:50_

---
