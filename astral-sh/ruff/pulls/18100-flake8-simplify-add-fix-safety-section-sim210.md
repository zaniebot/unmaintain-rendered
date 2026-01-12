```yaml
number: 18100
title: "[`flake8-simplify`] add fix safety section (`SIM210`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-safety-section-ast-ifexp
created_at: 2025-05-14T17:39:46Z
updated_at: 2025-05-15T20:26:11Z
url: https://github.com/astral-sh/ruff/pull/18100
synced_at: 2026-01-12T15:56:12Z
```

# [`flake8-simplify`] add fix safety section (`SIM210`)

---

_@VascoSch92_

The PR add the `fix safety` section for rule `SIM210` (#15584 )

It is a little cheating, as the Fix safety section is copy/pasted by #18086 as the problem is the same.

### Unsafe Fix Example

```python
class Foo():
    def __eq__(self, other):
        return 0

def foo():
    return True if Foo() == 0 else False

def foo_fix():
    return Foo() == 0

print(foo()) # False
print(foo_fix()) # 0
```


---

_Comment by @github-actions[bot] on 2025-05-14 17:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-05-14 20:28_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_ifexp.rs`:36 on 2025-05-14 20:59_

This one actually does drop comments 😆 : https://play.ruff.rs/a415e7a8-c951-4ed8-af98-a0ebcbc41a40

but only in the case of multi-line if-expressions, which were easiest for me to cause with implicitly-concatenated strings, so it might be pretty rare.

---

_@ntBre reviewed on 2025-05-14 21:00_

Thanks! I think this time we do need a note about comments.

---

_@VascoSch92 reviewed on 2025-05-14 21:31_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_ifexp.rs`:36 on 2025-05-14 21:31_

I will update ;)

I admit that I was lazy and i didn't check

---

_Review requested from @ntBre by @VascoSch92 on 2025-05-15 06:15_

---

_Comment by @VascoSch92 on 2025-05-15 06:15_

> Thanks! I think this time we do need a note about comments.

Added ;-)

---

_@ntBre approved on 2025-05-15 20:25_

Thank you!

---

_Merged by @ntBre on 2025-05-15 20:26_

---

_Closed by @ntBre on 2025-05-15 20:26_

---
