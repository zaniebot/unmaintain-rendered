```yaml
number: 18806
title: "[`Pyflakes`] `F504` fix changes behavior if args have side effects"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-06-19T23:09:31Z
updated_at: 2025-06-26T08:48:31Z
url: https://github.com/astral-sh/ruff/issues/18806
synced_at: 2026-01-10T11:09:58Z
```

# [`Pyflakes`] `F504` fix changes behavior if args have side effects

---

_Issue opened by @MeGaGiGaGon on 2025-06-19 23:09_

### Summary

The [percent-format-extra-named-arguments (F504)](https://docs.astral.sh/ruff/rules/percent-format-extra-named-arguments/#percent-format-extra-named-arguments-f504) fix can change behavior if the extra args have side effects. It should probably be an always unsafe fix, or only safe if the args being removed are only literals.
[playground](https://play.ruff.rs/e87d8cdc-5f74-4e38-bde5-a8951274d82e)
```
PS ~\Desktop\New_folder\ruff>Get-Content issue.py
```
```py
"Hello, %(name)s" % {"greeting": print(1), "name": "World"}
```
```
PS ~\Desktop\New_folder\ruff>py issue.py
```
```
1
```
```
PS ~\Desktop\New_folder\ruff>uvx ruff check issue.py --isolated --select F
```
```snap
issue.py:1:1: F504 [*] `%`-format string has unused named argument(s): greeting
  |
1 | "Hello, %(name)s" % {"greeting": print(1), "name": "World"}
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ F504
  |
  = help: Remove extra named arguments: greeting

Found 1 error.
[*] 1 fixable with the `--fix` option.
```
```
PS ~\Desktop\New_folder\ruff>uvx ruff check issue.py --isolated --select F --fix
```
```
Found 1 error (1 fixed, 0 remaining).
```
```
PS ~\Desktop\New_folder\ruff>Get-Content issue.py
```
```py
"Hello, %(name)s" % {"name": "World"}
```
```
PS ~\Desktop\New_folder\ruff>py issue.py
```
```
```

This also applies to [string-dot-format-extra-named-arguments (F522)](https://docs.astral.sh/ruff/rules/string-dot-format-extra-named-arguments/#string-dot-format-extra-named-arguments-f522) [playground](https://play.ruff.rs/55f7e08b-c4f7-4cfc-9512-11109d808095)

This also applies to [string-dot-format-extra-positional-arguments (F523)](https://docs.astral.sh/ruff/rules/string-dot-format-extra-positional-arguments/#string-dot-format-extra-positional-arguments-f523) [playground](https://play.ruff.rs/d586f954-6a06-4ebb-b1b5-d8199b523a68)

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Label `bug` added by @MichaReiser on 2025-06-20 06:49_

---

_Label `fixes` added by @MichaReiser on 2025-06-20 06:49_

---

_Comment by @MichaReiser on 2025-06-20 06:50_

I think we can limit it to only be unsafe if the argument is a call expression (or contains one). Yes, there are a few obscure cases where e.g. a `__str__` call (or `__format__`?) has side effects, but we shouldn't optimize for edge cases here.

---

_Closed by @MichaReiser on 2025-06-26 08:48_

---
