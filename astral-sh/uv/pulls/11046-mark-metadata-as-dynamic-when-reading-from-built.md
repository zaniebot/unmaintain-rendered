```yaml
number: 11046
title: Mark metadata as dynamic when reading from built wheel cache
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dyn
created_at: 2025-01-29T01:03:34Z
updated_at: 2025-01-29T01:27:10Z
url: https://github.com/astral-sh/uv/pull/11046
synced_at: 2026-01-12T16:09:39Z
```

# Mark metadata as dynamic when reading from built wheel cache

---

_@charliermarsh_

## Summary

The issue here boils down to: when we write metadata that came from building the wheel itself, we aren't setting the `dynamic` field.

We now _always_ set the dynamic field when reading, even when we read cached data.

Closes https://github.com/astral-sh/uv/issues/11047.

---

_Label `bug` added by @charliermarsh on 2025-01-29 01:13_

---

_Merged by @charliermarsh on 2025-01-29 01:27_

---

_Closed by @charliermarsh on 2025-01-29 01:27_

---

_Branch deleted on 2025-01-29 01:27_

---
