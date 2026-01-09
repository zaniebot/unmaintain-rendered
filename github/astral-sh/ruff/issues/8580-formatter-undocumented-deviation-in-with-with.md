---
number: 8580
title: "Formatter undocumented deviation in `with` with multiple context managers"
type: issue
state: closed
author: DeD1rk
labels:
  - documentation
  - formatter
assignees: []
created_at: 2023-11-09T11:39:31Z
updated_at: 2023-11-10T04:32:32Z
url: https://github.com/astral-sh/ruff/issues/8580
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter undocumented deviation in `with` with multiple context managers

---

_Issue opened by @DeD1rk on 2023-11-09 11:39_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi, I noticed a small difference between Black and ruff's formatting, that is not listed in https://docs.astral.sh/ruff/formatter/#intentional-deviations. I've added a sample below.

```python
# Input:

with some_context_manager(
    "aaaaaaaaaaaaaaaaaaaaaaaa", another_long_function("test1")
), some_context_manager(
    "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa", another_long_function("test1")
), some_context_manager(
    "aaaaaaaaaaaaaaaaaaaaaaa", another_long_function("test1")
):
    print("hi")


# Black:

with some_context_manager(
    "aaaaaaaaaaaaaaaaaaaaaaaa", another_long_function("test1")
), some_context_manager(
    "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa", another_long_function("test1")
), some_context_manager(
    "aaaaaaaaaaaaaaaaaaaaaaa", another_long_function("test1")
):
    print("hi")


# Ruff:

with some_context_manager(
    "aaaaaaaaaaaaaaaaaaaaaaaa", another_long_function("test1")
), some_context_manager(
    "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa", another_long_function("test1")
), some_context_manager("aaaaaaaaaaaaaaaaaaaaaaa", another_long_function("test1")):
    print("hi")

```

This is on ruff v0.1.4 and black 23.11.0, both with line-length 100.


---

_Label `documentation` added by @charliermarsh on 2023-11-09 19:09_

---

_Label `formatter` added by @charliermarsh on 2023-11-09 19:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-09 19:09_

---

_Comment by @charliermarsh on 2023-11-09 19:09_

👍 I believe this is intended but not documented. I will add it to the list.

---

_Referenced in [astral-sh/ruff#8597](../../astral-sh/ruff/pulls/8597.md) on 2023-11-10 04:24_

---

_Closed by @charliermarsh on 2023-11-10 04:32_

---
