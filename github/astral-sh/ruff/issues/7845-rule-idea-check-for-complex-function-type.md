---
number: 7845
title: "Rule idea: check for complex function type annotations"
type: issue
state: closed
author: tjkuson
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-10-07T09:10:16Z
updated_at: 2024-04-10T10:09:33Z
url: https://github.com/astral-sh/ruff/issues/7845
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule idea: check for complex function type annotations

---

_Issue opened by @tjkuson on 2023-10-07 09:10_

It would be nice to have a lint rule that checks for convoluted type annotations, similar to rules that check for complicated functions.

The recommendation would be to move the type information to a type alias or to create a new data type (such as a `NamedTuple`) to refactor type information.

I don't have a specific idea about how the rule should measure type complexity (it could involve measuring nesting, unions, or just the number of parameters); I am more interested in checking if there would be an appetite for any such rule in Ruff at the moment!

---

_Renamed from "Rule idea: check for convoluted function type annotations" to "Rule idea: check for complex function type annotations" by @tjkuson on 2023-10-07 09:11_

---

_Label `rule` added by @charliermarsh on 2023-10-08 14:41_

---

_Label `needs-decision` added by @charliermarsh on 2023-10-08 14:41_

---

_Comment by @strmwalker on 2023-11-01 17:31_

There is a rule in wemake-python-styleguide https://wemake-python-styleguide.readthedocs.io/en/latest/pages/usage/violations/complexity.html#wemake_python_styleguide.violations.complexity.TooComplexAnnotationViolation 
You can take a look at #3845

---

_Comment by @AlexWaygood on 2024-04-10 10:09_

Thanks for the feature request! I'm going to close this in favour of #2323, as it seems like _basically_ the same idea to me (happy to reopen if I'm mistaken!)

---

_Closed by @AlexWaygood on 2024-04-10 10:09_

---
