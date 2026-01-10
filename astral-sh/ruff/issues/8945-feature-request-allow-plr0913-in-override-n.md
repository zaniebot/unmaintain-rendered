---
number: 8945
title: "feature request: allow PLR0913 in `@override`n methods"
type: issue
state: closed
author: ZeeD
labels: []
assignees: []
created_at: 2023-12-01T10:09:20Z
updated_at: 2023-12-04T23:32:52Z
url: https://github.com/astral-sh/ruff/issues/8945
synced_at: 2026-01-10T01:22:48Z
---

# feature request: allow PLR0913 in `@override`n methods

---

_Issue opened by @ZeeD on 2023-12-01 10:09_

Basically same scenario of #8867 - in case of `@override` there is nothing I can do to simplify the signature of the method, so please ignore [too-many-arguments (PLR0913)](https://docs.astral.sh/ruff/rules/too-many-arguments/) checks.

---

_Renamed from "feature request: allow PLR0913 in `@override`d methods" to "feature request: allow PLR0913 in `@override`n methods" by @ZeeD on 2023-12-01 10:09_

---

_Referenced in [astral-sh/ruff#8954](../../astral-sh/ruff/pulls/8954.md) on 2023-12-01 18:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-01 18:18_

---

_Closed by @charliermarsh on 2023-12-01 18:22_

---

_Comment by @ZeeD on 2023-12-03 16:21_

Thanks!

---

_Comment by @g-as on 2023-12-04 22:26_

Same should apply to newly added `too-many-positional`/`PLR0917`?

---

_Comment by @charliermarsh on 2023-12-04 23:32_

Yes, sorry, just an oversight. Fixed in https://github.com/astral-sh/ruff/pull/9000.

---
