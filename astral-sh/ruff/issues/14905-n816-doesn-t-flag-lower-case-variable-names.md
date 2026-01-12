```yaml
number: 14905
title: "N816 doesn't flag lower-case variable names"
type: issue
state: closed
author: isaacag
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-12-11T08:19:54Z
updated_at: 2024-12-23T08:22:00Z
url: https://github.com/astral-sh/ruff/issues/14905
synced_at: 2026-01-12T15:54:54Z
```

# N816 doesn't flag lower-case variable names

---

_@isaacag_

Context managers are not taken into consideration for variable naming conventions, despite PEP8 not making exceptions for them. The only way to trigger variable rules is to wrap them in other structures like a `def`.

```python
def example_function():
    Variable = 10 # N806 triggered
    with open('file.txt') as file:
        Variable = file.read() # N806 triggered
```


```python
with open('file.txt') as file:
    Variable = file.read() # No rule triggered
```

---

_Comment by @MichaReiser on 2024-12-11 08:23_

The current behavior makes sense to me, considering that the rule is about variable names in functions and doesn't enforce lower case variable names in general. 

> Checks for the use of non-lowercase variable names in functions.

And `Variable` in the second example is a variable in the module scope. 



---

_Label `question` added by @MichaReiser on 2024-12-11 08:23_

---

_Comment by @isaacag on 2024-12-11 08:29_

If possible I think it would be nice to have an additional rule covering those cases, even if is another scope.

---

_Comment by @MichaReiser on 2024-12-11 08:59_

> If possible I think it would be nice to have an additional rule covering those cases, even if is another scope.

There's https://docs.astral.sh/ruff/rules/mixed-case-variable-in-global-scope/ but it doesn't forbid using variable names that start with an uppercase character. I'm not sure why is but the rule matches the behavior of the upstream rule https://github.com/PyCQA/pep8-naming/pull/69



---

_Comment by @isaacag on 2024-12-11 09:46_

That is odd, docs leave the impression that any non snake case variables will trigger the rule.

---

_Renamed from "PEP8 inconsistent variable rules for context managers" to "N816 doesn't flag lower-case variable names" by @MichaReiser on 2024-12-16 08:01_

---

_Label `question` removed by @MichaReiser on 2024-12-16 08:01_

---

_Label `rule` added by @MichaReiser on 2024-12-16 08:01_

---

_Label `needs-decision` added by @MichaReiser on 2024-12-16 08:01_

---

_Comment by @MichaReiser on 2024-12-23 08:21_

I merge this with https://github.com/astral-sh/ruff/issues/15069 that lists a few reasons

---

_Closed by @MichaReiser on 2024-12-23 08:21_

---
