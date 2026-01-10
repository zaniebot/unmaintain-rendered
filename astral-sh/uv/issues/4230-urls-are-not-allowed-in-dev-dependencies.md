```yaml
number: 4230
title: Urls are not allowed in dev dependencies
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-06-11T14:28:14Z
updated_at: 2024-06-11T15:30:36Z
url: https://github.com/astral-sh/uv/issues/4230
synced_at: 2026-01-10T05:31:37Z
```

# Urls are not allowed in dev dependencies

---

_Issue opened by @konstin on 2024-06-11 14:28_

The following fails:

```toml
[project]
name = "foo"
version = "0.4.0"
requires-python = ">=3.12"

[tool.uv]
dev-dependencies = [
  "tqdm @ https://files.pythonhosted.org/packages/18/eb/fdb7eb9e48b7b02554e1664afd3bd3f117f6b6d6c5881438a0b055554f9b/tqdm-4.66.4-py3-none-any.whl",
]

```

```
$ uv lock --preview
error: Package `tqdm` attempted to resolve via URL: https://files.pythonhosted.org/packages/18/eb/fdb7eb9e48b7b02554e1664afd3bd3f117f6b6d6c5881438a0b055554f9b/tqdm-4.66.4-py3-none-any.whl. URL dependencies must be expressed as direct requirements or constraints. Consider adding `tqdm @ https://files.pythonhosted.org/packages/18/eb/fdb7eb9e48b7b02554e1664afd3bd3f117f6b6d6c5881438a0b055554f9b/tqdm-4.66.4-py3-none-any.whl` to your dependencies or constraints file.
```

I assume the the lookahead resolver is missing for dev deps.

Meanwhile, this works:

```toml
[project]
name = "foo"
version = "0.4.0"
requires-python = ">=3.12"
dependencies = [
  "tqdm >=4.66.4,<4.67",
]

[tool.uv.sources]
tqdm = { url = "https://files.pythonhosted.org/packages/18/eb/fdb7eb9e48b7b02554e1664afd3bd3f117f6b6d6c5881438a0b055554f9b/tqdm-4.66.4-py3-none-any.whl" }
```

---

_Label `bug` added by @konstin on 2024-06-11 14:28_

---

_Label `preview` added by @konstin on 2024-06-11 14:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-11 14:37_

---

_Comment by @charliermarsh on 2024-06-11 14:37_

Thx.

---

_Closed by @charliermarsh on 2024-06-11 15:30_

---
