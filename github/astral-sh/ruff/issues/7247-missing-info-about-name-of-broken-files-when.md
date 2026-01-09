---
number: 7247
title: "Missing info about name of broken files when using `ruff format`"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - cli
  - formatter
assignees: []
created_at: 2023-09-08T18:06:20Z
updated_at: 2023-09-14T15:44:18Z
url: https://github.com/astral-sh/ruff/issues/7247
synced_at: 2026-01-07T13:12:15-06:00
---

# Missing info about name of broken files when using `ruff format`

---

_Issue opened by @qarmin on 2023-09-08 18:06_

Ruff 0.0.287 (latest changes from main branch)

Ruff check
```
ruff .
```
contains in error info about file
```
error: Failed to parse Broken/PY_FILE_TEST_11944308565.py:1:1: Unexpected token Indent
```
```
warning: Linting panicked A/Untitled Folder/295_IDX_0_RAND_65295806300875131446962.py: This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'called `Result::unwrap()` on an `Err` value: TryFromIntError(())', crates/ruff/src/noqa.rs:132:59
```

but 
```
ruff format . --check
```
do not contains info about files
```
error: error=source contains syntax errors: ParseError { error: UnrecognizedToken(Async, Some("name")), offset: 787, source_path: "<filename>" }
```
```
error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread '<unnamed>' panicked at 'slice index starts at 2 but ends at 1', crates/ruff_python_formatter/src/expression/binary_like.rs:472:27
```

---

_Renamed from "Missing info about broken files when using `ruff format`" to "Missing info about name of broken files when using `ruff format`" by @qarmin on 2023-09-08 18:24_

---

_Label `bug` added by @MichaReiser on 2023-09-08 18:37_

---

_Label `formatter` added by @MichaReiser on 2023-09-08 18:37_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-08 18:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-12 02:18_

---

_Label `cli` added by @MichaReiser on 2023-09-13 07:37_

---

_Referenced in [astral-sh/ruff#7377](../../astral-sh/ruff/pulls/7377.md) on 2023-09-14 02:26_

---

_Closed by @charliermarsh on 2023-09-14 15:44_

---
