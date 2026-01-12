```yaml
number: 10164
title: "fix: always write slash paths to RECORD file"
type: pull_request
state: merged
author: frostming
labels:
  - bug
  - windows
  - compatibility
assignees: []
merged: true
base: main
head: fix/RECORD-path
created_at: 2024-12-26T01:25:16Z
updated_at: 2024-12-26T16:08:57Z
url: https://github.com/astral-sh/uv/pull/10164
synced_at: 2026-01-12T16:09:09Z
```

# fix: always write slash paths to RECORD file

---

_@frostming_

Signed-off-by: Frost Ming <me@frostming.com>

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR solves an issue on Windows that platform-specific paths are written to the `RECORD` file when installing, which is inconsistent with PEP 376, quoting:

> Each record is composed of three elements:
>
>the file’s path
> * a ‘/’-separated path, relative to the base location, if the file is under the base location.
> * a ‘/’-separated path, relative to the base location, if the file is under the installation prefix AND if the base location is a subpath of the installation prefix.
> * an absolute path, using the local platform separator

## Test Plan

<!-- How was it tested? -->
Test case included

---

_@charliermarsh approved on 2024-12-26 02:40_

Thank you!

---

_Comment by @charliermarsh on 2024-12-26 02:41_

I can take a look at the test failures tomorrow.

---

_Comment by @frostming on 2024-12-26 02:44_

> I can take a look at the test failures tomorrow.

It is related to the change, ~~which I have fixed~~.

- - -

I have a difficulty following the way through how install receipt is written, since I don't have a Windows machine to test.

---

_Label `bug` added by @charliermarsh on 2024-12-26 14:23_

---

_Label `compatibility` added by @charliermarsh on 2024-12-26 14:23_

---

_Label `windows` added by @charliermarsh on 2024-12-26 14:23_

---

_Merged by @charliermarsh on 2024-12-26 14:33_

---

_Closed by @charliermarsh on 2024-12-26 14:33_

---

_Branch deleted on 2024-12-26 16:08_

---
