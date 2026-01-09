---
number: 11055
title: uv pip tree --strict does report success even if conflicts are found
type: issue
state: open
author: ssbarnea
labels:
  - bug
  - needs-decision
assignees: []
created_at: 2025-01-29T09:56:53Z
updated_at: 2025-01-29T14:12:53Z
url: https://github.com/astral-sh/uv/issues/11055
synced_at: 2026-01-07T13:12:18-06:00
---

# uv pip tree --strict does report success even if conflicts are found

---

_Issue opened by @ssbarnea on 2025-01-29 09:56_

### Summary

As opposed to the pipdeptree which was looked as reference when implementing `uv pip tree` command, the result code seems to always be zero, even if `--strict` mode is used and conflicts are reported. This prevents it from replacing pipdeptree and build pipelines as it might hide conflicts.

```shell
❯ uv pip tree --strict
Using Python 3.13.1 environment at: /Users/ssbarnea/.venv
...
warning: The package `openapi-schema-validator` requires `jsonschema-specifications>=2023.5.2,<2024.0.0`, but `2024.10.1` is installed

echo $0
0
```

### Platform

macos

### Version

 0.5.25

### Python version

3.13.1

---

_Label `bug` added by @ssbarnea on 2025-01-29 09:56_

---

_Comment by @charliermarsh on 2025-01-29 13:55_

I'm unsure -- this would be a departure from (e.g.) `--strict` in the `uv pip install` CLI.

---

_Label `needs-decision` added by @zanieb on 2025-01-29 14:12_

---
