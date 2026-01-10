```yaml
number: 8237
title: "Respect relative paths in `uv build` sources"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rel-build
created_at: 2024-10-16T01:39:31Z
updated_at: 2024-10-16T01:46:30Z
url: https://github.com/astral-sh/uv/pull/8237
synced_at: 2026-01-10T12:54:05Z
```

# Respect relative paths in `uv build` sources

---

_Pull request opened by @charliermarsh on 2024-10-16 01:39_

## Summary

Right now, `uv build` will fail if a package depends on a local source in `build-system.requires`.

---

_Label `bug` added by @charliermarsh on 2024-10-16 01:39_

---

_Comment by @charliermarsh on 2024-10-16 01:40_

@mmerickel -- this may also be the source of your failure (a bug in `uv build`).

---

_Merged by @charliermarsh on 2024-10-16 01:46_

---

_Closed by @charliermarsh on 2024-10-16 01:46_

---

_Branch deleted on 2024-10-16 01:46_

---
