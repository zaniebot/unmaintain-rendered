```yaml
number: 9860
title: "Remove `-isysroot` when patching sysconfig"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/patch-sysconfig-ii
created_at: 2024-12-13T03:53:23Z
updated_at: 2024-12-13T19:49:28Z
url: https://github.com/astral-sh/uv/pull/9860
synced_at: 2026-01-12T16:09:00Z
```

# Remove `-isysroot` when patching sysconfig

---

_@charliermarsh_

## Summary

This is equivalent to https://github.com/indygreg/python-build-standalone/pull/414, but at install-time, so that it affects older Python builds too.


---

_Comment by @zanieb on 2024-12-13 18:28_

Should this be scoped to macOS?

---

_@zanieb approved on 2024-12-13 18:28_

---

_Comment by @charliermarsh on 2024-12-13 19:38_

I think I'll leave it? We only set this in the macOS builds anyway, IIRC.

---

_Merged by @charliermarsh on 2024-12-13 19:49_

---

_Closed by @charliermarsh on 2024-12-13 19:49_

---

_Branch deleted on 2024-12-13 19:49_

---
