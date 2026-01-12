```yaml
number: 4793
title: Avoid hangs before exiting CLI
type: pull_request
state: merged
author: ibraheemdev
labels:
  - bug
assignees: []
merged: true
base: main
head: ibraheem/hang
created_at: 2024-07-03T23:50:09Z
updated_at: 2024-07-04T00:05:46Z
url: https://github.com/astral-sh/uv/pull/4793
synced_at: 2026-01-12T16:06:28Z
```

# Avoid hangs before exiting CLI

---

_@ibraheemdev_

## Summary

The resolver sometimes starts HTTP requests that end up not being necessary. When dropping the Tokio runtime before exiting we currently wait for those to complete. This can cause noticeable hangs in the CLI, particularly when the runtime is blocked on slow DNS resolution.

Resolves https://github.com/astral-sh/uv/issues/4599.

## Test Plan

This change resolves any reproducible hangs for me locally.

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-07-03 23:50_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-07-03 23:50_

---

_Label `bug` added by @ibraheemdev on 2024-07-03 23:50_

---

_@charliermarsh approved on 2024-07-03 23:51_

---

_Comment by @charliermarsh on 2024-07-03 23:51_

Nice.

---

_Merged by @ibraheemdev on 2024-07-04 00:05_

---

_Closed by @ibraheemdev on 2024-07-04 00:05_

---

_Branch deleted on 2024-07-04 00:05_

---
