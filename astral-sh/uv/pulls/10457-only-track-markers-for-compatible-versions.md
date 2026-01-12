```yaml
number: 10457
title: Only track markers for compatible versions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/implied
created_at: 2025-01-10T03:43:12Z
updated_at: 2025-01-10T13:10:29Z
url: https://github.com/astral-sh/uv/pull/10457
synced_at: 2026-01-12T16:09:18Z
```

# Only track markers for compatible versions

---

_@charliermarsh_

## Summary

We shouldn't consider incompatible distributions (e.g., those that don't match the required Python version) when determining the implied markers.

---

_Label `internal` added by @charliermarsh on 2025-01-10 03:43_

---

_Marked ready for review by @charliermarsh on 2025-01-10 03:43_

---

_Label `internal` removed by @charliermarsh on 2025-01-10 03:43_

---

_Label `bug` added by @charliermarsh on 2025-01-10 03:43_

---

_Converted to draft by @charliermarsh on 2025-01-10 04:02_

---

_Comment by @charliermarsh on 2025-01-10 04:03_

Interesting, need to revisit tomorrow.

---

_Comment by @charliermarsh on 2025-01-10 04:05_

Oh, I think these changes are right...

---

_@charliermarsh reviewed on 2025-01-10 04:10_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:7634 on 2025-01-10 04:10_

This is universal, but for Python 3.12. And while there _are_ Windows wheels for `torchvision===0.15.1`, there are no Python 3.12 wheels... So it's _not_ correct to fall back to them on Windows.

---

_Marked ready for review by @charliermarsh on 2025-01-10 04:10_

---

_Merged by @charliermarsh on 2025-01-10 13:10_

---

_Closed by @charliermarsh on 2025-01-10 13:10_

---

_Branch deleted on 2025-01-10 13:10_

---
