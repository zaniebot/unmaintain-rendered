```yaml
number: 1277
title: Strip UNC prefix when setting working directory
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/unc
created_at: 2024-02-12T00:47:18Z
updated_at: 2024-02-12T00:51:37Z
url: https://github.com/astral-sh/uv/pull/1277
synced_at: 2026-01-12T16:04:33Z
```

# Strip UNC prefix when setting working directory

---

_@charliermarsh_

## Summary

For PEP 517 builds, the current working directory needs to be set to the directory of the source distribution. It turns out that on Windows, if you use a UNC path for the working directory, then relative paths are interpreted relative to the root of the current drive ([source](https://www.fileside.app/blog/2023-03-17_windows-file-paths/#paths-relative-to-the-root-of-the-current-drive)). So, when builds attempted to resolve relative paths, they always errored...

This PR ensures that we remove the UNC prefix when setting the current working directory.

Closes #1238.

## Test Plan

I tested this on my Windows machine by installing `ujson` with `--no-binary ujson`. (I don't want to add that specific test, since it's really slow to build.)

---

_Label `bug` added by @charliermarsh on 2024-02-12 00:47_

---

_Label `windows` added by @charliermarsh on 2024-02-12 00:47_

---

_@charliermarsh reviewed on 2024-02-12 00:48_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/installed.rs`:9 on 2024-02-12 00:48_

I renamed this trait.

---

_@charliermarsh reviewed on 2024-02-12 00:48_

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:775 on 2024-02-12 00:48_

This is the fix.

---

_Merged by @charliermarsh on 2024-02-12 00:51_

---

_Closed by @charliermarsh on 2024-02-12 00:51_

---

_Branch deleted on 2024-02-12 00:51_

---
