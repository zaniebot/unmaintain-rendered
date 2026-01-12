```yaml
number: 2826
title: "Respect `--no-build` and `--no-binary` in `--find-links`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/find
created_at: 2024-04-05T01:47:36Z
updated_at: 2024-04-05T02:00:40Z
url: https://github.com/astral-sh/uv/pull/2826
synced_at: 2026-01-12T16:05:14Z
```

# Respect `--no-build` and `--no-binary` in `--find-links`

---

_@charliermarsh_

## Summary

In working on `--require-hashes`, I noticed that we're missing some incompatibility tracking for `--find-links` distributions. Specifically, we don't respect `--no-build` or `--no-binary`, so if we select a wheel due to `--find-links`, we then throw a hard error when trying to build it later (if `--no-binary` is provided), rather than selecting the source distribution instead.

Closes https://github.com/astral-sh/uv/issues/2827.


---

_Label `bug` added by @charliermarsh on 2024-04-05 01:47_

---

_Marked ready for review by @charliermarsh on 2024-04-05 01:47_

---

_Merged by @charliermarsh on 2024-04-05 02:00_

---

_Closed by @charliermarsh on 2024-04-05 02:00_

---

_Branch deleted on 2024-04-05 02:00_

---
