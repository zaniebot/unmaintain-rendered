```yaml
number: 10390
title: "Add fix to remove unused standard library imports from `__init__.py` files"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - fixes
assignees: []
created_at: 2024-03-13T17:55:30Z
updated_at: 2024-05-02T23:10:33Z
url: https://github.com/astral-sh/ruff/issues/10390
synced_at: 2026-01-10T11:09:52Z
```

# Add fix to remove unused standard library imports from `__init__.py` files

---

_Issue opened by @zanieb on 2024-03-13 17:55_

See https://github.com/astral-sh/ruff/issues/10365#issuecomment-1995132836

We currently do not offer any fixes for unused imports in `__init__.py` files.

This issue is to fix standard library imports by removing them from the file. We should probably label this fix as unsafe.
            

---

_Label `fixes` added by @zanieb on 2024-03-13 17:55_

---

_Label `good first issue` added by @zanieb on 2024-03-13 17:57_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-03-13 18:11_

---

_Assigned to @plredmond by @plredmond on 2024-04-22 15:29_

---

_Unassigned @AlexWaygood by @plredmond on 2024-04-22 15:29_

---

_Comment by @plredmond on 2024-04-24 21:44_

For the purposes of this check, I'm going to consider `from __future__ import ...` style imports to be "standard library".

---

_Comment by @charliermarsh on 2024-04-25 00:00_

I think `from __future__` imports can just be ignored entirely, since they should never be removed as "unused".

---

_Comment by @plredmond on 2024-04-25 00:17_

Considered together with #10391, does this summary of behaviors seem right?

<details><summary>Old table</summary>

```
for some unused import... 

future                            → no diagnostic or fix
stdlib ∧  in_init                 → unsafe, remove
stdlib ∧ ¬in_init                 → safe,   remove
1stpty ∧  in_init                 → safe,   move to __all__ or convert to explicit-import
1stpty ∧ ¬in_init                 → safe,   remove
3rdpty ∧  in_init ∧  ignore_init  → no diagnostic or fix
3rdpty ∧  in_init ∧ ¬ignore_init  → unsafe, remove
3rdpty ∧ ¬in_init                 → safe,   remove

```

</details>

---

_Comment by @charliermarsh on 2024-04-25 00:26_

My impression was that for `3rdpty ∧  in_init`, we never offered a fix.

---

_Comment by @zanieb on 2024-04-25 00:48_

That summary looks reasonable to me, with the addition that we should probably deprecate and remove the `ignore_init` option and remove third-party imports by default?

---

_Comment by @charliermarsh on 2024-04-25 00:50_

Yeah I revise my comment -- what Zanie says sounds right.

---

_Comment by @plredmond on 2024-04-25 16:21_

Ok, I've collapsed the table above. Here's an edited version without the `ignore_init` bit. I'll implement this, and try to make it flexible enough that I can PR #10390 separately from #10391 and so that we can more easily make changes to it in the future if desired.

```
for some unused import... 

future             → no diagnostic or fix
stdlib ∧  in_init  → unsafe, remove
stdlib ∧ ¬in_init  → safe,   remove
1stpty ∧  in_init  → safe,   move to __all__ or convert to explicit-import
1stpty ∧ ¬in_init  → safe,   remove
3rdpty ∧  in_init  → unsafe, remove
3rdpty ∧ ¬in_init  → safe,   remove
```


---

_Closed by @plredmond on 2024-05-02 23:10_

---
