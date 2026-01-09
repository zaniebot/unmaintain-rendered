---
number: 10680
title: "In a workspace, `uv sync` does not respect some environment variables for workspace members"
type: issue
state: closed
author: wsascha
labels:
  - bug
assignees: []
created_at: 2025-01-16T14:25:38Z
updated_at: 2025-01-17T18:55:11Z
url: https://github.com/astral-sh/uv/issues/10680
synced_at: 2026-01-07T13:12:18-06:00
---

# In a workspace, `uv sync` does not respect some environment variables for workspace members

---

_Issue opened by @wsascha on 2025-01-16 14:25_

It seems that the `UV_INDEX_*_USERNAME` and `UV_INDEX_*_PASSWORD` env vars are not considered for / propagated to workspace members when calling `uv sync`.

An MWE could look like the following (given that you have an index at hand that requires auth):

### Workspace `pyproject.toml`

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["bar"]

[tool.uv.sources]
bar = { workspace = true }

[tool.uv.workspace]
members = ["packages/bar"]
```

### Member `packages/bar/pyproject.toml`

```toml
[project]
name = "bar"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["baz"]

[tool.uv.sources]
baz = { index = "my-index" }

[[tool.uv.index]]
name = "my-index"
# url = "https://<USERNAME>:<PASSWORD>@example.com/simple"
url = "https://example.com/simple"
explicit = true
```

If I explicitly provide `<USERNAME>` and `<PASSWORD>` in the URL (commented out line) the "baz" project can be retrieved.

If I set the URL as is and provide the env vars as in

```sh
UV_INDEX_MY_INDEX_USERNAME=<USERNAME> UV_INDEX_MY_INDEX_PASSWORD=<PASSWORD> uv sync
```

the "baz" project download fails with `401 Unauthorized`.

### Workaround

If I explicitly set the index in the workspace `pyproject.toml`, adding

```toml
[[tool.uv.index]]
name = "my-index"
url = "https://example.com/simple"
explicit = true
```

the project can be retrieved.

### Conclusion

I think this is undesired behavior, thus opening this issue.

Thanks for your great work, I really love `uv`.

---

_Comment by @charliermarsh on 2025-01-16 15:15_

I can try it out.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-16 15:15_

---

_Label `bug` added by @charliermarsh on 2025-01-16 15:20_

---

_Referenced in [astral-sh/uv#10688](../../astral-sh/uv/pulls/10688.md) on 2025-01-16 17:38_

---

_Closed by @charliermarsh on 2025-01-17 18:55_

---

_Closed by @charliermarsh on 2025-01-17 18:55_

---
