```yaml
number: 7319
title: "Formatter undocumented deviation: condition and slice wrapping"
type: issue
state: closed
author: JonathanPlasse
labels:
  - formatter
assignees: []
created_at: 2023-09-12T21:04:31Z
updated_at: 2023-09-14T08:43:55Z
url: https://github.com/astral-sh/ruff/issues/7319
synced_at: 2026-01-10T11:09:49Z
```

# Formatter undocumented deviation: condition and slice wrapping

---

_Issue opened by @JonathanPlasse on 2023-09-12 21:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Black formatting
```python
def f():
    return (
        package_version is not None
        and package_version.split(".")[:2] == package_info.version.split(".")[:2]
    )
```
Ruff formatting
```python
def f():
    return package_version is not None and package_version.split(".")[
        :2
    ] == package_info.version.split(".")[:2]
```
I use Ruff 0.0.289 with line length 100.

In this case, I prefer Black.

---

_Label `formatter` added by @MichaReiser on 2023-09-13 06:42_

---

_Label `waiting-on-author` added by @MichaReiser on 2023-09-13 06:42_

---

_Comment by @MichaReiser on 2023-09-13 06:43_

Thanks for reporting this deviation. I'm, unfortunately, unable to reproduce the deviation. Both Black and Ruff create the same formatted output for me. 

```
❯ pipx run ruff format test.py
warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
1 file left unchanged

~/astral/test via 🐍 v3.11.5 
❯ bat test.py
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: test.py
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ def f():
   2   │     return (
   3   │         package_version is not None
   4   │         and package_version.split(".")[:2] == package_info.version.split(".")[:2]
   5   │     )
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

```

---

_Comment by @JonathanPlasse on 2023-09-13 17:16_

```console
❯ cat haha.py
def f():
    return (
        package_version is not None
        and package_version.split(".")[:2] == package_info.version.split(".")[:2]
    )
❯ pipx run ruff format haha.py --isolated
warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
1 file reformatted
❯ cat haha.py
def f():
    return package_version is not None and package_version.split(".")[
        :2
    ] == package_info.version.split(".")[:2]
```

---

_Comment by @MichaReiser on 2023-09-13 17:39_

This is probably related to 

https://github.com/astral-sh/ruff/blob/ebe9c03545a4496811efeee7a530a5632e48e7d2/crates/ruff_python_formatter/src/expression/expr_subscript.rs#L69-L71

using `parenthesized`. It seems that subscript has a lower precedence than binary expressions, meaning it should use `in_parentheses_only_*` as well. 


[Playground](https://play.ruff.rs/7a0ebba3-4261-4ca6-ad4c-e2d9a3088ea9)

---

_Label `waiting-on-author` removed by @MichaReiser on 2023-09-13 17:44_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-13 17:44_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-13 17:44_

---

_Closed by @MichaReiser on 2023-09-14 08:43_

---
