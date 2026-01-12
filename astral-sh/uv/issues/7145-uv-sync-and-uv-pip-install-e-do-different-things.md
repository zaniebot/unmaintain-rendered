```yaml
number: 7145
title: "`uv sync` and `uv pip install -e .` do different things when virtual packages are in the workspace"
type: issue
state: open
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-09-06T22:24:41Z
updated_at: 2024-11-15T10:49:56Z
url: https://github.com/astral-sh/uv/issues/7145
synced_at: 2026-01-12T15:59:10Z
```

# `uv sync` and `uv pip install -e .` do different things when virtual packages are in the workspace

---

_@charliermarsh_

`uv sync` omits them, while `uv pip install -e .` installs them as non-editable. This seems bad. At the very least, I'd say that `uv pip install -e .` should install them as editable.

---

_Label `bug` added by @charliermarsh on 2024-09-06 22:25_

---

_Comment by @charliermarsh on 2024-09-06 23:15_

\cc @zanieb for input on the correct behavior

---

_Comment by @zanieb on 2024-09-07 15:36_

Can you share a bit more details on what a reproduction looks like?

---

_Comment by @charliermarsh on 2024-09-07 15:52_

If you `uv init foo` then `cd foo` and `uv init bar`, and add `bar` as a dependency:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["bar"]

[tool.uv.workspace]
members = ["bar"]

[tool.uv.sources]
bar = { workspace = true }
```

---

_Comment by @charliermarsh on 2024-09-07 15:52_

Then `uv sync`:

```
❯ uv sync
Using Python 3.12.1
Creating virtualenv at: .venv
Resolved 2 packages in 0.81ms
Audited in 0.00ms
```

Versus `uv pip install -e .`:

```
❯ uv pip install -e .
Resolved 2 packages in 2ms
Installed 2 packages in 1ms
 + bar==0.1.0 (from file:///Users/crmarsh/workspace/puffin/foo/bar)
 + foo==0.1.0 (from file:///Users/crmarsh/workspace/puffin/foo)
```

---

_Comment by @Ruhrozz on 2024-11-15 10:49_

same problem here

---
