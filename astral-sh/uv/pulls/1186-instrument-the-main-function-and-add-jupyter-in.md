```yaml
number: 1186
title: Instrument the main function and add jupyter.in
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/jupyter-in-and-main-instrumentation
created_at: 2024-01-30T11:01:18Z
updated_at: 2024-01-30T11:03:25Z
url: https://github.com/astral-sh/uv/pull/1186
synced_at: 2026-01-10T15:39:03Z
```

# Instrument the main function and add jupyter.in

---

_Pull request opened by @konstin on 2024-01-30 11:01_

Instrument the main function as anchor span for checking overhead and update tracing-durations-export to 0.2.0 for differentiating blocking/non-blocking tasks.

Add a `jupyter.in` requirement since `pip install jupyter` is a common operation. I tried `jupyterlab` too but there is no difference in performance (1.00 ± 0.07).

---

_Label `internal` added by @konstin on 2024-01-30 11:01_

---

_Merged by @konstin on 2024-01-30 11:03_

---

_Closed by @konstin on 2024-01-30 11:03_

---

_Branch deleted on 2024-01-30 11:03_

---
