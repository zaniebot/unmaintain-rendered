```yaml
number: 18086
title: "[`flake8-simplify`] add fix safety section (`SIM103`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-safety-section-needless-bool
created_at: 2025-05-14T06:42:22Z
updated_at: 2025-05-14T18:24:15Z
url: https://github.com/astral-sh/ruff/pull/18086
synced_at: 2026-01-10T18:51:01Z
```

# [`flake8-simplify`] add fix safety section (`SIM103`)

---

_Pull request opened by @VascoSch92 on 2025-05-14 06:42_

The PR add the `fix safety` section for rule `SIM103` (#15584 )

### Unsafe Fix Example

```python
class Foo:
    def __eq__(self, other):
        return 1
    
def foo():
    if Foo() == 1:
        return True
    return False

def foo_fix():
    return Foo() == 1
    
print(foo()) # True
print(foo_fix()) # 1
```

### Note

I updated the code snippet example, because I thought it was cool to have a correct example, i.e., that I can paste inside the playground and it works :-) 

---

_Comment by @github-actions[bot] on 2025-05-14 06:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-05-14 15:45_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs`:52 on 2025-05-14 15:57_

I don't think the part about comments is true. The code checks for comments in the fix range and avoids a fix in that case (although that happens way earlier in the code than the fix generation, so it's not that clear).

I wanted to say that the boolean condition part wasn't true either because we do try to add `bool` calls in cases where it doesn't look like a boolean, but your clever example does change the behavior. Do you think it's worth mentioning `__eq__` or the other comparison methods that could cause problems? Maybe a second sentence like:

> While the fix will try to wrap non-boolean values in a call to `bool`, custom implementations of comparison functions like `__eq__` can avoid the `bool` call and still lead to altered behavior.

---

_@ntBre reviewed on 2025-05-14 16:12_

Thanks! Great catch again on the `__eq__` example too. Just one comment about comments and an idea for incorporating the findings from your example.

---

_@VascoSch92 reviewed on 2025-05-14 17:16_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs`:52 on 2025-05-14 17:16_

> I don't think the part about comments is true. The code checks for comments in the fix range and avoids a fix in that case (although that happens way earlier in the code than the fix generation, so it's not that clear).

You are right. I delete this part.

> I wanted to say that the boolean condition part wasn't true either because we do try to add bool calls in cases where it doesn't look like a boolean, but your clever example does change the behavior. Do you think it's worth mentioning __eq__ or the other comparison methods that could cause problems? Maybe a second sentence like:

Fine for me. I update the fix safety section.



---

_Review requested from @ntBre by @VascoSch92 on 2025-05-14 17:16_

---

_@ntBre approved on 2025-05-14 18:23_

Great, thank you!

---

_Merged by @ntBre on 2025-05-14 18:24_

---

_Closed by @ntBre on 2025-05-14 18:24_

---
