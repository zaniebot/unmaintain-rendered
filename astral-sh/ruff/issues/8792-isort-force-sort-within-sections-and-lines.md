```yaml
number: 8792
title: "[isort] force-sort-within-sections and lines-between-types should probably be incompatible"
type: issue
state: closed
author: bluthej
labels:
  - bug
  - isort
assignees: []
created_at: 2023-11-20T19:11:30Z
updated_at: 2023-12-07T04:56:15Z
url: https://github.com/astral-sh/ruff/issues/8792
synced_at: 2026-01-10T11:09:51Z
```

# [isort] force-sort-within-sections and lines-between-types should probably be incompatible

---

_Issue opened by @bluthej on 2023-11-20 19:11_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Current behavior

When setting force-sort-within-sections to `true` and lines-between-types to 1, Ruff adds a blank line before the first from import that appears after a straight import.

## (Presumably) desired behavior

isort does not add blank lines in this situation, which I think makes sense, so I propose to stick to that behavior. I guess the force-sort-within-sections setting should simply override the lines-between-types setting.

## Example

I tested isort with `force_sort_within_sections` and `lines_between_types = 1` against the following example:
```python
from a import x
import b
from c import y
import d
```
and it doesn't do anything. Here is the command I ran (isort version 5.12.0):
```shell
isort --fss --lbt 1 --stdout example.py
```

However, running `cargo r -p ruff_cli -- check example.py --no-cache --diff`  adds a blank line after `import b` with the following config:
```toml
select = ["I001"]

[isort]
force-sort-within-sections = true
lines-between-types = 1
```

---

_Label `bug` added by @charliermarsh on 2023-11-21 00:24_

---

_Label `isort` added by @charliermarsh on 2023-11-21 00:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-07 03:56_

---

_Closed by @charliermarsh on 2023-12-07 04:56_

---
