---
number: 449
title: "The integration tests should use `--exclude-newer` where possible"
type: issue
state: closed
author: konstin
labels:
  - internal
assignees: []
created_at: 2023-11-19T12:04:12Z
updated_at: 2023-11-19T15:10:59Z
url: https://github.com/astral-sh/uv/issues/449
synced_at: 2026-01-07T13:12:16-06:00
---

# The integration tests should use `--exclude-newer` where possible

---

_Issue opened by @konstin on 2023-11-19 12:04_

Currently, the integration tests will fail when new versions of packages are published. We should use the `--exclude-newer` option introduced in #434 to make them less flaky. Note that #434 didn't have much real world exposure yet so this requires some testing to make sure it works

---

_Comment by @charliermarsh on 2023-11-19 13:44_

I can do this.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-19 13:44_

---

_Label `internal` added by @charliermarsh on 2023-11-19 13:44_

---

_Renamed from "The intergration tests should use `--exclude-newer` where possible" to "The integration tests should use `--exclude-newer` where possible" by @konstin on 2023-11-19 13:45_

---

_Referenced in [astral-sh/uv#456](../../astral-sh/uv/pulls/456.md) on 2023-11-19 15:00_

---

_Closed by @charliermarsh on 2023-11-19 15:10_

---
