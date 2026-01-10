```yaml
number: 6284
title: Make cache robust to removed archives
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rev
created_at: 2024-08-20T23:39:48Z
updated_at: 2024-08-20T23:56:26Z
url: https://github.com/astral-sh/uv/pull/6284
synced_at: 2026-01-10T13:09:51Z
```

# Make cache robust to removed archives

---

_Pull request opened by @charliermarsh on 2024-08-20 23:39_

## Summary

Closes https://github.com/astral-sh/uv/issues/6147.

## Test Plan

- `cargo run pip install flask --no-binary flask --cache-dir foo --reinstall`
- `rm -rf foo/archive-v0`
- `cargo run pip install flask --no-binary flask --cache-dir foo --reinstall`


---

_Merged by @charliermarsh on 2024-08-20 23:56_

---

_Closed by @charliermarsh on 2024-08-20 23:56_

---

_Branch deleted on 2024-08-20 23:56_

---

_Label `bug` added by @charliermarsh on 2024-08-20 23:56_

---
