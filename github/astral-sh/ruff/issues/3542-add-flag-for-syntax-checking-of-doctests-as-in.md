---
number: 3542
title: Add flag for syntax checking of doctests as in flake8 --doctests
type: issue
state: open
author: marcinbarczynski
labels:
  - core
  - linter
assignees: []
created_at: 2023-03-15T16:38:25Z
updated_at: 2024-03-20T07:44:05Z
url: https://github.com/astral-sh/ruff/issues/3542
synced_at: 2026-01-07T13:12:14-06:00
---

# Add flag for syntax checking of doctests as in flake8 --doctests

---

_Issue opened by @marcinbarczynski on 2023-03-15 16:38_

Currently, there is no way to force ruff to check the syntax of doctests.


---

_Label `core` added by @charliermarsh on 2023-03-15 23:22_

---

_Comment by @charliermarsh on 2023-03-15 23:22_

Agreed.

---

_Comment by @charliermarsh on 2023-03-15 23:23_

Should we be checking these by default?

---

_Comment by @marcinbarczynski on 2023-03-16 06:37_

IMO yes.

---

_Comment by @jamesbraza on 2023-04-25 22:24_

For `flake8 --doctests`: https://flake8.pycqa.org/en/latest/user/options.html#cmdoption-flake8-doctests

I believe `doctests = False` is the default in `flake8`.

+1 to this request, I also think it should be the default for both `flake8` and `ruff`

---

_Referenced in [astral-sh/ruff#4125](../../astral-sh/ruff/issues/4125.md) on 2023-04-27 04:58_

---

_Comment by @tqa236 on 2024-03-19 17:54_

Hello, we are also interested in this feature, especially when the code in docstring can be formatted with `ruff` already. Is there any plan to implement this feature soon?

---

_Referenced in [pandas-dev/pandas#57863](../../pandas-dev/pandas/pulls/57863.md) on 2024-03-19 18:04_

---

_Label `cli` added by @dhruvmanila on 2024-03-20 07:34_

---

_Label `linter` added by @dhruvmanila on 2024-03-20 07:34_

---

_Comment by @dhruvmanila on 2024-03-20 07:44_

> Is there any plan to implement this feature soon?

We're interested in adding support to lint and format embedded Python code in general. It's just we're a small team and currently we have our hands full with other tasks, but we'll notify once we start work on this.

Related #8237

---

_Label `cli` removed by @dhruvmanila on 2024-03-20 07:44_

---

_Referenced in [icesat2py/icepyx#543](../../icesat2py/icepyx/pulls/543.md) on 2024-08-12 21:31_

---
