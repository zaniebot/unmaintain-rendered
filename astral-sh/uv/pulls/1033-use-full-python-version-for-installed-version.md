```yaml
number: 1033
title: Use full Python version for installed version
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/version
created_at: 2024-01-22T02:02:07Z
updated_at: 2024-01-22T06:44:40Z
url: https://github.com/astral-sh/uv/pull/1033
synced_at: 2026-01-12T16:04:22Z
```

# Use full Python version for installed version

---

_@charliermarsh_

## Summary

`interpreter.version()` returns the `python_full_version`, but the marker variant uses `python_version` instead of `python_full_version` -- so it's omitting the patch.

---

_Review requested from @zanieb by @charliermarsh on 2024-01-22 02:02_

---

_Label `bug` added by @charliermarsh on 2024-01-22 02:02_

---

_@zanieb approved on 2024-01-22 06:31_

Thank you!

---

_Comment by @zanieb on 2024-01-22 06:35_

Eek now we're going to need to make sure our local Python versions are the latest patch or filter it out of tests 😬 

We should probably codify all the Python versions used in the repository at some point.

---

_Merged by @zanieb on 2024-01-22 06:44_

---

_Closed by @zanieb on 2024-01-22 06:44_

---

_Branch deleted on 2024-01-22 06:44_

---
