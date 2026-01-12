```yaml
number: 11468
title: Omit lockfile version when additional fields are dynamic
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dep-dyn
created_at: 2025-02-13T01:05:22Z
updated_at: 2025-02-13T01:44:15Z
url: https://github.com/astral-sh/uv/pull/11468
synced_at: 2026-01-12T16:09:51Z
```

# Omit lockfile version when additional fields are dynamic

---

_@charliermarsh_

## Summary

Just a logic issue... If we see a dynamic field that isn't `"version"`, we end up _not_ propagating the fact that `"version"` is also dynamic.

Closes https://github.com/astral-sh/uv/issues/11460.


---

_Label `bug` added by @charliermarsh on 2025-02-13 01:05_

---

_Marked ready for review by @charliermarsh on 2025-02-13 01:05_

---

_Merged by @charliermarsh on 2025-02-13 01:44_

---

_Closed by @charliermarsh on 2025-02-13 01:44_

---

_Branch deleted on 2025-02-13 01:44_

---
