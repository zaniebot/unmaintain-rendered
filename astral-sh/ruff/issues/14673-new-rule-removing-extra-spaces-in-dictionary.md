```yaml
number: 14673
title: new rule - Removing extra spaces in dictionary
type: issue
state: closed
author: spaceby
labels:
  - rule
assignees: []
created_at: 2024-11-29T08:52:08Z
updated_at: 2024-11-29T21:40:41Z
url: https://github.com/astral-sh/ruff/issues/14673
synced_at: 2026-01-12T15:54:54Z
```

# new rule - Removing extra spaces in dictionary

---

_@spaceby_

Ruff does not remove extra spaces in such dictionaries.
```python
d = {'a':               'b'}
```
In other cases, spaces are removed.
Please add a rule for this case too.

---

_Comment by @MichaReiser on 2024-11-29 09:05_

Have you tried `ruff format`? It should remove the extra spacing. Or aren't you using it because it is too opinionated?



---

_Comment by @spaceby on 2024-11-29 12:11_

I don't use it.

---

_Label `rule` added by @MichaReiser on 2024-11-29 21:34_

---

_Comment by @MichaReiser on 2024-11-29 21:40_

I'll merge this into https://github.com/astral-sh/ruff/issues/2402 because this is covered by pycodestyle E241

---

_Closed by @MichaReiser on 2024-11-29 21:40_

---
