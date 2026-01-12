```yaml
number: 7271
title: "Replace `tokio-tar` with `krata-tokio-tar` fork"
type: pull_request
state: merged
author: mgorny
labels:
  - bug
assignees: []
merged: true
base: main
head: krata-tokio-tar
created_at: 2024-09-10T19:03:25Z
updated_at: 2024-09-11T02:08:25Z
url: https://github.com/astral-sh/uv/pull/7271
synced_at: 2026-01-12T16:07:45Z
```

# Replace `tokio-tar` with `krata-tokio-tar` fork

---

_@mgorny_

## Summary

Replace the unmaintained `tokio-tar` crate with the `krata-tokio-tar` fork.  The latter just merged a fix necessary for the crate to work on PowerPC, and has better chances of future maintenance.

Fixes #3423

## Test Plan

`cargo test`

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-10 20:14_

---

_Comment by @charliermarsh on 2024-09-10 20:14_

Thanks, just want to take a look and compare before merging.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-09-10 20:14_

---

_Comment by @charliermarsh on 2024-09-10 21:19_

I reviewed each new commit. They seem reasonable to me. We only ever unpack into empty directories so I'm not sure the bug fixes around removing targets is relevant.

---

_Label `bug` added by @charliermarsh on 2024-09-10 21:20_

---

_@BurntSushi approved on 2024-09-10 21:27_

I took a look too, and things LGTM.

---

_Merged by @charliermarsh on 2024-09-10 21:28_

---

_Closed by @charliermarsh on 2024-09-10 21:28_

---

_Branch deleted on 2024-09-11 02:08_

---

_Comment by @mgorny on 2024-09-11 02:08_

Thanks!

---
