```yaml
number: 4280
title: Use registry URL for fetching source distributions from lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/u
created_at: 2024-06-12T16:48:56Z
updated_at: 2024-06-12T17:01:30Z
url: https://github.com/astral-sh/uv/pull/4280
synced_at: 2026-01-10T13:54:02Z
```

# Use registry URL for fetching source distributions from lockfile

---

_Pull request opened by @charliermarsh on 2024-06-12 16:48_

## Summary

This is just a logic bug with no testing. We were using the registry URL (like `https://pypi.org/simple`) as the URL from which a source distribution should be downloaded.

Closes https://github.com/astral-sh/uv/issues/4281.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2024-06-12 16:49_

---

_Label `preview` added by @charliermarsh on 2024-06-12 16:49_

---

_Merged by @charliermarsh on 2024-06-12 17:01_

---

_Closed by @charliermarsh on 2024-06-12 17:01_

---

_Branch deleted on 2024-06-12 17:01_

---
