---
number: 100
title: Implement AST-to-source code generation
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-09-04T14:03:01Z
updated_at: 2022-10-04T20:27:58Z
url: https://github.com/astral-sh/ruff/issues/100
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement AST-to-source code generation

---

_Issue opened by @charliermarsh on 2022-09-04 14:03_

Not critical but this could be somewhat useful for autofixing. Similar to [astor](https://github.com/berkerpeksag/astor/blob/a9c5f8d119d617c891b7ba0aa8bc046b8af01e84/astor/code_gen.py). The main issue with AST-to-code generation is that it's impossible to preserve formatting, including comments and docstrings, since those aren't captured in the AST. That's a big limitation, and means that you can really only use this for reconstructing small subtrees.


---

_Label `enhancement` added by @charliermarsh on 2022-09-04 14:03_

---

_Comment by @charliermarsh on 2022-09-04 14:04_

(The solution to the 'formatting and comments' problem is to use a CST like in LibCST or an FST like in [RedBaron](https://redbaron.readthedocs.io/en/latest/index.html).)

---

_Referenced in [astral-sh/ruff#292](../../astral-sh/ruff/pulls/292.md) on 2022-09-30 15:27_

---

_Closed by @charliermarsh on 2022-10-04 20:27_

---
