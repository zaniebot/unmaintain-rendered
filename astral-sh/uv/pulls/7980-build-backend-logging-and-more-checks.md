```yaml
number: 7980
title: "Build backend: Logging and more checks"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: build-backend8-logging-and-checks
created_at: 2024-10-07T18:00:23Z
updated_at: 2024-10-08T15:25:41Z
url: https://github.com/astral-sh/uv/pull/7980
synced_at: 2026-01-10T12:54:00Z
```

# Build backend: Logging and more checks

---

_Pull request opened by @konstin on 2024-10-07 18:00_

Add better logging and a missed check.

Log output (debug):

```
$ RUST_LOG=uv=debug cargo run -q build-backend build-wheel dist2
  DEBUG uv 0.4.18
  DEBUG Writing wheel at dist2/uv_backend-0.1.0-py3-none-any.whl
  DEBUG Adding content files to dist2/uv_backend-0.1.0-py3-none-any.whl
  DEBUG Adding metadata files to dist2/uv_backend-0.1.0-py3-none-any.whl
  uv_backend-0.1.0-py3-none-any.whl
```

Log output (trace):

```
$ RUST_LOG=uv=trace cargo run -q build-backend build-wheel dist2
  DEBUG uv 0.4.18
  DEBUG Writing wheel at dist2/uv_backend-0.1.0-py3-none-any.whl
  DEBUG Adding content files to dist2/uv_backend-0.1.0-py3-none-any.whl
  TRACE Adding directory uv_backend
  TRACE Adding uv_backend/__init__.py from src/uv_backend/__init__.py
  DEBUG Adding metadata files to dist2/uv_backend-0.1.0-py3-none-any.whl
  TRACE Adding directory uv_backend-0.1.0.dist-info
  TRACE Adding uv_backend-0.1.0.dist-info/WHEEL
  TRACE Adding uv_backend-0.1.0.dist-info/entry_points.txt
  TRACE Adding uv_backend-0.1.0.dist-info/METADATA
  TRACE Adding uv_backend-0.1.0.dist-info/RECORD
  TRACE Adding central directory
  uv_backend-0.1.0-py3-none-any.whl
```


---

_Label `preview` added by @konstin on 2024-10-07 18:00_

---

_Review requested from @BurntSushi by @konstin on 2024-10-07 18:00_

---

_@BurntSushi approved on 2024-10-08 14:08_

---

_Merged by @konstin on 2024-10-08 15:25_

---

_Closed by @konstin on 2024-10-08 15:25_

---

_Branch deleted on 2024-10-08 15:25_

---
