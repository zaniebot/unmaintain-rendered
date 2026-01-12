```yaml
number: 8775
title: "Ruff ecosystem: pass no-preview cli arg by default"
type: pull_request
state: merged
author: T-256
labels:
  - ci
assignees: []
merged: true
base: main
head: patch-4
created_at: 2023-11-20T05:48:16Z
updated_at: 2023-11-20T18:21:52Z
url: https://github.com/astral-sh/ruff/pull/8775
synced_at: 2026-01-10T23:40:55Z
```

# Ruff ecosystem: pass no-preview cli arg by default

---

_Pull request opened by @T-256 on 2023-11-20 05:48_

Addresses https://github.com/astral-sh/ruff/pull/8489#issuecomment-1793513411
That issues still exist on formatter, but since `black` doesn't support `no-preview` cli arg, I didn't include it in this PR.


---

_Comment by @zanieb on 2023-11-20 15:18_

Thanks! You can add this to the formatter too. We don't use comparisons with Black in CI anyway.

---

_Label `ci` added by @zanieb on 2023-11-20 15:18_

---

_@zanieb reviewed on 2023-11-20 16:04_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/projects.py`:49 on 2023-11-20 16:04_

Please revert, this change is not correct.

```python
def foo(**kwargs):
    print(kwargs)


a = {"x": 1}
b = {"x": 2}

foo(**{**a, **b})
foo(**a, **b)
```
```
❯ python example.py
{'x': 2}
Traceback (most recent call last):
  File "/Users/mz/eng/src/astral-sh/ruff/example.py", line 9, in <module>
    foo(**a, **b)
TypeError: __main__.foo() got multiple values for keyword argument 'x'
```

---

_@zanieb reviewed on 2023-11-20 16:15_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/projects.py`:49 on 2023-11-20 16:15_

I guess Python 3.9+ can use the `|` operator 🤯 e.g. `foo(**(a | b))` — that'd be fun someday.

---

_@T-256 reviewed on 2023-11-20 16:17_

---

_Review comment by @T-256 on `python/ruff-ecosystem/ruff_ecosystem/projects.py`:49 on 2023-11-20 16:17_

Oh correct, actually I was working on that rule before :)
This line became visually problem to my eyes. Can we use cleaner approach? For example:
```py
kwargs.update(asdict(self))
return type(self)(**kwargs)
```
it's safe to mutate the `kwargs` because it won't access out of scope never again.

---

_@zanieb reviewed on 2023-11-20 16:26_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/projects.py`:49 on 2023-11-20 16:26_

It'd need to be reversed because we want the kwargs to override asdict.

In general though, we shouldn't make unrelated changes like this in the same pull request as a fix.

---

_Renamed from "lint ecosystem: respect no-preview" to "Ruff ecosystem: pass no-preview cli arg by default" by @T-256 on 2023-11-20 16:46_

---

_@zanieb approved on 2023-11-20 18:00_

Thanks!

---

_Comment by @github-actions[bot] on 2023-11-20 18:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @zanieb on 2023-11-20 18:21_

---

_Closed by @zanieb on 2023-11-20 18:21_

---
