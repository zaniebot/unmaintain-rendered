---
number: 3045
title: Rule to enforce import module only
type: issue
state: open
author: justinchuby
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-02-20T00:18:56Z
updated_at: 2025-10-30T16:51:38Z
url: https://github.com/astral-sh/ruff/issues/3045
synced_at: 2026-01-07T13:12:14-06:00
---

# Rule to enforce import module only

---

_Issue opened by @justinchuby on 2023-02-20 00:18_

The google style guide recommends importing modules only. https://google.github.io/styleguide/pyguide.html#22-imports. It would be great if there’s a rule to enforce that. 

---

_Label `rule` added by @charliermarsh on 2023-02-20 00:19_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:31_

---

_Referenced in [astral-sh/ruff#5841](../../astral-sh/ruff/issues/5841.md) on 2023-07-17 21:04_

---

_Comment by @Avasam on 2024-08-11 20:28_

If I understand correctly, you'd want https://docs.astral.sh/ruff/rules/banned-import-from/ but for all imports ? (which could be done if globbing is supported, see https://github.com/astral-sh/ruff/issues/2660 )

---

_Comment by @justinchuby on 2024-08-12 15:51_

Not exactly. From import should still be allowed if the imported name is a module. 

E.g.

from torch import nn

should be allowed because nn is a module. 

Whereas

from torch.nn import Module 

should be disallowed 

---

_Comment by @qthequartermasterman on 2025-03-24 17:20_

https://github.com/Enforcer/pylint_google_style_guide_imports_enforcing exists as a reference implementation. It is a pylint plugin.

---

_Referenced in [astral-sh/ruff#21118](../../astral-sh/ruff/pulls/21118.md) on 2025-10-29 15:08_

---

_Comment by @revmischa on 2025-10-29 16:38_

I opened a PR to enforce the google style guide rules #21118 

---

_Comment by @qthequartermasterman on 2025-10-30 16:51_

It's tragic the PR was closed. The [pylint plugin](https://github.com/Enforcer/pylint_google_style_guide_imports_enforcing) enforcing this feature is the only things keeping us on pylint still.

---
