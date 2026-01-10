---
number: 5929
title: Parent uv.toml configuration not used when pyproject.toml present
type: issue
state: closed
author: ttrei
labels:
  - bug
  - configuration
assignees: []
created_at: 2024-08-08T20:23:14Z
updated_at: 2024-08-08T21:05:03Z
url: https://github.com/astral-sh/uv/issues/5929
synced_at: 2026-01-10T01:23:54Z
---

# Parent uv.toml configuration not used when pyproject.toml present

---

_Issue opened by @ttrei on 2024-08-08 20:23_

Given this project structure
```
==> uv.toml <==
[pip]
emit-index-url = true

==> foo/pyproject.toml <==
[project]
name = "foo"
dependencies = [
  "httpx",
]
```
uv should load configuration from uv.toml:
https://github.com/astral-sh/uv/blob/bf0497e652d4de8a880d047d15acd01c2de9b4ae/docs/configuration/files.md
> If a pyproject.toml file is found, uv will read configuration from the [tool.uv.pip] table. For example, to set a persistent index URL, add the following to a pyproject.toml:
> (If there is no such table, the pyproject.toml file will be ignored, and uv will continue searching in the directory hierarchy.)
> If a uv.toml file is found, uv will read from the [pip] table.

But it considers uv.toml only if pyproject.toml is not present:
```
> cd foo
> uv pip compile pyproject.toml --show-settings | grep emit_index_url
        emit_index_url: false,
> rm pyproject.toml
> uv pip compile pyproject.toml --show-settings | grep emit_index_url
        emit_index_url: true,
```

Reproduced with 0.2.12 and later versions (latest 0.2.34).
It works correctly in 0.2.11.

---

_Renamed from "Parent uv.toml configuration not read when pyproject.toml present" to "Parent uv.toml configuration not used when pyproject.toml present" by @ttrei on 2024-08-08 20:23_

---

_Comment by @charliermarsh on 2024-08-08 20:30_

I'll take a look, that seems wrong.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-08 20:30_

---

_Label `bug` added by @charliermarsh on 2024-08-08 20:30_

---

_Label `configuration` added by @charliermarsh on 2024-08-08 20:30_

---

_Referenced in [astral-sh/uv#5931](../../astral-sh/uv/pulls/5931.md) on 2024-08-08 20:49_

---

_Closed by @charliermarsh on 2024-08-08 21:05_

---

_Closed by @charliermarsh on 2024-08-08 21:05_

---
