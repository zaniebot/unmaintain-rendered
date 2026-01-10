```yaml
number: 2766
title: Use distribution database and index for all pre-resolution phases
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/wait
created_at: 2024-04-02T00:04:32Z
updated_at: 2024-04-02T00:34:14Z
url: https://github.com/astral-sh/uv/pull/2766
synced_at: 2026-01-10T14:49:08Z
```

# Use distribution database and index for all pre-resolution phases

---

_Pull request opened by @charliermarsh on 2024-04-02 00:04_

## Summary

Ensures that if we resolve any distributions before the resolver, we cache the metadata in-memory.

_Also_ ensures that we lock (important!) when resolving Git distributions.

---

_Marked ready for review by @charliermarsh on 2024-04-02 00:14_

---

_Label `internal` added by @charliermarsh on 2024-04-02 00:14_

---

_@charliermarsh reviewed on 2024-04-02 00:14_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:1382 on 2024-04-02 00:14_

This test fails on `main`.

---

_Label `internal` removed by @charliermarsh on 2024-04-02 00:17_

---

_Label `bug` added by @charliermarsh on 2024-04-02 00:17_

---

_Merged by @charliermarsh on 2024-04-02 00:34_

---

_Closed by @charliermarsh on 2024-04-02 00:34_

---

_Branch deleted on 2024-04-02 00:34_

---
