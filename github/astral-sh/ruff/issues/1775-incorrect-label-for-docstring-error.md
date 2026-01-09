---
number: 1775
title: Incorrect label for docstring error
type: issue
state: closed
author: colin99d
labels:
  - bug
  - docstring
assignees: []
created_at: 2023-01-11T02:06:54Z
updated_at: 2023-01-12T01:01:55Z
url: https://github.com/astral-sh/ruff/issues/1775
synced_at: 2026-01-07T13:12:14-06:00
---

# Incorrect label for docstring error

---

_Issue opened by @colin99d on 2023-01-11 02:06_


<img width="766" alt="Screenshot 2023-01-10 at 9 04 07 PM" src="https://user-images.githubusercontent.com/72827203/211700931-26bf31e0-6885-490d-9330-9a892bf0926d.png">

I believe the wording on this warning is incorrect. It is fixed by switching to triple quotes. However, it says you need double quotes.


---

_Comment by @charliermarsh on 2023-01-11 02:10_

I believe it's referring to single quotes (`'`) vs. double quotes (`"`). But, it seems wrong because your docstring has double quotes? What's up with that?

---

_Comment by @colin99d on 2023-01-11 02:11_

I thought it also might be referring to that as well. I can try to have a fix in for that tonight!

---

_Comment by @charliermarsh on 2023-01-11 02:18_

Thanks!

---

_Comment by @charliermarsh on 2023-01-11 02:19_

It looks like that code isn't robust to single character docstrings (like `"` instead of `"""`).

---

_Label `bug` added by @charliermarsh on 2023-01-11 02:19_

---

_Label `docstring` added by @charliermarsh on 2023-01-11 02:19_

---

_Comment by @colin99d on 2023-01-11 02:21_

Thats what it looks like to me. I am trying to look at flake8-quotes documentation to see if:
1. `"` is valid and should be allowed (no warning thrown)
2. `"""` is required, so we need to change the message


I guess its all up to taste in the end, how do you want me to do it? [For reference](https://pypi.org/project/flake8-quotes/)

---

_Comment by @charliermarsh on 2023-01-11 02:24_

`"` should be valid. It shouldn't matter if you use three quote characters or one quote character, for the sake of _that_ rule.

---

_Comment by @charliermarsh on 2023-01-11 02:25_

Might help to look at `pydocstyle/rules.rs#triple_quotes`, and `TRIPLE_QUOTE_PREFIXES`. We should check the prefix rather than doing a `.contains` call.

---

_Referenced in [astral-sh/ruff#1777](../../astral-sh/ruff/pulls/1777.md) on 2023-01-11 03:08_

---

_Closed by @charliermarsh on 2023-01-12 01:01_

---
