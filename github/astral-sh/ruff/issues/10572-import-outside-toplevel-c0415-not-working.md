---
number: 10572
title: import-outside-toplevel / C0415 not working
type: issue
state: closed
author: darkanthey
labels:
  - needs-info
assignees: []
created_at: 2024-03-25T16:31:10Z
updated_at: 2024-03-29T19:56:59Z
url: https://github.com/astral-sh/ruff/issues/10572
synced_at: 2026-01-07T13:12:15-06:00
---

# import-outside-toplevel / C0415 not working

---

_Issue opened by @darkanthey on 2024-03-25 16:31_

* The current Ruff version => ruff 0.3.4
* According to ruff implemented [pylint options](https://github.com/astral-sh/ruff/issues/970). 
import-outside-toplevel / C0415 (PLC0415) supported.

So code from the pylint example should get [warning](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/import-outside-toplevel.html): 

![Screenshot 2024-03-25 at 16 27 35](https://github.com/astral-sh/ruff/assets/200977/c2e25124-0f7a-49d3-b57c-11b93f7b6b09)

But 
```
$ ruff check __init__.py
All checks passed!
```


---

_Comment by @charliermarsh on 2024-03-25 16:33_

Are you running with `--preview`? See: https://docs.astral.sh/ruff/rules/import-outside-top-level/

---

_Comment by @darkanthey on 2024-03-26 12:37_

Sorry check the CLI option "--preview" not the return error. 
ruff check __init__.py  --preview

But working with pyproject.toml


---

_Comment by @charliermarsh on 2024-03-26 13:09_

It's working correctly for me!

```
❯ ruff check foo.py --preview --select PLC
foo.py:2:5: PLC0415 `import` should be at the top-level of a file
  |
1 | def print_python_version():
2 |     import sys
  |     ^^^^^^^^^^ PLC0415
  |

Found 1 error.
```

---

_Comment by @charliermarsh on 2024-03-26 13:09_

Perhaps you need to enable the rule?

---

_Label `needs-info` added by @charliermarsh on 2024-03-26 13:09_

---

_Closed by @charliermarsh on 2024-03-29 19:56_

---
