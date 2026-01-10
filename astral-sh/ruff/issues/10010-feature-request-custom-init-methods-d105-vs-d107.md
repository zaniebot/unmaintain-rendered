---
number: 10010
title: "Feature Request: Custom Init Methods (D105 vs D107)"
type: issue
state: open
author: Spectre5
labels:
  - docstring
assignees: []
created_at: 2024-02-16T16:36:20Z
updated_at: 2024-02-21T20:19:09Z
url: https://github.com/astral-sh/ruff/issues/10010
synced_at: 2026-01-10T01:22:49Z
---

# Feature Request: Custom Init Methods (D105 vs D107)

---

_Issue opened by @Spectre5 on 2024-02-16 16:36_

D105 checks for documented magic methods, except that `__init__` is special cased into D107 since many people put that documentation instead into the class docstring.  I'd like ability to add addition magic methods to be considered an init method.  In particular, the dataclass `__post_init__` method as well as the attrs `__attrs_pre_init__`, `__attrs_post_init__`, and `__attrs_init__`.

Or perhaps it is more appropriate to add an option of methods to be ignored for docstrings, similar to how that [already exists](https://docs.astral.sh/ruff/settings/#lint_pydocstyle_ignore-decorators) for decorators.  Then I could add those various init functions to that instead to be ignored by D105.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-20 05:53_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-02-20 05:53_

---

_Label `docstring` added by @charliermarsh on 2024-02-20 05:53_

---

_Comment by @charliermarsh on 2024-02-20 05:54_

If possible, i'd prefer not to make this an option and instead figure out if this is a common pattern and, if so, just change the rule to reflect it, similar to `__init__`. Do you know if this pattern of (not) documenting `__post_init__` is common?

---

_Comment by @Spectre5 on 2024-02-21 20:19_

To be honest, I'm not sure if `__post_init__` is typically documented or not.  I personally see no reason to document it as anything unique in it should be documented in the class docstring, in my opinion.  This is why I kind of shifted my thought in the second paragraph that perhaps it makes more sense to add a whitelist of function names to be ignored by D105, which would also allow a use to add the `attrs` methods or any other library or common functions they don't care to document...

---
