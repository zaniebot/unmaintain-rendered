```yaml
number: 17480
title: "[`ruff`] add fix safety section (`RUF017`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: document-fix-safety-quadratic-list-summation
created_at: 2025-04-19T15:05:12Z
updated_at: 2025-05-01T06:03:58Z
url: https://github.com/astral-sh/ruff/pull/17480
synced_at: 2026-01-12T15:56:02Z
```

# [`ruff`] add fix safety section (`RUF017`)

---

_@VascoSch92_

The PR add the `fix safety` section for rule `RUF017` (#15584 )


---

_Comment by @github-actions[bot] on 2025-04-19 15:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`ruff`] add fix safety section (`RUFF017`)" to "[`ruff`] add fix safety section (`RUF017`)" by @VascoSch92 on 2025-04-19 16:23_

---

_Label `documentation` added by @AlexWaygood on 2025-04-22 19:19_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-04-22 19:35_

---

_Comment by @dylwil3 on 2025-04-26 11:56_

Thanks for this!

Looking at the implementation for this rule, I notice that it is only emitted when the user explicitly provides the `start` argument for `sum` as an empty list (either `list()` or `[]`). The expression `sum(lists,[])` would raise a type error if the iterable `lists` had any non-list items, and the user has no control over the implementation of `__add__` or `__iadd__` for the primitive `list`.

However, there is a small difference in implementation between `__add__` and `__iadd__` for `list`: the former requires the right summand to be a list, whereas the latter allows for any iterable. 

So, unless I'm misunderstanding, I think the only thing the fix could do that would change behavior is turn code that used to raise an exception into code that no longer raises an exception. Namely:

```python
import functools
import operator

ranges = [range(2), range(3)]

# raises a TypeError
sum(ranges, []) 

# evaluates to [0, 1, 0, 1, 2]
functools.reduce(operator.iadd, ranges, [])
```

So maybe we can reword the fix safety section in light of this?

---

_Comment by @VascoSch92 on 2025-04-26 21:20_

Hey @dylwil3 
thanks for the explanation. It is really subtile the case here, and pretty interesting.

Perhaps something on this direction?

```text
The fix is always marked as unsafe because `sum` uses the `__add__` magic method while
`operator.iadd` uses the `__iadd__` magic method, and these behaves different on lists, i.e., 
the former requires the right summand to be a list, whereas the latter allows for any iterable. 
Therefore, the fix could inadvertently cause code that previously raised an error to silently 
succeed with different results. Moreover, the fix could remove comments from the original code.
```

Another question: what is the politic with deleted comments? Should this be mentioned in the `fix safety` section? I think yes but not sure.

---

_@dylwil3 approved on 2025-04-28 22:04_

Thank you! Your wording was good - I added it in with some minor corrections.

---

_Merged by @dylwil3 on 2025-04-28 22:07_

---

_Closed by @dylwil3 on 2025-04-28 22:07_

---

_Branch deleted on 2025-05-01 06:03_

---
