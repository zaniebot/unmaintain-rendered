---
number: 11797
title: " F841 is triggered by unused variables in tuple unpacking from with statements (#890 regression)"
type: issue
state: closed
author: andykais
labels: []
assignees: []
created_at: 2024-06-07T15:30:59Z
updated_at: 2024-11-10T03:14:13Z
url: https://github.com/astral-sh/ruff/issues/11797
synced_at: 2026-01-07T13:12:15-06:00
---

#  F841 is triggered by unused variables in tuple unpacking from with statements (#890 regression)

---

_Issue opened by @andykais on 2024-06-07 15:30_

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
An older issue (https://github.com/astral-sh/ruff/issues/890#) called out this behavior as fixed, but it appears to be reintroduced in `ruff 0.4.7`. The following code snippet gives an error on `F841`:

```py
def paginate():
  pass

def foo():
  total, results = paginate()
  return results
```


`ruff check ruff_test.py`
```
ruff_test.py:5:3: F841 Local variable `total` is assigned to but never used
  |
4 | def foo():
5 |   total, results = paginate()
  |   ^^^^^ F841
6 |   return results
  |
  = help: Remove assignment to unused variable `total`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

The error goes away by prefixing `total` with `_`. Is this now the required recommendation for unused tuple unpacking args? I noticed that for loops do not exhibit this error. E.g.
```py
  for i in range(100):
    print('foo')
```
this does not report an error

---

_Comment by @dylwil3 on 2024-11-10 03:14_

With apologies for the delay, let me try to give an update on the current state of things!

> The following code snippet gives an error on F841

At the moment (Ruff version at the time of this writing is 0.7.3), [`F841`](https://docs.astral.sh/ruff/rules/unused-variable/) is only triggered for unpacked assignments when [`preview`](https://docs.astral.sh/ruff/preview/) mode is enabled. [Playground link](https://play.ruff.rs/1c343a63-cc5d-44c7-81f6-36ef449fbdf9) - you can change the preview mode setting by clicking the settings icon on the left hand side.

> The error goes away by prefixing total with _. Is this now the required recommendation for unused tuple unpacking args?

That is indeed the default recommendation, and there is sometimes an available (unsafe) fix for this, which will prefix the unused variable with an underscore. You can customize the regex used to determine what is a valid name for a "dummy" variable [in the linter settings](https://docs.astral.sh/ruff/settings/#lint_dummy-variable-rgx).

> I noticed that for loops do not exhibit this error

Loop control variables are a treated differently than variables that you are assigning a value to, so there is a separate lint for this - it's [`B007`](https://docs.astral.sh/ruff/rules/unused-loop-control-variable/).

Hope this helps, however belated!


---

_Closed by @dylwil3 on 2024-11-10 03:14_

---
