---
number: 950
title: Ignore E501 for long URLs in comments or docstrings
type: issue
state: closed
author: brandon-leapyear
labels:
  - question
assignees: []
created_at: 2022-11-28T23:45:39Z
updated_at: 2024-02-26T06:07:07Z
url: https://github.com/astral-sh/ruff/issues/950
synced_at: 2026-01-07T13:12:14-06:00
---

# Ignore E501 for long URLs in comments or docstrings

---

_Issue opened by @brandon-leapyear on 2022-11-28 23:45_

https://github.com/PyCQA/pycodestyle/blob/1063db8747e7d4e213160458aa3792e5ec05bc10/pycodestyle.py#L278-L285

---

_Comment by @charliermarsh on 2022-11-29 00:57_

Thanks! Do you have a minimal code snippet that reproduces the issue for you? I thought we _did_ support this so want to ensure I have the right example to work from.

---

_Label `question` added by @charliermarsh on 2022-11-29 00:57_

---

_Comment by @brandon-leapyear on 2022-11-29 01:00_

:sparkles: _**This is an old work account. Please reference @brandonchinn178 for all future communication**_ :sparkles:
<!-- updated by mention_personal_account_in_comments.py -->

---

This fails with `ruff` but passes with `flake8`:
```py
# https://google.com/aowriejfaeoifjawoeihfjawoiuehfjoaiwejfoaiwejfoawiejfoiawejfoaajwfoiaewjfoiawejfoiwajfweaoi
```

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-29 01:01_

---

_Comment by @charliermarsh on 2022-11-29 01:08_

I think this was actually a regression, fixing.

---

_Referenced in [astral-sh/ruff#952](../../astral-sh/ruff/pulls/952.md) on 2022-11-29 01:10_

---

_Closed by @charliermarsh on 2022-11-29 01:10_

---

_Comment by @brandon-leapyear on 2022-11-29 01:11_

:sparkles: _**This is an old work account. Please reference @brandonchinn178 for all future communication**_ :sparkles:
<!-- updated by mention_personal_account_in_comments.py -->

---

So fast! Thanks!

---

_Comment by @charliermarsh on 2022-11-29 01:11_

This is going out in [0.0.144](https://github.com/charliermarsh/ruff/releases/tag/v0.0.144).

---

_Comment by @charliermarsh on 2022-11-29 01:11_

No prob! Appreciate the issues.

---
