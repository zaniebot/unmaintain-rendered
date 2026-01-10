```yaml
number: 9066
title: Prefer Python executable names that match the request over default names
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/order-executables
created_at: 2024-11-12T17:42:17Z
updated_at: 2024-11-13T16:00:27Z
url: https://github.com/astral-sh/uv/pull/9066
synced_at: 2026-01-10T12:00:00Z
```

# Prefer Python executable names that match the request over default names

---

_Pull request opened by @zanieb on 2024-11-12 17:42_

This restores behavior previously removed in https://github.com/astral-sh/uv/pull/7649.

I thought it'd be clearer (and simpler) to have a consistent Python executable name ordering. However, we've seen some cases where this can be surprising and, in combination with #8481, can result in incorrect behavior. For example, see https://github.com/astral-sh/uv/issues/9046 where we prefer `python3` over `python3.12` in the same directory even though `python3.12` was requested. While `python3` and `python3.12` both point to valid Python 3.12 environments there, the expectation is that when `python3.12` is requested that the `python3.12` executable is preferred. This expectation may be less obvious if the user requests `python@3.12`, but uv does not distinguish between these request forms. Similarly, this may be surprising as by default uv prefers `python` over `python3` but when requesting `python3.12` the preference will be swapped.

---

_Label `bug` added by @zanieb on 2024-11-12 17:42_

---

_Comment by @zanieb on 2024-11-12 19:07_

The Windows failures are due to the switched preference for `python3` and `python`. For some reason, both exist on Windows. Perhaps we should always prefer `python3` over `python`. More complexity, but... we don't want Python 2 anyway.

---

_@charliermarsh approved on 2024-11-12 22:32_

---

_Comment by @zanieb on 2024-11-13 14:28_

I fixed the Windows failures by switching the test from 3.11/3.12 to 3.12/3.13 since 3.12+ seem to use `python3.exe` consistently on the GitHub runners but 3.11 used `python.exe`.

---

_Merged by @zanieb on 2024-11-13 16:00_

---

_Closed by @zanieb on 2024-11-13 16:00_

---

_Branch deleted on 2024-11-13 16:00_

---
