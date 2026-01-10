```yaml
number: 6162
title: Simplify available package version ranges when the name includes markers or extras
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/available-name
created_at: 2024-08-16T20:09:37Z
updated_at: 2024-08-16T20:21:51Z
url: https://github.com/astral-sh/uv/pull/6162
synced_at: 2026-01-10T13:09:50Z
```

# Simplify available package version ranges when the name includes markers or extras

---

_Pull request opened by @zanieb on 2024-08-16 20:09_

There were different `PubGrubPackage` types so they never matched the available versions set! Luckily, the available versions are agnostic to the markers and optional dependencies so we can just broaden to using `PackageName` as a lookup key.

Addresses yet another complaint in https://github.com/astral-sh/uv/issues/5046

---

_Label `error messages` added by @zanieb on 2024-08-16 20:09_

---

_@charliermarsh approved on 2024-08-16 20:10_

---

_Merged by @zanieb on 2024-08-16 20:21_

---

_Closed by @zanieb on 2024-08-16 20:21_

---

_Branch deleted on 2024-08-16 20:21_

---
