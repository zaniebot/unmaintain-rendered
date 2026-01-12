```yaml
number: 11371
title: "[Fix error] PIE804 auto fix does not handle := "
type: issue
state: closed
author: njharman
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-05-11T17:49:37Z
updated_at: 2024-05-11T22:04:55Z
url: https://github.com/astral-sh/ruff/issues/11371
synced_at: 2026-01-12T15:54:51Z
```

# [Fix error] PIE804 auto fix does not handle := 

---

_@njharman_

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
I believe the issue is ruff 0.4.4 PIE804 auto fix does not handle := with same name as dict key

```
        func(
              **{
                  "live": False,
                  "url_foo": (url_foo := "http://custom.com"),
                  "url_bar": (url_bar := "http://bespoke.com"),
              },
          )

```



---

_Comment by @zanieb on 2024-05-11 18:21_

What happens? No fix is offered? The fix is incorrect?

---

_Comment by @trag1c on 2024-05-11 18:23_

Seems like Ruff fixes this as
```py
func(
      live=False,
      url_foo=url_foo := "http://custom.com",
      url_bar=url_bar := "http://bespoke.com",
)
```
which is a syntax error

---

_Label `bug` added by @zanieb on 2024-05-11 18:24_

---

_Label `fixes` added by @zanieb on 2024-05-11 18:24_

---

_Comment by @njharman on 2024-05-11 18:31_

> What happens? No fix is offered? The fix is incorrect?

Ruff fixed, found syntax error then reverted fix (without showing it (or maybe I'm blind?)) and asked me to kindly report it.

---

_Comment by @charliermarsh on 2024-05-11 21:28_

I think they just need to be parenthesized.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-11 21:28_

---

_Closed by @charliermarsh on 2024-05-11 22:04_

---
