```yaml
number: 18666
title: unnecessary_dict_kwargs doc - a note on type checking benefits
type: pull_request
state: merged
author: Andrej730
labels:
  - documentation
assignees: []
merged: true
base: main
head: unnecessary_dict_kwargs_doc
created_at: 2025-06-13T19:16:29Z
updated_at: 2025-06-20T06:46:11Z
url: https://github.com/astral-sh/ruff/pull/18666
synced_at: 2026-01-12T15:56:23Z
```

# unnecessary_dict_kwargs doc - a note on type checking benefits

---

_@Andrej730_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Recently stumbled upon type checkers typically not being able to verify kwargs when they're unpacked from just constructed dictionary:

Snippet:
```python
def foo(bar: int):
    return bar + 1


print(foo(**{"bar": 2}))
# No typing errors.
print(foo(**{"bar":2, "baz":22}))
```

[pyright-play](https://pyright-play.net/?strict=true&code=CYUwZgBGD20BQCMCGAnAXBAlgOwC4Eo0BYAKAnIhRFwFcVsJkUIBqCARlK5IAcUdccGPABUIgN4AiJpIwAmAL758pPgKGw4YqTLRyANBGlIAXrLmLlQA)
[mypy-play](https://mypy-play.net/?mypy=latest&python=3.12&gist=2f8eb19089aa0bb1f7d65f69844cf4da)


And I've found that PIE804 can be helpful to highlight those cases.
Perhaps a note in PIE804 documentation can help advertise this rule as the one also enchancing type safety.



---

_Label `documentation` added by @ntBre on 2025-06-13 22:32_

---

_Review requested from @ntBre by @ntBre on 2025-06-13 22:34_

---

_Comment by @github-actions[bot] on 2025-06-13 22:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Closed by @MichaReiser on 2025-06-19 11:04_

---

_Reopened by @MichaReiser on 2025-06-19 11:04_

---

_@MichaReiser approved on 2025-06-20 06:27_

---

_Comment by @MichaReiser on 2025-06-20 06:27_

Thank you

---

_Merged by @MichaReiser on 2025-06-20 06:27_

---

_Closed by @MichaReiser on 2025-06-20 06:27_

---

_Comment by @Andrej730 on 2025-06-20 06:46_

🎉🎉
Technically my first few lines of code in .rs I've ever written 😅

---

_Branch deleted on 2025-06-20 06:46_

---
