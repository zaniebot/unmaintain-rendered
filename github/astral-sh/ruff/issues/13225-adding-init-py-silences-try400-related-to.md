---
number: 13225
title: "Adding `__init__.py` silences `TRY400` (related to relative import)"
type: issue
state: open
author: DimitriPapadopoulos
labels:
  - bug
assignees: []
created_at: 2024-09-03T08:44:53Z
updated_at: 2024-09-05T07:07:23Z
url: https://github.com/astral-sh/ruff/issues/13225
synced_at: 2026-01-07T13:12:15-06:00
---

# Adding `__init__.py` silences `TRY400` (related to relative import)

---

_Issue opened by @DimitriPapadopoulos on 2024-09-03 08:44_

Consider the following directory:
```
.
└── distutils
    ├── _log.py
    └── _msvccompiler.py
```
with:
* **`distutils/_log.py`**
  ```python
  import logging
  
  log = logging.getLogger()
  ```
* **`distutils/_msvccompiler.py`**
  ```python
  from ._log import log
  
  try:
      i = 1 / 0
  except DivisionByZero as e:
      log.error(f"raised {e}")
  ````

These files generate a `TRY400` error:
```console
$ ruff check --select TRY400 --isolated --no-cache
distutils/_msvccompiler.py:6:5: TRY400 Use `logging.exception` instead of `logging.error`
  |
4 |     i = 1 / 0
5 | except DivisionByZero as e:
6 |     log.error(f"raised {e}")
  |     ^^^^^^^^^^^^^^^^^^^^^^^^ TRY400
  |
  = help: Replace with `exception`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
$ 
```

Then add an empty `__init__.py` file:
```
.
└── distutils
    ├── __init__.py
    ├── _log.py
    └── _msvccompiler.py
```

It silences the `TRY400` error, presumably because `log` is not identified as a [`logging.Logger`](https://docs.python.org/3/library/logging.html#logging.Logger) any more:
```console
$ ruff check --select TRY400 --isolated --no-cache
All checks passed!
$ 
```

I may be missing something about Python imports, but I don't understand why ruff fails to take into account the relative import with `__init__.py`.

* List of keywords you searched for before creating this issue:
   "\_\_init\_\_.py", "relative import"
* The current Ruff version:
   `ruff 0.6.3`

**EDIT**: added `--no-cache` to hide **additional complexity** related to the cache

---

_Referenced in [pypa/distutils#292](../../pypa/distutils/issues/292.md) on 2024-09-03 08:47_

---

_Renamed from "Adding an __init__.py file silences error (related to relative import)" to "Adding `__init__.py` silences `TRY400` (related to relative import)" by @DimitriPapadopoulos on 2024-09-03 08:48_

---

_Comment by @dhruvmanila on 2024-09-04 05:26_

This is a weird bug. I think the problem might be in the cache.

```console
❯ ruff check --select=TRY400 .
_msvcompiler.py:6:5: TRY400 Use `logging.exception` instead of `logging.error`
  |
4 |     i = 1 / 0
5 | except DivisionByZero as e:
6 |     log.error(f"raised {e}")
  |     ^^^^^^^^^^^^^^^^^^^^^^^^ TRY400
  |
  = help: Replace with `exception`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

Now, I added the `__init__.py` file.

```console
❯ ruff check --select=TRY400 .
_msvcompiler.py:6:5: TRY400 Use `logging.exception` instead of `logging.error`
  |
4 |     i = 1 / 0
5 | except DivisionByZero as e:
6 |     log.error(f"raised {e}")
  |     ^^^^^^^^^^^^^^^^^^^^^^^^ TRY400
  |
  = help: Replace with `exception`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).

❯ ruff check --select=TRY .   
All checks passed!

❯ ruff check --select=TRY400 .
_msvcompiler.py:6:5: TRY400 Use `logging.exception` instead of `logging.error`
  |
4 |     i = 1 / 0
5 | except DivisionByZero as e:
6 |     log.error(f"raised {e}")
  |     ^^^^^^^^^^^^^^^^^^^^^^^^ TRY400
  |
  = help: Replace with `exception`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).

❯ ruff check --select=TRY400 --preview . 
All checks passed!

❯ ruff check --select=TRY400 --isolated .
_msvcompiler.py:6:5: TRY400 Use `logging.exception` instead of `logging.error`
  |
4 |     i = 1 / 0
5 | except DivisionByZero as e:
6 |     log.error(f"raised {e}")
  |     ^^^^^^^^^^^^^^^^^^^^^^^^ TRY400
  |
  = help: Replace with `exception`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

There's no config file in the directory nor any of it's parent directories.

---

_Label `bug` added by @dhruvmanila on 2024-09-04 05:26_

---

_Comment by @DimitriPapadopoulos on 2024-09-04 05:55_

To be clear, in my example, I always delete the cache with `rm -r .ruff_cache` before running `ruff check`. But yes, the cache adds additional complexity, as the last result stored in the cache has an influence on the new result.

There may be two bugs here, the first one related to relative imports and `__init__.py`, and the second one related to the effect of cache contents.

---

_Comment by @DimitriPapadopoulos on 2024-09-04 06:06_

I took the liberty to edit the initial description to show the bug independently of the effect of the cache, changing:
```console
$ ruff check --select TRY400
```
to:
```console
$ rm -r .ruff_cache
$ 
$ ruff check --select TRY400 --isolated
```

---

_Comment by @dhruvmanila on 2024-09-04 06:13_

I think you meant `--no-cache` instead of `--isolated` ?

---

_Comment by @DimitriPapadopoulos on 2024-09-04 06:41_

I was unaware of `--no-cache`, I'll add it.

---

_Label `multifile-analysis` added by @MichaReiser on 2024-09-05 06:32_

---

_Comment by @MichaReiser on 2024-09-05 06:38_

The fact that ruff doesn't invalidate the cache when adding/removing an `__init__.py` is a known limitation, see https://github.com/astral-sh/ruff/issues/5449. 

Before adding an `__init__.py`,`distutils` is a namespace package and Ruff seems to ignore the `log` import? Adding the `__init__.py` makes `distutils` a regular package and Ruff now correctly handles the `log` import, except that it can't tell that `_log.log` is an alias for `log` because Ruff has no multifile analysis support.

---

_Comment by @MichaReiser on 2024-09-05 06:57_

Looking at the code:

* Without an `__init__.py` we end up in https://github.com/astral-sh/ruff/blob/b15e9e6e059b79f2422119ba9b2d5e2be70852f6/crates/ruff_python_semantic/src/analyze/logging.rs#L58-L72
* With an `__init__.py` we end up in https://github.com/astral-sh/ruff/blob/b15e9e6e059b79f2422119ba9b2d5e2be70852f6/crates/ruff_python_semantic/src/analyze/logging.rs#L38-L56



---

_Comment by @MichaReiser on 2024-09-05 07:07_

Okay. The "root cause" is that `self.module.qualified_name` returns `None` for a namespace package 

https://github.com/astral-sh/ruff/blob/58c641c92faf345aa1065659316af41734c135c7/crates/ruff_python_semantic/src/model.rs#L827-L831

The package comes all the way from

https://github.com/astral-sh/ruff/blob/73160dc8b68428de19f8ee599e224455f49886e4/crates/ruff/src/commands/check.rs#L53-L58



---

_Label `multifile-analysis` removed by @MichaReiser on 2024-09-05 07:07_

---

_Referenced in [astral-sh/ruff#13519](../../astral-sh/ruff/issues/13519.md) on 2024-09-26 06:23_

---
