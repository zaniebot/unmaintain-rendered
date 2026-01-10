---
number: 11600
title: N805 - method with no args shows no lint warnings
type: issue
state: closed
author: s-cork
labels:
  - bug
assignees: []
created_at: 2024-05-29T14:40:40Z
updated_at: 2024-05-30T13:18:27Z
url: https://github.com/astral-sh/ruff/issues/11600
synced_at: 2026-01-10T01:22:51Z
---

# N805 - method with no args shows no lint warnings

---

_Issue opened by @s-cork on 2024-05-29 14:40_

```python
class Foo:
    def baz(): # no lint warning
        return 42

    def bar(s): # correctly gives lint warning
        return 42
```

I don't see a lint warning for this method
checked in the ruff playground
I'd expect `N805: First argument of a method should be named "self"`





---

_Label `bug` added by @charliermarsh on 2024-05-30 02:24_

---

_Referenced in [astral-sh/ruff#11605](../../astral-sh/ruff/pulls/11605.md) on 2024-05-30 02:35_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-30 02:35_

---

_Comment by @charliermarsh on 2024-05-30 02:38_

This would differ from `pep8-naming`... though it's almost certainly an error.

---

_Comment by @charliermarsh on 2024-05-30 02:51_

I gave this a try in https://github.com/astral-sh/ruff/pull/11605#issuecomment-2138575064 but decided against changing it after reviewing the ecosystem results. There are a ton of cases of folks omitting arguments from methods (especially in test suites) that seem valid. The rule is really about naming, and if you omitted the argument entirely, that's actually a correctness issue which requires different logic.

---

_Closed by @charliermarsh on 2024-05-30 02:51_

---

_Reopened by @charliermarsh on 2024-05-30 03:12_

---

_Comment by @charliermarsh on 2024-05-30 03:12_

Let me revisit after fixing one bug in that PR.

---

_Comment by @s-cork on 2024-05-30 03:37_

It might also be that you move this to E0211 rather than N805

See this issue where E0211 is marked as implemented (possibly incorrectly): https://github.com/astral-sh/ruff/issues/970
https://britonad.github.io/pylint-errors/plerr/errors/classes/E0211.html




---

_Comment by @charliermarsh on 2024-05-30 13:18_

We may want another rule for it eventually but after reviewing the changes, I think we should leave the pep8-naming rules as they are.

---

_Closed by @charliermarsh on 2024-05-30 13:18_

---

_Referenced in [astral-sh/ruff#970](../../astral-sh/ruff/issues/970.md) on 2024-05-30 13:32_

---
