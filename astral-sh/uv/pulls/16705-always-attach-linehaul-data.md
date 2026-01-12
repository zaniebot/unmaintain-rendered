```yaml
number: 16705
title: Always attach linehaul data
type: pull_request
state: merged
author: zsol
labels:
  - registry
assignees: []
merged: true
base: main
head: zsol/jj-txxwyytvlkwm
created_at: 2025-11-12T12:24:02Z
updated_at: 2025-11-12T17:10:17Z
url: https://github.com/astral-sh/uv/pull/16705
synced_at: 2026-01-12T16:12:24Z
```

# Always attach linehaul data

---

_@zsol_

## Summary

This PR makes uv attach linehaul data to the user agent in cases when marker values for a Python interpreter weren't passed in to the registry clients (e.g. `uv auth login`)

## Test Plan

Observe the right user agent string being attached for `uv auth login`. Not sure this warrants a test case, but happy to add one.


---

_@zanieb approved on 2025-11-12 13:46_

---

_Marked ready for review by @zsol on 2025-11-12 16:01_

---

_Label `registry` added by @zsol on 2025-11-12 16:01_

---

_@charliermarsh approved on 2025-11-12 16:43_

---

_Merged by @zsol on 2025-11-12 17:10_

---

_Closed by @zsol on 2025-11-12 17:10_

---

_Branch deleted on 2025-11-12 17:10_

---
