```yaml
number: 18818
title: "[`Pylint`] [preview] `PLE4703` false negative on any reassignment"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
  - rule
assignees: []
created_at: 2025-06-20T07:22:51Z
updated_at: 2025-06-24T06:24:10Z
url: https://github.com/astral-sh/ruff/issues/18818
synced_at: 2026-01-12T15:54:56Z
```

# [`Pylint`] [preview] `PLE4703` false negative on any reassignment

---

_@MeGaGiGaGon_

### Summary

If there is any reassignment of the target variable, even when both are as a set, [modified-iterating-set (PLE4703)](https://docs.astral.sh/ruff/rules/modified-iterating-set/#modified-iterating-set-ple4703) will have a false negative. [playground](https://play.ruff.rs/9d10264a-9541-4f70-bc20-3483c22ce9b4)
```py
works = {1, 2, 3}
for num in works:  # PLE4703
    works.add(num + 5)

nums = {1, 2, 3}
nums = {1, 2, 3}
for num in nums:   # No error
    nums.add(num + 5)
```

### Version

playground

---

_Label `rule` added by @ntBre on 2025-06-20 12:29_

---

_Label `bug` added by @ntBre on 2025-06-20 12:30_

---

_Comment by @MichaReiser on 2025-06-24 06:24_

Same as for https://github.com/astral-sh/ruff/issues/18855

Simply using `resolve_name` over `only_binding` trades false negatives with false positives. 

I think the right approach here is to modify the implementation to resolve all visible bindings and verify that all of them are dictionaries. I can't say how hard that is because I'm not very familiar with our semantic model. 

An alternative is to change `only_binding` to return `Some` even if there are multiple bindings but if only one binding is visible unconditionally (not dependent on control flow paths)

---
