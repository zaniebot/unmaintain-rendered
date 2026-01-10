```yaml
number: 835
title: "Fix `tracing-duration-export` compilation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/trace
created_at: 2024-01-08T17:55:06Z
updated_at: 2024-01-08T18:05:02Z
url: https://github.com/astral-sh/uv/pull/835
synced_at: 2026-01-10T15:44:44Z
```

# Fix `tracing-duration-export` compilation

---

_Pull request opened by @charliermarsh on 2024-01-08 17:55_

## Summary

I'm unable to run `puffin-cli` on `main` as the `tracing-durations-export` is marked as optional, but the crate actually depends on it to compile. Further, without `tracing-durations-export`, there are `Option` types that can't resolve to a concrete type.

This PR fixes compilation with and without the feature.

---

_Review requested from @konstin by @charliermarsh on 2024-01-08 17:55_

---

_Label `bug` added by @charliermarsh on 2024-01-08 17:55_

---

_Merged by @charliermarsh on 2024-01-08 18:04_

---

_Closed by @charliermarsh on 2024-01-08 18:04_

---

_Branch deleted on 2024-01-08 18:04_

---

_Comment by @konstin on 2024-01-08 18:05_

sorry, forgot to test without the feature

---
