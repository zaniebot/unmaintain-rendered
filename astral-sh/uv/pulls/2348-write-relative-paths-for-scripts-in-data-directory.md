```yaml
number: 2348
title: Write relative paths for scripts in data directory
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rel
created_at: 2024-03-10T22:52:12Z
updated_at: 2024-03-10T23:02:03Z
url: https://github.com/astral-sh/uv/pull/2348
synced_at: 2026-01-10T14:54:43Z
```

# Write relative paths for scripts in data directory

---

_Pull request opened by @charliermarsh on 2024-03-10 22:52_

## Summary

In #2000, I shipped a regression whereby we stopped writing relative paths for scripts within `data` directories. The net effect here is that we aren't _uninstalling_ binaries in all cases. (This does _not_ apply to entrypoints, only scripts in `data` directories.)

Closes https://github.com/astral-sh/uv/issues/2330.

## Test Plan

Most Python packages ship entrypoints, not binaries, so I don't know how to test this cheaply. But I did test it locally by verifying that `uv` is now removed from the `bin` directory after an uninstall.


---

_Label `bug` added by @charliermarsh on 2024-03-10 22:54_

---

_Merged by @charliermarsh on 2024-03-10 23:02_

---

_Closed by @charliermarsh on 2024-03-10 23:02_

---

_Branch deleted on 2024-03-10 23:02_

---
