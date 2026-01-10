```yaml
number: 17496
title: "[`flynt`] add fix safety section (`FLY002`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: document-fix-safety-static_join_to_fstring
created_at: 2025-04-20T12:51:12Z
updated_at: 2025-04-26T16:04:04Z
url: https://github.com/astral-sh/ruff/pull/17496
synced_at: 2026-01-10T19:33:02Z
```

# [`flynt`] add fix safety section (`FLY002`)

---

_Pull request opened by @VascoSch92 on 2025-04-20 12:51_

The PR add the fix safety section for rule `FLY002` (#15584 )

The motivation for the content of the fix safety section is given by the following example

```python
foo = 1
bar = [2, 3]

try:
    result_join = " ".join((foo, bar))
    print(f"Join result: {result_join}")
except TypeError as e:
    print(f"Join error: {e}")
```

which print `Join error: sequence item 0: expected str instance, int found`

But after the fix is applied, we have

```python
foo = 1
bar = [2, 3]

try:
    result_join = f"{foo} {bar}"
    print(f"Join result: {result_join}")
except TypeError as e:
    print(f"Join error: {e}")
```

which print `Join result: 1 [2, 3]`

---

_Comment by @github-actions[bot] on 2025-04-20 12:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @AlexWaygood on 2025-04-22 19:18_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-04-22 19:34_

---

_@dylwil3 approved on 2025-04-26 15:57_

Thank you!

---

_Merged by @dylwil3 on 2025-04-26 16:00_

---

_Closed by @dylwil3 on 2025-04-26 16:00_

---
