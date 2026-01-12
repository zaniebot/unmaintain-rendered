```yaml
number: 9597
title: "[`ruff`] Implement `mutable-fromkeys-value` (`RUF024`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: mutable-fromkeys-value
created_at: 2024-01-21T15:10:58Z
updated_at: 2024-01-22T09:35:59Z
url: https://github.com/astral-sh/ruff/pull/9597
synced_at: 2026-01-12T15:55:29Z
```

# [`ruff`] Implement `mutable-fromkeys-value` (`RUF024`)

---

_@tjkuson_

## Summary

Implement rule `mutable-fromkeys-value` (`RUF023`).

Autofixes

```python
dict.fromkeys(foo, [])
```

to

```python
{key: [] for key in foo}
```

The fix is marked as unsafe as it changes runtime behaviour. It also uses `key` as the comprehension variable, which may not always be desired.

Closes #4613.

## Test Plan

`cargo test`


---

_Marked ready for review by @tjkuson on 2024-01-21 15:20_

---

_Comment by @github-actions[bot] on 2024-01-21 15:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-01-21 15:47_

Could you maybe use RUF024 for this? I'm using RUF023 for #9564 :)

---

_Comment by @tjkuson on 2024-01-21 15:52_

Oops, I will change it when I am back at my desk

---

_Comment by @Skylion007 on 2024-01-21 16:37_

Can you also open an issue on this for flake8-bugbear? I'm sure they'd love to include this rule and it would make more sense with a reserved B error code. Other ruff rule suggestions have been added there and we usually migrate the rule alias.

---

_Comment by @Skylion007 on 2024-01-21 16:43_

Ah nvm, there is an issue that has been open for a while: https://github.com/PyCQA/flake8-bugbear/issues/387

---

_Renamed from "Implement rule `mutable-fromkeys-value` (`RUF023`)" to "Implement rule `mutable-fromkeys-value` (`RUF024`)" by @AlexWaygood on 2024-01-21 16:49_

---

_Comment by @tjkuson on 2024-01-21 17:20_

Thanks for renaming the PR @AlexWaygood, didn't realise I had missed that

---

_Review requested from @charliermarsh by @charliermarsh on 2024-01-21 20:34_

---

_@charliermarsh approved on 2024-01-22 00:10_

Excellent work, thank you.

---

_Label `rule` added by @charliermarsh on 2024-01-22 00:10_

---

_Label `preview` added by @charliermarsh on 2024-01-22 00:10_

---

_Renamed from "Implement rule `mutable-fromkeys-value` (`RUF024`)" to "[`ruff`] Implement `mutable-fromkeys-value` (`RUF024`)" by @charliermarsh on 2024-01-22 00:10_

---

_Merged by @charliermarsh on 2024-01-22 00:22_

---

_Closed by @charliermarsh on 2024-01-22 00:22_

---

_Branch deleted on 2024-01-22 09:35_

---
