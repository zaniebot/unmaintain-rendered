```yaml
number: 17490
title: "[`pyupgrade`] Add fix safety section to docs (`UP029`)"
type: pull_request
state: merged
author: Kalmaegi
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc_fix_safety_for_unnecessary_builtin_import
created_at: 2025-04-20T08:00:55Z
updated_at: 2025-08-29T14:02:59Z
url: https://github.com/astral-sh/ruff/pull/17490
synced_at: 2026-01-10T17:46:21Z
```

# [`pyupgrade`] Add fix safety section to docs (`UP029`)

---

_Pull request opened by @Kalmaegi on 2025-04-20 08:00_

## Summary

add `fix safety` section to `UP029: unnecessary_builtin_import.rs`, for  #15584 

I conducted some tests and found that the only side effect should be the removal of inline comments along with the code. However, during the deletion process, comments placed above the code are preserved. I'm not sure if this behavior needs to be explicitly described.


---

_Comment by @github-actions[bot] on 2025-04-20 08:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @VascoSch92 on 2025-04-20 12:01_

I have two example one should look into :-)

The first one is the case where we import a builtin with an alias, e.g., `from builtins import str as builtin_str`, is the rule removing this import or not?

For the second, let's suppose that we do something like:
```python
# Original code
def str(x):
    return f"Custom: {x}"

# somewhere in the code
from builtins import str
result = str(1)  # Uses Python's builtin str
```
then the rules will correct that as

```python
def str(x):
    return f"Custom: {x}"

result = str(1)
```

which is a change of behaviour.

---

_Comment by @Kalmaegi on 2025-04-20 13:41_

> I have two example one should look into :-)
> 
> The first one is the case where we import a builtin with an alias, e.g., `from builtins import str as builtin_str`, is the rule removing this import or not?
> 
> For the second, let's suppose that we do something like:
> 
> ```python
> # Original code
> def str(x):
>     return f"Custom: {x}"
> 
> # somewhere in the code
> from builtins import str
> result = str(1)  # Uses Python's builtin str
> ```
> 
> then the rules will correct that as
> 
> ```python
> def str(x):
>     return f"Custom: {x}"
> 
> result = str(1)
> ```
> 
> which is a change of behaviour.

The first point should be skipped, but the second one does affect the original behavior. Thank you very much!

---

_Renamed from "[pyupgrade] Add fix safety section to docs (UP029)" to "[`pyupgrade`] Add fix safety section to docs (`UP029`)" by @Kalmaegi on 2025-04-20 14:58_

---

_Label `documentation` added by @dhruvmanila on 2025-04-21 15:10_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-04-22 19:34_

---

_@dylwil3 approved on 2025-08-17 23:33_

Thanks @Kalmaegi  and @VascoSch92 !

---

_Merged by @dylwil3 on 2025-08-29 13:55_

---

_Closed by @dylwil3 on 2025-08-29 13:55_

---
