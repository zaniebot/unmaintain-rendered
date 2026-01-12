```yaml
number: 740
title: Add spans to all significant tasks
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/spans-spans-spans
created_at: 2023-12-31T12:14:04Z
updated_at: 2024-01-02T16:17:04Z
url: https://github.com/astral-sh/uv/pull/740
synced_at: 2026-01-12T16:04:09Z
```

# Add spans to all significant tasks

---

_@konstin_

I've tried to investigate puffin's performance wrt to builds and parallelism in general, but found the previous instrumentation to granular. I've tried to add spans to every function that either needs noticeable io or cpu resources without creating duplication. This also fixes some wrong tracing usage on async functions (https://docs.rs/tracing/latest/tracing/struct.Span.html#in-asynchronous-code) and some spans that weren't actually entered.

---

_@charliermarsh approved on 2024-01-02 14:54_

---

_Label `internal` added by @charliermarsh on 2024-01-02 14:54_

---

_Review comment by @zanieb on `crates/puffin-build/src/lib.rs`:461 on 2024-01-02 14:55_

Just wondering, why don't we use `run_python_script` here?

---

_@zanieb reviewed on 2024-01-02 14:55_

---

_@konstin reviewed on 2024-01-02 15:52_

---

_Review comment by @konstin on `crates/puffin-build/src/lib.rs`:461 on 2024-01-02 15:52_

`run_python_script` uses `python -c <str>` while this runs `python setup.py bdist_wheel`, but yeah we're lacking the env vars here.

---

_@zanieb reviewed on 2024-01-02 16:05_

---

_Review comment by @zanieb on `crates/puffin-build/src/lib.rs`:461 on 2024-01-02 16:05_

👍 

---

_Merged by @konstin on 2024-01-02 16:17_

---

_Closed by @konstin on 2024-01-02 16:17_

---

_Branch deleted on 2024-01-02 16:17_

---
