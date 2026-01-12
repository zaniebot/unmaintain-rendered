```yaml
number: 6437
title: Formatter fails to parenthesize walrus expressions in function return types
type: issue
state: closed
author: charliermarsh
labels:
  - formatter
assignees: []
created_at: 2023-08-09T04:31:37Z
updated_at: 2023-08-16T07:11:20Z
url: https://github.com/astral-sh/ruff/issues/6437
synced_at: 2026-01-12T15:54:46Z
```

# Formatter fails to parenthesize walrus expressions in function return types

---

_@charliermarsh_

I saw this example in Black:

```python
def this_is_so_dumb() -> (please := no): ...

def this_is_so_dumb(x) -> (please := no): ...
```

Right now, the formatter strips parentheses on the right-hand side, seemingly because `optional_parentheses` doesn't take _required_ parentheses into account, unlike `maybe_parenthesize_expression`.


---

_Label `formatter` added by @charliermarsh on 2023-08-09 04:31_

---

_Comment by @charliermarsh on 2023-08-09 04:33_

@konstin - Do you know if it's possible to remove `optional_parentheses`, and replace its usages with `maybe_parenthesize_expression`? My experience from https://github.com/astral-sh/ruff/pull/6436 suggests that it might be possible, but might in turn require a new `Parenthesize` variant to support the "always parenthesize if the group breaks" behavior seen in that PR for the no-function-arguments case.

---

_Comment by @charliermarsh on 2023-08-09 04:38_

@konstin - (Feel free to ignore, actually, I need to explore this more myself before I ask for your time.)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-09 04:47_

---

_Closed by @charliermarsh on 2023-08-11 18:28_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:11_

---
