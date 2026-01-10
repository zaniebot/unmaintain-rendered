```yaml
number: 8756
title: "Fix `add httpx` example with real git branch"
type: pull_request
state: merged
author: manzt
labels:
  - documentation
assignees: []
merged: true
base: main
head: manzt/docs-fix-branch
created_at: 2024-11-01T16:58:51Z
updated_at: 2024-11-02T02:47:33Z
url: https://github.com/astral-sh/uv/pull/8756
synced_at: 2026-01-10T11:59:59Z
```

# Fix `add httpx` example with real git branch

---

_Pull request opened by @manzt on 2024-11-01 16:58_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The example in the docs for adding a git source with `--branch` fails because `main` doesn't exist.

```sh
uv add git+https://github.com/encode/httpx --branch main
# error: Git operation failed
#   Caused by: failed to fetch into: /Users/manzt/.cache/uv/git-v0/db/4c0b1441d92956e1
#   Caused by: failed to fetch branch `main`
#   Caused by: process didn't exit successfully: `/usr/bin/git fetch --force --update-head-ok 'https://github.com/encode/httpx' '+refs/heads/main:refs/remotes/origin/main'` (exit status: 128)
```

This PR changes the example to the default branch for httpx, `master`.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

N/A

<!-- How was it tested? -->


---

_Label `documentation` added by @charliermarsh on 2024-11-01 17:03_

---

_Merged by @charliermarsh on 2024-11-01 17:03_

---

_Closed by @charliermarsh on 2024-11-01 17:03_

---

_Comment by @charliermarsh on 2024-11-01 17:03_

Thanks!

---

_Branch deleted on 2024-11-01 17:07_

---

_Comment by @zanieb on 2024-11-01 23:32_

As a minor note, that branch is going to change to `main` soon, I think.

---

_Comment by @manzt on 2024-11-02 02:47_

Thanks for the heads up. Now following https://github.com/encode/httpx/issues/3362 and will try to follow up PR when changed!

---
