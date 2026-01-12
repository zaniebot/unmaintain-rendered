```yaml
number: 10689
title: uv lock should not run build backend for dynamic versions
type: issue
state: closed
author: danielhollas
labels: []
assignees: []
created_at: 2025-01-16T17:40:28Z
updated_at: 2025-01-17T04:27:48Z
url: https://github.com/astral-sh/uv/issues/10689
synced_at: 2026-01-12T16:00:19Z
```

# uv lock should not run build backend for dynamic versions

---

_@danielhollas_

`uv lock` still invokes the build backend even after the project's dynamic version has been omited from uv.lock in #10622. 

We're hitting this over at https://github.com/astral-sh/uv-pre-commit/issues/35

_Originally posted by @danielhollas in https://github.com/astral-sh/uv/issues/10622#issuecomment-2595601765_
            
@charliemarsh mentioned that this is a TODO in the code, I guess it is this one:

https://github.com/astral-sh/uv/pull/10622/files#diff-94a57a0e91f87a94e015da3bd8f9e8660ce3c8564718e78ee6599a3e214965d3R1230

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-16 17:41_

---

_Closed by @charliermarsh on 2025-01-17 04:27_

---
