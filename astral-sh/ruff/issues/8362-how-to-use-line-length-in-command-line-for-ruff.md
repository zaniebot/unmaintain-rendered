```yaml
number: 8362
title: How to use line-length in command line for ruff format?
type: issue
state: closed
author: alanwilter
labels: []
assignees: []
created_at: 2023-10-30T17:49:52Z
updated_at: 2023-11-02T01:39:53Z
url: https://github.com/astral-sh/ruff/issues/8362
synced_at: 2026-01-10T11:09:50Z
```

# How to use line-length in command line for ruff format?

---

_Issue opened by @alanwilter on 2023-10-30 17:49_

Plain simple, I want to use ruff format and I don't have any toml configuration.

```bash
echo 'aa = [ "blablabla", "blablabla","blablabla","blablabla","blablabla","blablabla","blablabla","blablabla","blablabla","blablabla"]' > test.py
ruff check test.py --select ALL --line-length=120

warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
test.py:1:1: D100 Missing docstring in public module
test.py:1:121: E501 Line too long (128 > 120)
Found 2 errors.
(```

But

```bash
ruff format test.py --line-length=120

error: unexpected argument '--line-length' found

  tip: to pass '--line-length' as a value, use '-- --line-length'

Usage: ruff format <FILES|--check|--diff|--config <CONFIG>|--no-cache|--cache-dir <CACHE_DIR>|--respect-gitignore|--no-respect-gitignore|--exclude <FILE_PATTERN>|--force-exclude|--no-force-exclude|--isolated|--stdin-filename <STDIN_FILENAME>|--target-version <TARGET_VERSION>|--preview|--no-preview>

For more information, try '--help'.
```

If using `ruff format test.py` it will use the default 88.


---

_Comment by @zanieb on 2023-10-30 17:57_

Duplicate of https://github.com/astral-sh/ruff/issues/8352

---

_Closed by @zanieb on 2023-10-30 17:57_

---

_Closed by @zanieb on 2023-11-02 01:39_

---
