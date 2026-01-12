```yaml
number: 13275
title: "[`ruff`] Handle unary operators in `decimal-from-float-literal (RUF032)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
assignees: []
merged: true
base: main
head: decimal
created_at: 2024-09-07T05:18:45Z
updated_at: 2024-09-13T14:07:20Z
url: https://github.com/astral-sh/ruff/pull/13275
synced_at: 2026-01-12T15:55:43Z
```

# [`ruff`] Handle unary operators in `decimal-from-float-literal (RUF032)`

---

_@dylwil3_

This PR ensures that the suggested "stringified" float passed to `decimal.Decimal` does not contain more than one unary operator, and that the rule ignores floats with the invalid unary operators `~` and `not`. For example, 

```python
Decimal(- + - + - +4.0)
```
has a suggested fix of `Decimal("-4.0")`.

Closes #13258


---

_Renamed from "[ruff] Handle unary operators in `decimal-from-float-literal (RUF032)`" to "[`ruff`] Handle unary operators in `decimal-from-float-literal (RUF032)`" by @dylwil3 on 2024-09-07 05:19_

---

_Comment by @github-actions[bot] on 2024-09-07 05:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-09-07 13:23_

Thanks! I pushed a commit to reduce the amount of cloning of AST nodes, which can be quite expensive. I also made a few other small simplifications -- let me know if you have any questions about the changes I pushed!

---

_Merged by @AlexWaygood on 2024-09-07 13:25_

---

_Closed by @AlexWaygood on 2024-09-07 13:25_

---

_Comment by @dylwil3 on 2024-09-07 13:27_

I was wondering how to do something like that, so that's super helpful to see - thanks for making the code much nicer!

---

_Branch deleted on 2024-09-07 13:28_

---

_Label `bug` added by @dhruvmanila on 2024-09-13 14:07_

---
