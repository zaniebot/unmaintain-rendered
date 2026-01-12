```yaml
number: 5859
title: "`dependencies` cannot be missing when `requires-python` is present in inline script metadata"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-07T13:19:29Z
updated_at: 2024-08-07T14:56:06Z
url: https://github.com/astral-sh/uv/issues/5859
synced_at: 2026-01-12T15:58:59Z
```

# `dependencies` cannot be missing when `requires-python` is present in inline script metadata

---

_@zanieb_

e.g.

```python
# /// script
# requires-python = ">=3.12"
# ///

pass
```

```
❯ uv run script.py
warning: `uv run` is experimental and may change without warning
error: TOML parse error at line 1, column 1
  |
1 | requires-python = ">=3.12"
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^
missing field `dependencies`
```

I honestly cannot tell if this is complaint with the [specification](https://packaging.python.org/en/latest/specifications/inline-script-metadata/#inline-script-metadata).

> This document MAY include the top-level fields dependencies and requires-python, and MAY optionally include a [tool] table.



---

_Comment by @charliermarsh on 2024-08-07 13:20_

I think we need to be robust to them being absent.

---

_Label `bug` added by @charliermarsh on 2024-08-07 13:20_

---

_Label `preview` added by @charliermarsh on 2024-08-07 13:20_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-07 14:37_

---

_Closed by @charliermarsh on 2024-08-07 14:56_

---

_Closed by @charliermarsh on 2024-08-07 14:56_

---
