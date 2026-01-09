---
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
synced_at: 2026-01-07T13:12:18-06:00
---

# uv lock should not run build backend for dynamic versions

---

_Issue opened by @danielhollas on 2025-01-16 17:40_

`uv lock` still invokes the build backend even after the project's dynamic version has been omited from uv.lock in #10622. 

We're hitting this over at https://github.com/astral-sh/uv-pre-commit/issues/35

_Originally posted by @danielhollas in https://github.com/astral-sh/uv/issues/10622#issuecomment-2595601765_
            
@charliemarsh mentioned that this is a TODO in the code, I guess it is this one:

https://github.com/astral-sh/uv/pull/10622/files#diff-94a57a0e91f87a94e015da3bd8f9e8660ce3c8564718e78ee6599a3e214965d3R1230

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-16 17:41_

---

_Referenced in [astral-sh/uv-pre-commit#35](../../astral-sh/uv-pre-commit/issues/35.md) on 2025-01-16 17:42_

---

_Referenced in [aiidateam/aiida-core#6699](../../aiidateam/aiida-core/pulls/6699.md) on 2025-01-16 18:01_

---

_Referenced in [astral-sh/uv#10703](../../astral-sh/uv/pulls/10703.md) on 2025-01-17 04:00_

---

_Closed by @charliermarsh on 2025-01-17 04:27_

---

_Referenced in [aiidateam/aiida-core#6711](../../aiidateam/aiida-core/pulls/6711.md) on 2025-01-17 14:02_

---

_Referenced in [vega/altair#3776](../../vega/altair/pulls/3776.md) on 2025-01-18 13:46_

---
