```yaml
number: 17048
title: Bump ambient-id to 0.0.7
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
  - dependencies
assignees: []
merged: true
base: main
head: ww/bump
created_at: 2025-12-09T16:38:00Z
updated_at: 2025-12-09T17:08:28Z
url: https://github.com/astral-sh/uv/pull/17048
synced_at: 2026-01-12T16:12:35Z
```

# Bump ambient-id to 0.0.7

---

_@woodruffw_

## Summary 

This bumps our own `ambient-id` crate to 0.0.7. The only notable change in this release is BuildKite support, which means that `uv publish` will be able to do Trusted Publishing between BuildKite and Python indices that support it as a provider. Currently none do, but having client exchange support is a requirement regardless 🙂 

## Test Plan

Tested within `ambient-id` itself; the publishing integration tests within uv should ensure this doesn't regress on current TP support. Once actual BuildKite publishing is supposed on indices, I'll follow up with full integration tests for those as well.

---

_Review requested from @konstin by @woodruffw on 2025-12-09 16:38_

---

_Assigned to @woodruffw by @woodruffw on 2025-12-09 16:38_

---

_Label `internal` added by @woodruffw on 2025-12-09 16:38_

---

_Label `dependencies` added by @woodruffw on 2025-12-09 16:38_

---

_@zanieb approved on 2025-12-09 16:56_

---

_@konstin approved on 2025-12-09 17:06_

---

_Merged by @woodruffw on 2025-12-09 17:08_

---

_Closed by @woodruffw on 2025-12-09 17:08_

---

_Branch deleted on 2025-12-09 17:08_

---
