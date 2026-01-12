```yaml
number: 10657
title: Install Python via uv in Windows CI
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/install-python-win
created_at: 2025-01-15T22:37:23Z
updated_at: 2025-01-21T15:17:53Z
url: https://github.com/astral-sh/uv/pull/10657
synced_at: 2026-01-12T16:09:25Z
```

# Install Python via uv in Windows CI

---

_@zanieb_

Python 3.8 is a GHA cache miss now, so it is actually like 30-45s. uv may be faster


---

_Label `internal` added by @zanieb on 2025-01-15 22:37_

---

_Comment by @zanieb on 2025-01-15 22:39_

It looks like about the same now 🤔 

---

_Comment by @zanieb on 2025-01-17 19:12_

It seems ideal to test our own Python on Windows if the timing is the same anyway

---

_Marked ready for review by @zanieb on 2025-01-17 19:12_

---

_@zanieb reviewed on 2025-01-17 19:12_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:482 on 2025-01-17 19:12_

It looks like `setup-uv` sets this and we use `XDG_BIN_HOME` in tests which `UV_TOOL_BIN_DIR` takes precedence over.

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:482 on 2025-01-17 19:12_

https://github.com/astral-sh/setup-uv/blob/3548439624127c489c1ca842eb263681394bc7b3/README.md?plain=1#L25-L26

---

_@zanieb reviewed on 2025-01-17 19:13_

---

_Review requested from @charliermarsh by @zanieb on 2025-01-17 19:58_

---

_@charliermarsh approved on 2025-01-17 20:23_

---

_Merged by @zanieb on 2025-01-21 15:02_

---

_Closed by @zanieb on 2025-01-21 15:02_

---

_Branch deleted on 2025-01-21 15:02_

---
