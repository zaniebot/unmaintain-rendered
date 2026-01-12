```yaml
number: 18165
title: "`sys.version_info.major` not flagged by `UP036`"
type: issue
state: closed
author: mxr
labels:
  - rule
assignees: []
created_at: 2025-05-18T19:48:00Z
updated_at: 2025-06-23T20:01:56Z
url: https://github.com/astral-sh/ruff/issues/18165
synced_at: 2026-01-12T15:54:56Z
```

# `sys.version_info.major` not flagged by `UP036`

---

_@mxr_

### Summary

[`UP036`](https://docs.astral.sh/ruff/rules/outdated-version-block/) is flagging `sys.version_info >= (3,)` checks but not `sys.version_info.major >= 3` checks. Both cases should be flagged in the below example:

```console
$ cat example.py            
import sys

# flagged
if sys.version_info >= (3,):
    print("3")
else:
    print("2")

# not flagged
if sys.version_info.major >= 3:
    print("3")
else:
    print("2")
```

```console
$ ruff --verbose check --select 'UP' --target-version 'py38' --isolated example.py
[2025-05-18][15:44:40][ruff::resolve][DEBUG] Isolated mode, not reading any pyproject.toml
[2025-05-18][15:44:40][ruff::commands::check][DEBUG] Identified files to lint in: 3.107ms
[2025-05-18][15:44:40][ruff::commands::check][DEBUG] Checked 1 files in: 19.042µs
example.py:4:4: UP036 Version block is outdated for minimum Python version
  |
3 | # flagged
4 | if sys.version_info >= (3,):
  |    ^^^^^^^^^^^^^^^^^^^^^^^^ UP036
5 |     print("3")
6 | else:
  |
  = help: Remove outdated version block

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

Real-life example [here](https://github.com/deeplook/sparklines/blob/5e298bf8a99de82d3812ebc25c9267b1a018a750/sparklines/__main__.py#L18).

### Version

ruff 0.11.10

---

_Comment by @MichaReiser on 2025-05-19 07:13_

This makes sense to me. The rule already supports `sys.version_info[0]`

---

_Label `rule` added by @MichaReiser on 2025-05-19 07:13_

---

_Comment by @IDrokin117 on 2025-05-28 13:19_

@MichaReiser Can I work on this?

---

_Assigned to @IDrokin117 by @MichaReiser on 2025-05-28 13:26_

---

_Closed by @ntBre on 2025-06-23 20:01_

---
