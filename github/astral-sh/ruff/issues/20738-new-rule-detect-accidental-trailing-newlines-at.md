---
number: 20738
title: new rule - detect accidental trailing newlines at the start and end of docstrings
type: issue
state: open
author: DetachHead
labels:
  - rule
  - docstring
  - needs-decision
assignees: []
created_at: 2025-10-07T06:47:50Z
updated_at: 2025-10-07T19:08:46Z
url: https://github.com/astral-sh/ruff/issues/20738
synced_at: 2026-01-07T13:12:16-06:00
---

# new rule - detect accidental trailing newlines at the start and end of docstrings

---

_Issue opened by @DetachHead on 2025-10-07 06:47_

### Summary

```py
def foo():
    """

    foo bar baz

    bar

    """
```

in this example, there is an unnecessary additional newline at the start and end of the docstring. it would be nice if ruff had a rule to detect this.

note that this is partially already covered by [`multi-line-summary-first-line` (`D212`)](https://docs.astral.sh/ruff/rules/multi-line-summary-first-line/), but it only detects the newline at the start of the docstring but not the end. i also don't like to have that rule enabled because i don't like how it formats multiline docstrings (why is there a newline at the end of the docstring but not at the start?):

```py
def foo():
    """foo bar baz

    bar
    """
```

---

_Label `rule` added by @ntBre on 2025-10-07 17:17_

---

_Label `needs-decision` added by @ntBre on 2025-10-07 17:17_

---

_Label `docstring` added by @MichaReiser on 2025-10-07 19:07_

---

_Comment by @MichaReiser on 2025-10-07 19:08_

Related to https://github.com/astral-sh/ruff/issues/19564

---
