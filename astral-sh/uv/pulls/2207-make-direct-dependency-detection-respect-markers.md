```yaml
number: 2207
title: Make direct dependency detection respect markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/markers
created_at: 2024-03-05T16:57:49Z
updated_at: 2024-03-05T17:25:07Z
url: https://github.com/astral-sh/uv/pull/2207
synced_at: 2026-01-12T16:04:55Z
```

# Make direct dependency detection respect markers

---

_@charliermarsh_

## Summary

When determining "direct" dependencies, we need to ensure that we respect markers. In the linked issue, the user had an optional dependency like:

```toml
[project.optional-dependencies]
dev = [
  "setuptools>=64",
  "setuptools_scm>=8"
]
```

By not respecting markers, we tried to resolve `setuptools` to the lowest-available version. However, since `setuptools>=64` _isn't_ enabled (since it's optional), we won't respect _that_ constraint.

To be consistent, we need to omit optional dependencies just as we will at resolution time.

Closes https://github.com/astral-sh/uv/issues/2203.

## Test Plan

`cargo test`


---

_Marked ready for review by @charliermarsh on 2024-03-05 17:16_

---

_Label `bug` added by @charliermarsh on 2024-03-05 17:16_

---

_Merged by @charliermarsh on 2024-03-05 17:25_

---

_Closed by @charliermarsh on 2024-03-05 17:25_

---

_Branch deleted on 2024-03-05 17:25_

---
