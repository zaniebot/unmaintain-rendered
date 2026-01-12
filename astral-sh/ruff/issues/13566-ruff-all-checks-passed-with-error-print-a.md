```yaml
number: 13566
title: "ruff All checks passed! with error  `print.a()`"
type: issue
state: closed
author: ArtemIsmagilov
labels: []
assignees: []
created_at: 2024-09-30T07:14:38Z
updated_at: 2024-09-30T11:49:11Z
url: https://github.com/astral-sh/ruff/issues/13566
synced_at: 2026-01-12T15:54:53Z
```

# ruff All checks passed! with error  `print.a()`

---

_@ArtemIsmagilov_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
- code
  a.py
  ```python
  print.a()
  ```
  ruff
  ```bash
  ruff check a.py 
  All checks passed!
  ```
  pyright
  ```bash
  pyright a.py 
  Traceback (most recent call last):
  File "a.py", line 1, in <module>
    print.a()
    ^^^^^^^
  AttributeError: 'builtin_function_or_method' object has no attribute 'a'
  ```
- pyproject.toml by default
- ruff 0.6.5

Why does ruff miss this?

---

_Renamed from "ruff All checks passed! with error syntax  `print.a()`" to "ruff All checks passed! with error  `print.a()`" by @AlexWaygood on 2024-09-30 08:10_

---

_Comment by @AlexWaygood on 2024-09-30 11:49_

Hi, thanks for the issue! 

> * pyright
>       ```shell
>       pyright a.py 
>       Traceback (most recent call last):
>       File "a.py", line 1, in <module>
>         print.a()
>         ^^^^^^^
>       AttributeError: 'builtin_function_or_method' object has no attribute 'a'
>       ```

Hmm, that looks like an exception raised at runtime rather than a diagnostic that pyright would emit. Regardless, it's true that there are many errors that a type checker will catch but Ruff will currently not catch. Type checkers and linters are different kinds of tools that catch different kinds of errors; it's good to use both tools together rather than relying on one or the other.

The good news is that we're working on a type checking tool! You can follow our progress by watching commits to the various `red_knot_*` crates in Ruff. It'll be a while until it's ready, however; until then, I'd recommend that you use a type checker alongside Ruff to catch a broad range of statically detectable errors.

---

_Closed by @AlexWaygood on 2024-09-30 11:49_

---
