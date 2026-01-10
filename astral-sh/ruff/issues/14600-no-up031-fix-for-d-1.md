```yaml
number: 14600
title: "no UP031 fix for `\"%d\" % 1`"
type: issue
state: open
author: njzjz
labels:
  - question
  - type-inference
assignees: []
created_at: 2024-11-26T05:11:44Z
updated_at: 2024-12-27T19:33:05Z
url: https://github.com/astral-sh/ruff/issues/14600
synced_at: 2026-01-10T11:09:56Z
```

# no UP031 fix for `"%d" % 1`

---

_Issue opened by @njzjz on 2024-11-26 05:11_

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
Ruff 0.8.0
Minimal example:
```py
"%d" % 1
```
https://play.ruff.rs/2acb42ef-d5d7-4dc0-a649-2b7ff4e174f2

```
ruff check up031.py --select UP031 --fix --unsafe-fixes
up031.py:1:1: UP031 Use format specifiers instead of percent format
  |
1 | "%d" % 1
  | ^^^^ UP031
  |
  = help: Replace with format specifiers

Found 1 error.
```

There is no fix, even no unsafe fix. But there is a fix if `%d` is replaced by `%f`. Isn't the logic similar here?

In a real project, there are hundreds of errors like this.


---

_Comment by @MichaReiser on 2024-11-26 07:08_

> There is no fix, even no unsafe fix. But there is a fix if %d is replaced by %f. Isn't the logic similar here?

The logic is similar but what's different to `%s` is that all parameters need to be wrapped in `int(parameter)` to avoid changing behavior. 

```py
>>> "a %d" % 2.34
'a 2'
>>> f"a {2.34}"
'a 2.34'

>>> "a %d" % "test"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: %d format: a real number is required, not str
>>> f"a {'test'}"
'a test'
```

We would get the same behavior when wrapping the parameter with `int`

```py
>>> f"a {int(2.34)}"
'a 2'
>>> f"a {int('test')}"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: 'test'
```

Unfortunately, Ruff doesn't understand today whether the parameter is an int literal or not, which is why the fix would have to wrap all parameters with `int`, which may or may not be what users want. 





---

_Label `question` added by @MichaReiser on 2024-11-26 07:08_

---

_Label `type-inference` added by @MichaReiser on 2024-11-26 07:08_

---

_Comment by @un-pogaz on 2024-12-19 10:58_

I personaly don't think that add a int convertion is absolutly required here, and implement a simple conversion to `"{}"` or `f"{value}"` will enough. We are under `--unsafe-fixes` flag, so it's to the final user to be sure that no wrong conversion has happen.

I agree that it's a partial solution, and the conversion of "%d" need be keep under the `--unsafe-fixes` flag until a good solution is implemented, BUT it will work perfectly fine for the 90% of user case.

---

_Comment by @Skylion007 on 2024-12-27 19:32_

Wouldn't just `f"a {2.34:d}"` be equivalent?

---
