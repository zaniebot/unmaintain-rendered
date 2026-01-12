```yaml
number: 6977
title: "Consider requiring explicit `VIRTUAL_ENV` for `uv run` tests"
type: issue
state: open
author: zanieb
labels:
  - internal
  - testing
assignees: []
created_at: 2024-09-03T19:34:56Z
updated_at: 2024-09-03T19:35:01Z
url: https://github.com/astral-sh/uv/issues/6977
synced_at: 2026-01-12T15:59:09Z
```

# Consider requiring explicit `VIRTUAL_ENV` for `uv run` tests

---

_@zanieb_

Right now, the `uv run` tests set `VIRTUAL_ENV` by default but other project commands do not (#6976).

Following https://github.com/astral-sh/uv/pull/6864, this means we'll see some warnings in snapshots. We should considering not setting it by default and instead setting it explicitly in the relevant `uv run` tests.

---

_Label `internal` added by @zanieb on 2024-09-03 19:35_

---

_Label `testing` added by @zanieb on 2024-09-03 19:35_

---
