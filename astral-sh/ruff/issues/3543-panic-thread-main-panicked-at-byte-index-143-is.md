```yaml
number: 3543
title: "[Panic] thread 'main' panicked at 'byte index 143 is not a char boundary; when scanning file"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-15T17:07:01Z
updated_at: 2023-03-19T18:16:52Z
url: https://github.com/astral-sh/ruff/issues/3543
synced_at: 2026-01-12T15:54:43Z
```

# [Panic] thread 'main' panicked at 'byte index 143 is not a char boundary; when scanning file

---

_@qarmin_

Ruff 0.0.255

command
```
ruff .
```

crash
```
error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at 'byte index 143 is not a char boundary; it is inside '\u{2ecdf}' (bytes 140..144) of `"""SCons.Tool.python

Regi""assert""con"]" tinue";"sters"except"  the Python scanner for the suppor󂩢ed Python source file suffixes.

""""𮳟s"`', crates/ruff_python_ast/src/str.rs:27:21
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Aborted (core dumped)
```
[4621382747python30.py.zip](https://github.com/charliermarsh/ruff/files/10982790/4621382747python30.py.zip)



---

_Label `bug` added by @charliermarsh on 2023-03-15 23:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-17 19:32_

---

_Closed by @charliermarsh on 2023-03-19 18:16_

---
