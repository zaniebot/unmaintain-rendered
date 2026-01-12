```yaml
number: 9876
title: "Avoid `panic!()` when current directory does not exist"
type: pull_request
state: merged
author: bepri
labels:
  - bug
assignees: []
merged: true
base: main
head: main
created_at: 2024-12-13T19:13:53Z
updated_at: 2024-12-13T21:39:46Z
url: https://github.com/astral-sh/uv/pull/9876
synced_at: 2026-01-12T16:09:01Z
```

# Avoid `panic!()` when current directory does not exist

---

_@bepri_

## Summary

If the shell is currently in a directory that no longer exists, uv will panic from any command. Panicking is a confusing behavior to those unfamiliar with Rust and can sometimes make it hard to determine the true issue.

Closes #9875 

## Test Plan

The reproduction steps in the issue report were followed and uv no longer panics. `uv version` can still successfully print the version if the directory does exist.

---

_Comment by @bepri on 2024-12-13 19:17_

Failing CI seems unrelated - runner needs `liblzma-dev` maybe?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-13 20:29_

---

_Label `bug` added by @charliermarsh on 2024-12-13 20:29_

---

_@charliermarsh approved on 2024-12-13 21:30_

---

_Renamed from "fix: Remove possible panic!()" to "Avoid `panic!()` when current directory does not exist" by @charliermarsh on 2024-12-13 21:30_

---

_Merged by @charliermarsh on 2024-12-13 21:39_

---

_Closed by @charliermarsh on 2024-12-13 21:39_

---
