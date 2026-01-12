```yaml
number: 5198
title: "Use `which`-retrieved path directly when spawning pager"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/page
created_at: 2024-07-18T20:18:52Z
updated_at: 2024-07-18T20:30:15Z
url: https://github.com/astral-sh/uv/pull/5198
synced_at: 2026-01-12T16:06:41Z
```

# Use `which`-retrieved path directly when spawning pager

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/5124.

## Test Plan

Ran `cargo run -- help pip compile` on my Windows machine, which failed before but succeeds after this change.


---

_Label `bug` added by @charliermarsh on 2024-07-18 20:18_

---

_Label `windows` added by @charliermarsh on 2024-07-18 20:18_

---

_Merged by @charliermarsh on 2024-07-18 20:29_

---

_Closed by @charliermarsh on 2024-07-18 20:29_

---

_Branch deleted on 2024-07-18 20:29_

---

_Comment by @zanieb on 2024-07-18 20:30_

Glad that worked!

---
