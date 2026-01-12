```yaml
number: 12391
title: D301 autofix doubles escaped backslashes
type: issue
state: open
author: kbattocchi
labels:
  - docstring
  - fixes
assignees: []
created_at: 2024-07-18T19:33:44Z
updated_at: 2024-07-18T22:45:45Z
url: https://github.com/astral-sh/ruff/issues/12391
synced_at: 2026-01-12T15:54:51Z
```

# D301 autofix doubles escaped backslashes

---

_@kbattocchi_

[D301](https://docs.astral.sh/ruff/rules/escape-sequence-in-docstring/) states that docstrings with literal backslashes should use raw (`r"""`) strings.  So something like

    def f():
        """This docstring has a backslash \\ in it"""
        pass

should (perhaps, see below for more thoughts) instead be written as

    def f():
        r"""This docstring has a backslash \ in it"""
        pass

However, the autofix for this issue just adds the `r` modifier to the existing string, so the fix results in

    def f():
        r"""This docstring has a backslash \\ in it"""
        pass

which now has two actual backslashes.  For some escape sequences this is probably the right thing to do (e.g. `C:\venv` in a docstring probably intends a backslash before `v` rather than a vertical tab before `e`) and so ignoring the escape sequence and just treating the backslash as if it were literal during the fix makes sense.  However, I'd argue that for escaped backslashes it's less likely that the user really wanted them doubled up.

Personally, I think that it would make sense to make an exception to D301 for escaped backslashes anyway (like there is for line continuations and escaped unicode characters).  Otherwise, if you need both unicode characters and backslashes in the same string, then you need to include the unicode character inline in a raw string rather than use the unicode escape because the backslash isn't allowed to be escaped.  But if the rule is going to apply, having the fix deduplicate the backslashes in this case would be nice.

---

_Label `docstring` added by @charliermarsh on 2024-07-18 22:45_

---

_Label `fixes` added by @charliermarsh on 2024-07-18 22:45_

---

_Comment by @charliermarsh on 2024-07-18 22:45_

That seems reasonable, though not certain what the "general rule" would be.

---
