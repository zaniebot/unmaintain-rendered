```yaml
number: 6627
title: "Different errors being reported on the same file depending on whether parsing from `stdin` or not"
type: issue
state: closed
author: bryanforbes
labels:
  - bug
assignees: []
created_at: 2023-08-16T18:04:39Z
updated_at: 2023-08-16T20:34:00Z
url: https://github.com/astral-sh/ruff/issues/6627
synced_at: 2026-01-12T15:54:46Z
```

# Different errors being reported on the same file depending on whether parsing from `stdin` or not

---

_@bryanforbes_

Given the following file named `foo.pyi`:

```python
from pathlib import Path

def get_home_directory() -> Path: ...
```

`ruff` reports different errors depending on whether it's being parsed from `stdin`. The following reports no errors:

```
ruff --force-exclude --no-cache --no-fix --quiet --format json --isolated --select TCH asyncpg-stubs/compat.pyi
```

And the following reports an error in the first line:

```
cat foo.pyi | ruff --force-exclude --no-cache --no-fix --quiet --format json --isolated --select TCH --stdin-filename path/to/foo.pyi -
```

The reason this is coming up is because `ruff-lsp` runs `ruff` similar to how the second command runs and is reporting errors in my stubs project.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-16 18:08_

---

_Label `bug` added by @charliermarsh on 2023-08-16 18:09_

---

_Closed by @charliermarsh on 2023-08-16 20:34_

---
