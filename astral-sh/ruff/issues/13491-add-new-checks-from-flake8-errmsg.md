---
number: 13491
title: Add new checks from flake8-errmsg
type: issue
state: open
author: wwuck
labels:
  - rule
  - needs-design
assignees: []
created_at: 2024-09-24T05:10:03Z
updated_at: 2024-09-25T07:04:12Z
url: https://github.com/astral-sh/ruff/issues/13491
synced_at: 2026-01-10T01:22:53Z
---

# Add new checks from flake8-errmsg

---

_Issue opened by @wwuck on 2024-09-24 05:10_

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
I would like to request adding two new checks from flake8-errmsg that have been added since the original checks were added to ruff.

- [ ] EM104: Check for missing parentheses for built-in exceptions.
- [ ] EM105: Check for missing message for built-in exceptions.

https://github.com/henryiii/flake8-errmsg/pull/23

Example `test.py`:
```python
def foo():
    raise Exception


def bar():
    raise Exception()
```
```
test.py:2:5: EM104 Built-in Exceptions must not be thrown without being called
test.py:6:5: EM105 Built-in Exceptions must have a useful message
```

---

_Comment by @opk12 on 2024-09-24 06:31_

Related: [RSE102](https://docs.astral.sh/ruff/rules/unnecessary-paren-on-raise-exception/) `Unnecessary parentheses on raised exception [unnecessary-paren-on-raise-exception]`

---

_Comment by @MichaReiser on 2024-09-24 10:14_

> Related: [RSE102](https://docs.astral.sh/ruff/rules/unnecessary-paren-on-raise-exception/) `Unnecessary parentheses on raised exception [unnecessary-paren-on-raise-exception]`

This is a great note. We would have to merge `RSE102` and `EM104` to avoid introducing conflicting rules, but that's difficult because `RSE102` applies to all exceptions where `EM104` applies only to built-in exceptions, unless we extend its scope. This needs some design and a decision. 



---

_Label `rule` added by @MichaReiser on 2024-09-24 10:14_

---

_Label `needs-design` added by @MichaReiser on 2024-09-24 10:14_

---

_Comment by @opk12 on 2024-09-25 06:54_

Newbie Q: is EM104 redundant with EM105 though?

---

_Comment by @opk12 on 2024-09-25 07:00_

Also, what about checking for any stdlib exception, even if not a builtin in [this](https://docs.python.org/3/library/exceptions.html) sense? The original [feature request](https://github.com/henryiii/flake8-errmsg/issues/13) argument (that is, ease of debugging and log file informativeness) applies if an exception has an unspecific name, or can be raised in very many places. I don't know if stdlib non-builtin exceptions do exist after [PEP 3151](https://peps.python.org/pep-3151/), though.

---

_Comment by @MichaReiser on 2024-09-25 07:03_

> Newbie Q: is EM104 redundant with EM105 though?

I consider EM104 a less opinionated version of EM105. Having the rules separate allows some users to only opt in to the stylistic EM104 without requiring an exception message. But yes, the rule is redundant if both EM104 and EM105 are enabled. This is a great point.

---
