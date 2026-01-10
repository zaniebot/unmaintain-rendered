```yaml
number: 16463
title: Clarify system Python discovery logging order
type: pull_request
state: merged
author: terror
labels:
  - tracing
assignees: []
merged: true
base: main
head: discovery-logging-order
created_at: 2025-10-27T03:17:55Z
updated_at: 2025-10-28T12:18:13Z
url: https://github.com/astral-sh/uv/pull/16463
synced_at: 2026-01-10T06:36:16Z
```

# Clarify system Python discovery logging order

---

_Pull request opened by @terror on 2025-10-27 03:17_

Resolves https://github.com/astral-sh/uv/issues/16453

When `--system` is used, the debug log now reports interpreter discovery sources in the same order they are probed, prioritising the PATH ahead of managed installs on every platform. I also added a few unit tests that use `DiscoveryPreferences::sources`, ensuring the log strings stay aligned with the actual discovery sequence for both System and OnlySystem preferences.


---

_@zanieb approved on 2025-10-28 12:18_

---

_Label `tracing` added by @zanieb on 2025-10-28 12:18_

---

_Merged by @zanieb on 2025-10-28 12:18_

---

_Closed by @zanieb on 2025-10-28 12:18_

---
