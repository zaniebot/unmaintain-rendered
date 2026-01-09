---
number: 20460
title: "`ruff check --fix` to stdout when reading from stdin"
type: issue
state: open
author: pelson
labels:
  - cli
  - wish
assignees: []
created_at: 2025-09-18T03:26:57Z
updated_at: 2025-10-23T10:55:40Z
url: https://github.com/astral-sh/ruff/issues/20460
synced_at: 2026-01-07T13:12:16-06:00
---

# `ruff check --fix` to stdout when reading from stdin

---

_Issue opened by @pelson on 2025-09-18 03:26_

### Summary

It looks like when running `ruff` with `--fix` and stdin input, the fixed code is not written to stdout - only a summary message is shown. For example:

```
$ echo "import os, sys" | ruff check --fix --stdin-filename example.py -
Found 3 errors (3 fixed, 0 remaining).
```

I would expect that when reading from stdin, `ruff` could emit the fixed code to stdout, similar to how `ruff format -` behaves. I tried the `--output-file` flag, but got the summary rather than the changes (plus I couldn't specify stdout there...).

There were a few other relate discussions, but not covering the `--fix` and stdout expectation: #17307 #13690, 

I discovered this whilst trying to prepare a simple reproducer for a fixer issue, and assumed that chaining via pipes between `format` and `check` would be fine.

### Version

_No response_

---

_Referenced in [astral-sh/ruff#2389](../../astral-sh/ruff/issues/2389.md) on 2025-09-18 03:33_

---

_Label `cli` added by @MichaReiser on 2025-09-18 07:01_

---

_Label `wish` added by @MichaReiser on 2025-09-18 07:01_

---

_Comment by @MichaReiser on 2025-09-18 07:02_

Thanks for the nice write up. 

I guess that simply hasn't come up yet. I quickly checked if ruff-lsp had some clever way of doing this (it calls `ruff check -` internally to get the file diagnostics) but it uses the JSON output and applies the edits manually (or more precisely, it forwards the edits to the editor that applies them)

---

_Comment by @spaceone on 2025-10-23 10:55_

Workaround using bash:

```bash
$ ruff check --isolated --diff --fix --extend-select UP034 <(echo 'import os; str( u"", errors=("strict"))')
--- /dev/fd/63
+++ /dev/fd/63
@@ -1 +1 @@
-import os; str( u"", errors=("strict"))
+str( u"", errors=("strict"))

Would fix 1 error.

```

---
