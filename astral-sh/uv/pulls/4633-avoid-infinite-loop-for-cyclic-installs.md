```yaml
number: 4633
title: Avoid infinite loop for cyclic installs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/c
created_at: 2024-06-28T18:53:55Z
updated_at: 2024-06-28T20:15:29Z
url: https://github.com/astral-sh/uv/pull/4633
synced_at: 2026-01-12T16:06:21Z
```

# Avoid infinite loop for cyclic installs

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/4629.

## Test Plan

Run `uv sync` with:

```toml
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.9"
dependencies = ["poetry"]
```


---

_Label `bug` added by @charliermarsh on 2024-06-28 18:53_

---

_Label `preview` added by @charliermarsh on 2024-06-28 18:53_

---

_@zanieb approved on 2024-06-28 19:28_

---

_@charliermarsh reviewed on 2024-06-28 20:00_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:2664 on 2024-06-28 20:00_

Smallest case I could find (surfaced from an old Poetry issue and then digging through the versions that were relevant back in 2018)...

---

_Merged by @charliermarsh on 2024-06-28 20:15_

---

_Closed by @charliermarsh on 2024-06-28 20:15_

---

_Branch deleted on 2024-06-28 20:15_

---
