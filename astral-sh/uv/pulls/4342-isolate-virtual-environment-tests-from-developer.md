```yaml
number: 4342
title: Isolate virtual environment tests from developer toolchains
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/venv-test-isolate
created_at: 2024-06-16T15:41:47Z
updated_at: 2024-06-17T10:40:12Z
url: https://github.com/astral-sh/uv/pull/4342
synced_at: 2026-01-12T16:06:10Z
```

# Isolate virtual environment tests from developer toolchains

---

_@zanieb_

Otherwise, when testing `uv venv --preview` the default toolchain directory will leak into the test.

I believe I've made a similar change for the standard `TestContext` in another commit somewhere in my stack, if not I'll add it after.

---

_Label `internal` added by @zanieb on 2024-06-16 15:41_

---

_Label `testing` added by @zanieb on 2024-06-16 15:41_

---

_@konstin approved on 2024-06-17 08:58_

---

_Merged by @zanieb on 2024-06-17 10:40_

---

_Closed by @zanieb on 2024-06-17 10:40_

---

_Branch deleted on 2024-06-17 10:40_

---
