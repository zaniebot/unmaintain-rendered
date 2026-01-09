---
number: 8604
title: "[ruff-format] flake8-class-newline"
type: issue
state: closed
author: oysstu
labels:
  - formatter
  - style
assignees: []
created_at: 2023-11-10T15:28:49Z
updated_at: 2023-11-10T16:35:32Z
url: https://github.com/astral-sh/ruff/issues/8604
synced_at: 2026-01-07T13:12:15-06:00
---

# [ruff-format] flake8-class-newline

---

_Issue opened by @oysstu on 2023-11-10 15:28_

Working on a codebase where [flake8-class-newline](https://pypi.org/project/flake8-class-newline/) is enforced. To quote that module:

> [PEP8](https://www.python.org/dev/peps/pep-0008/#blank-lines) says we should surround every class method with a single blank line. However flake8 is ambiguous about the first method having a blank line above it.

The ruff formatter currently removes leading spaces after a class definition if there is no docstring. Honestly, I've seen more Python classes without a leading space before the first method than ones with a leading space, so I don't think adding a space should necessary be the default, but it would be nice to have an option for having it (or ignoring it if it exists).

Class without newline (current ruff formatting)
```python
class AClassWithoutANewLine(object):
    def a_method(self):
        return 'a_value'
```

Class with newline (flake8-class-newline expected formatting)
```python
class AClassWithoutANewLine(object):

    def a_method(self):
        return 'a_value'
```

When a docstring is present, ruff currently adds a space between the docstring and first method, which passes the flake8-class-newline linter.

---

_Comment by @zanieb on 2023-11-10 16:21_

Thanks for the report; this is a duplicate of https://github.com/astral-sh/ruff/issues/8566 and I think the reasoning there stands. Happy to hear additional thoughts though.

---

_Closed by @zanieb on 2023-11-10 16:21_

---

_Comment by @oysstu on 2023-11-10 16:34_

Aha, didn't find that issue when searching. Fair enough, I wish the PEP8 would be more explicit regarding this

---

_Label `formatter` added by @zanieb on 2023-11-10 16:35_

---

_Label `style` added by @zanieb on 2023-11-10 16:35_

---
