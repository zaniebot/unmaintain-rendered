```yaml
number: 5900
title: "`uv lock --upgrade-package` has no effect"
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-08T08:22:33Z
updated_at: 2024-08-08T13:07:03Z
url: https://github.com/astral-sh/uv/issues/5900
synced_at: 2026-01-10T04:53:49Z
```

# `uv lock --upgrade-package` has no effect

---

_Issue opened by @konstin on 2024-08-08 08:22_

Reproducer: Start with

```toml
[project]
name = "foo"
version = "1.0.0"
requires-python = "==3.11.*"
dependencies = [
  "pydantic<=2.8.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Run `uv.lock`. pydantic 2.8.0 is locked. Remove the bound:

```toml
[project]
name = "foo"
version = "1.0.0"
requires-python = "==3.11.*"
dependencies = [
  "pydantic",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Run `uv lock --upgrade-package pydantic`. Nothing happens.

Run `uv lock --upgrade`. We see the correct updates:

```
Updated pydantic v2.8.0 -> v2.8.2
Updated pydantic-core v2.20.0 -> v2.20.1
```

---

_Label `bug` added by @konstin on 2024-08-08 08:22_

---

_Label `preview` added by @konstin on 2024-08-08 08:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-08 12:19_

---

_Renamed from "`uv locik --upgrade-package` has no effect" to "`uv lock --upgrade-package` has no effect" by @charliermarsh on 2024-08-08 12:36_

---

_Closed by @charliermarsh on 2024-08-08 13:07_

---

_Closed by @charliermarsh on 2024-08-08 13:07_

---
