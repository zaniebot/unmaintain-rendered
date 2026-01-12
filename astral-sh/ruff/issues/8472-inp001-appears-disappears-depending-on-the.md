```yaml
number: 8472
title: INP001 appears/disappears depending on the current directory
type: issue
state: closed
author: DimitriPapadopoulos
labels:
  - question
assignees: []
created_at: 2023-11-03T13:42:44Z
updated_at: 2023-11-09T04:13:26Z
url: https://github.com/astral-sh/ruff/issues/8472
synced_at: 2026-01-12T15:54:48Z
```

# INP001 appears/disappears depending on the current directory

---

_@DimitriPapadopoulos_

When checking a single file, the [INP001](https://docs.astral.sh/ruff/rules/implicit-namespace-package/) error message appears/disappears depending on whether we run ruff from the directory containing the Python file or not:
* ```
   """Documentation."""
   ```
* ```
  $ cd /
  $ 
  $ ruff --no-cache --isolated --select INP001 /path/to/file.py 
  path/to/file.py:1:1: INP001 File `path/to/file.py` is part of an implicit namespace package. Add an `__init__.py`.
  Found 1 error.
  $ 
  $ 
  $ cd /path
  $ 
  $ ruff --no-cache --isolated --select INP001 /path/to/file.py 
  to/file.py:1:1: INP001 File `to/file.py` is part of an implicit namespace package. Add an `__init__.py`.
  Found 1 error.
  $ 
  $ cd /path/to
  $ 
  $ ruff --no-cache --isolated --select INP001 /path/to/file.py 
  $ ruff --no-cache --isolated --select INP001 file.py 
  $ 
  ```
* ```
  $ ruff --version
  ruff 0.1.3
  $ 
  ```
I find this behaviour a little bit disturbing. Is it expected?

---

_Comment by @charliermarsh on 2023-11-03 16:38_

Yeah, we avoid flagging files that are _direct_ children of the source directory, since these tend to be top-level scripts, like `manage.py`.

But I think you shouldn't see this behavior if you use a `pyproject.toml`, since we'll then detect the source directory correctly no matter where you run Ruff from. Ruff only becomes "non-portable" in this way when you don't have a `pyproject.toml` or `ruff.toml` to detect the project root -- without those, we have to fall back to the current working directory as a best guess.

---

_Label `question` added by @charliermarsh on 2023-11-03 16:38_

---

_Comment by @DimitriPapadopoulos on 2023-11-03 16:49_

I was running ruff on a standalone Python script, so you're correct I don't have a `pyproject.toml` or `ruff.toml` file.

---

_Comment by @DimitriPapadopoulos on 2023-11-03 16:59_

The error message is disturbing because INP001 is irrelevant for standalone scripts, and even more disturbing as it appears/disappears based on the above algorithm. However, I understand identifying a "standalone" script might not be possible.

Selecting INP001 for a standalone script is a bad idea in the first place, of course, but I was initially using  `--select ALL`, not  `--select INP001`.

---

_Comment by @charliermarsh on 2023-11-09 04:13_

I think I'm gonna close as "working as intended". It's just hard for us to do much better here, and the inverse would also cause confusion.

---

_Closed by @charliermarsh on 2023-11-09 04:13_

---
