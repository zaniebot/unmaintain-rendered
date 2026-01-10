```yaml
number: 4743
title: "Add `uvx` to binary artifacts"
type: pull_request
state: merged
author: zanieb
labels:
  - releases
assignees: []
merged: true
base: main
head: zb/uvx-binaries
created_at: 2024-07-02T22:02:59Z
updated_at: 2024-07-02T22:27:04Z
url: https://github.com/astral-sh/uv/pull/4743
synced_at: 2026-01-10T13:48:28Z
```

# Add `uvx` to binary artifacts

---

_Pull request opened by @zanieb on 2024-07-02 22:02_

We have a custom binary build step and we were not including the new `uvx` binary (from #4632) in the artifacts but the installer expects them to exist.

---

_@charliermarsh approved on 2024-07-02 22:03_

Looks correct. And the sdist entry shouldn't need any modifications.

---

_Label `release` added by @zanieb on 2024-07-02 22:06_

---

_Marked ready for review by @zanieb on 2024-07-02 22:08_

---

_Merged by @zanieb on 2024-07-02 22:15_

---

_Closed by @zanieb on 2024-07-02 22:15_

---

_Branch deleted on 2024-07-02 22:15_

---
