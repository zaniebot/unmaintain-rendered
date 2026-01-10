```yaml
number: 8642
title: Actually perform checks on alternate trampoline platforms
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/check-trampoline-matrix
created_at: 2024-10-28T18:47:38Z
updated_at: 2024-10-28T19:12:38Z
url: https://github.com/astral-sh/uv/pull/8642
synced_at: 2026-01-10T12:54:14Z
```

# Actually perform checks on alternate trampoline platforms

---

_Pull request opened by @zanieb on 2024-10-28 18:47_

It seems unintentional that we basically did nothing on these alternative platforms? It seems like an artifact from some previous change.

I'm not sure it's worth running Clippy multiple times. We could also just reduce the matrix here.

---

_Label `internal` added by @zanieb on 2024-10-28 18:47_

---

_Review requested from @konstin by @zanieb on 2024-10-28 18:54_

---

_Marked ready for review by @zanieb on 2024-10-28 18:54_

---

_@charliermarsh approved on 2024-10-28 19:00_

I think initially we didn't build ARM trampolines.

---

_Merged by @zanieb on 2024-10-28 19:12_

---

_Closed by @zanieb on 2024-10-28 19:12_

---

_Branch deleted on 2024-10-28 19:12_

---
