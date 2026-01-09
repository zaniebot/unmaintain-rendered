---
number: 252
title: E501 reported in spite of noqa for string literal containing spaces
type: issue
state: closed
author: cjfuller
labels:
  - bug
assignees: []
created_at: 2022-09-21T22:29:04Z
updated_at: 2023-02-03T18:28:29Z
url: https://github.com/astral-sh/ruff/issues/252
synced_at: 2026-01-07T13:12:14-06:00
---

# E501 reported in spite of noqa for string literal containing spaces

---

_Issue opened by @cjfuller on 2022-09-21 22:29_

The following code incorrectly reports an E501:

test.py
```python
"""
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa aaaaaaaaaaaaaaaaa
"""  # noqa: E501

```

```
$ ruff test.py
Found 1 error(s).
test.py:2:89: E501 Line too long (89 > 88 characters)
```

However without the space (and an extra "a" added to keep the length the same), this error is correctly ignored (no errors are reported:

```python
"""
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
"""  # noqa: E501

```

In both cases, flake8 does not report a line length error.

(ruff 0.0.44, python 3.8.14, ubuntu 22.04)

---

_Comment by @charliermarsh on 2022-09-21 23:38_

(I think the issue here is that Flake8 respects the ` # noqa: E501` on the following line, while ruff only respects noqa directives on the same line.)

---

_Comment by @cjfuller on 2022-09-21 23:53_

Sorry, why does the second example not error then? I initially thought that might be the case, but I was able to add an arbitrary number of lines to this string, and as long as none of them contained spaces, ruff was fine.

---

_Comment by @charliermarsh on 2022-09-21 23:54_

Flake8 has the same behavior (unless I'm mistaken), which is: if you have a long line consisting of a single token, don't raise a line length error. This is helpful for things like URLs.

---

_Label `bug` added by @charliermarsh on 2022-09-21 23:57_

---

_Comment by @charliermarsh on 2022-09-22 12:31_

I'll try to fix this today, I think it's important.

---

_Added to milestone `Release 0.1.0` by @charliermarsh on 2022-09-22 12:31_

---

_Referenced in [astral-sh/ruff#257](../../astral-sh/ruff/pulls/257.md) on 2022-09-22 16:15_

---

_Closed by @charliermarsh on 2022-09-22 16:56_

---
