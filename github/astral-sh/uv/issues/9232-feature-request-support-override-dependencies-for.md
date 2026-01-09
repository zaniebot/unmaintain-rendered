---
number: 9232
title: "feature request: support `override-dependencies` for inline scripts"
type: issue
state: closed
author: Fogapod
labels:
  - bug
assignees: []
created_at: 2024-11-19T16:04:00Z
updated_at: 2024-11-19T16:08:20Z
url: https://github.com/astral-sh/uv/issues/9232
synced_at: 2026-01-07T13:12:18-06:00
---

# feature request: support `override-dependencies` for inline scripts

---

_Issue opened by @Fogapod on 2024-11-19 16:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

uv: `0.5.2`
According to docs inline scripts support `[tool.uv]` settings but override-dependencies doesn't work for me:
```
error: TOML parse error at line 7, column 1
  |
7 | [tool.uv]
  | ^^^^^^^^^
unknown field `override-dependencies`
```

reproducer:
```python
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "pymilvus",
# ]
#
# [tool.uv]
# override-dependencies = [
#     "milvus-lite; sys_platform == 'never'",
# ]
# ///
```


---

_Comment by @charliermarsh on 2024-11-19 16:08_

This is fixed in v0.5.3, which hasn't gone out yet: https://github.com/astral-sh/uv/pull/9229

---

_Closed by @charliermarsh on 2024-11-19 16:08_

---

_Label `bug` added by @charliermarsh on 2024-11-19 16:08_

---

_Comment by @charliermarsh on 2024-11-19 16:08_

Relevant PR is: https://github.com/astral-sh/uv/pull/9162

---
