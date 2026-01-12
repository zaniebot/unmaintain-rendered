```yaml
number: 1725
title: Add support for absolute paths on Windows
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/abs-path
created_at: 2024-02-20T01:03:51Z
updated_at: 2024-02-20T01:36:54Z
url: https://github.com/astral-sh/uv/pull/1725
synced_at: 2026-01-12T16:04:42Z
```

# Add support for absolute paths on Windows

---

_@charliermarsh_

## Summary

The main change is that we need to have an explicit list of protocols we _do_ support (like `https`), so that when we see a Windows absolute path (`C:\...`), we don't treat the `C` as a protocol itself.

Closes https://github.com/astral-sh/uv/issues/1539.


---

_Label `bug` added by @charliermarsh on 2024-02-20 01:29_

---

_Marked ready for review by @charliermarsh on 2024-02-20 01:31_

---

_Merged by @charliermarsh on 2024-02-20 01:36_

---

_Closed by @charliermarsh on 2024-02-20 01:36_

---

_Branch deleted on 2024-02-20 01:36_

---
