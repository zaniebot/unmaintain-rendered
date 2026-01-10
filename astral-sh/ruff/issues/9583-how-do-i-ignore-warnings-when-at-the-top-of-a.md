---
number: 9583
title: How do I ignore warnings when at the top of a module file?
type: issue
state: closed
author: red8888
labels:
  - question
assignees: []
created_at: 2024-01-19T18:58:57Z
updated_at: 2024-06-01T23:38:08Z
url: https://github.com/astral-sh/ruff/issues/9583
synced_at: 2026-01-10T01:22:49Z
---

# How do I ignore warnings when at the top of a module file?

---

_Issue opened by @red8888 on 2024-01-19 18:58_

I asked the same question in the [flake8-docstrings](https://github.com/PyCQA/flake8-docstrings/issues/72) repo, but I have since switched to ruff so curious if this is possible.

I use sphinx and at the top of my py files I have this:

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

"""
my-module
=========
Some notes about my module
"""
```

These warning are incorrect in the this context:

1 blank line required between summary line and descriptionFlake8(D205)
First line should end with a periodFlake8(D400)

I can't put a blank line above ======= because that breaks the headings and I also don't want to put a period after my heading. How can I ignore in just these cases?

EDIT
====
So looking here: https://stackoverflow.com/questions/7877522/how-do-i-disable-missing-docstring-warnings-at-a-file-level-in-pylint

It seems with pylint I could have disabled C0114, how do I do that with the rust project.toml?

---

_Label `question` added by @charliermarsh on 2024-01-20 00:58_

---

_Comment by @Siecje on 2024-02-23 20:59_

You can ignore rules using 

 https://github.com/Siecje/htmd/blob/b8702ebabaa33cf91200488a89ce32b75a2ea449/pyproject.toml#L48-L50

or ignore rules for a specific file using.
https://github.com/Siecje/htmd/blob/b8702ebabaa33cf91200488a89ce32b75a2ea449/pyproject.toml#L72-L74



---

_Comment by @charliermarsh on 2024-06-01 23:38_

Yeah I'd suggest taking a look at https://docs.astral.sh/ruff/configuration/. You can either use per-file ignores to ignore in certain files, or disable the rules entirely.

---

_Closed by @charliermarsh on 2024-06-01 23:38_

---
