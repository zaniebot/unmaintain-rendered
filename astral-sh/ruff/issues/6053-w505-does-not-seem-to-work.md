```yaml
number: 6053
title: W505 does not seem to work
type: issue
state: closed
author: scop
labels:
  - documentation
assignees: []
created_at: 2023-07-25T04:41:12Z
updated_at: 2023-07-25T15:53:10Z
url: https://github.com/astral-sh/ruff/issues/6053
synced_at: 2026-01-10T11:09:48Z
```

# W505 does not seem to work

---

_Issue opened by @scop on 2023-07-25 04:41_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

W505 does not seem to trigger for the [bad example in its docs](https://beta.ruff.rs/docs/rules/doc-line-too-long/#example):

```shellsession
ruff 0.0.280
$ cat t.py 
def function(x):
    """Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis auctor purus ut ex fermentum, at maximus est hendrerit."""
$ ruff t.py 
t.py:2:89: E501 Line too long (127 > 88 characters)
Found 1 error.
$ ruff --select W505 --ignore E501 t.py 
$ 
```

Actually, I seem unable to trigger it in the first place:

```shellsession
$ cat t.py 
def function(x):
    # Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis auctor purus ut ex fermentum, at maximus est hendrerit.
    pass
$ ruff t.py 
t.py:2:89: E501 Line too long (123 > 88 characters)
Found 1 error.
$ ruff --select W505 --ignore E501 t.py 
$ 
```

No `pyproject.toml` involved.

---

_Comment by @scop on 2023-07-25 04:45_

> No pyproject.toml involved.

Ah, this is the issue. Configuring `pycodestyle.max-doc-length` makes it appear. Maybe this should be noted in the docs, will have a look.

---

_Comment by @scop on 2023-07-25 04:51_

Note included in #6052

---

_Label `documentation` added by @charliermarsh on 2023-07-25 15:26_

---

_Closed by @charliermarsh on 2023-07-25 15:53_

---
