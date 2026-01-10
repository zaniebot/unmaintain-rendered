```yaml
number: 17652
title: "[`flake8-bugbear `] Add fix safety section (`B006`) "
type: pull_request
state: merged
author: Kalmaegi
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc_fix_safety_for_mutable_argument_default
created_at: 2025-04-27T09:14:25Z
updated_at: 2025-05-28T20:27:14Z
url: https://github.com/astral-sh/ruff/pull/17652
synced_at: 2026-01-10T18:45:04Z
```

# [`flake8-bugbear `] Add fix safety section (`B006`) 

---

_Pull request opened by @Kalmaegi on 2025-04-27 09:14_

## Summary

This PR add the `fix safety` section for rule `B006` in `mutable_argument_default.rs` for #15584 

When applying this rule for fixes, certain changes may alter the original logical behavior. For example:

before:
```python
def cache(x, storage=[]):
    storage.append(x)
    return storage

print(cache(1))  # [1]
print(cache(2))  # [1, 2]
```

after:
```python
def cache(x, storage=[]):
    storage.append(x)
    return storage

print(cache(1))  # [1]
print(cache(2))  # [2]
```



---

_Comment by @github-actions[bot] on 2025-04-27 09:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @MichaReiser on 2025-04-27 09:52_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:73 on 2025-04-30 20:43_

This is pretty vague. I think something like this would be more helpful:

```rust
/// ## Fix safety
///
/// This fix is marked as unsafe because it replaces the mutable default with `None`
/// and initializes it in the function body, which may not be what the user intended,
/// as described above.
```

Where "described above" refers to the `## Known problems` section. I think it could be reasonable to move that into the `## Fix safety` section too, as  an alternative.

---

_@ntBre requested changes on 2025-04-30 20:44_

---

_@MichaReiser reviewed on 2025-05-26 09:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:73 on 2025-05-26 09:37_

Should we just go with this version and merge the PR?

---

_@Kalmaegi reviewed on 2025-05-27 10:21_

---

_Review comment by @Kalmaegi on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:73 on 2025-05-27 10:21_

Sorry im late, I haven’t used my computer lately, but I’ve implemented the latest revisions as suggested. Really appreciate your assistance!

---

_@ntBre approved on 2025-05-28 20:27_

Thank you!

---

_Merged by @ntBre on 2025-05-28 20:27_

---

_Closed by @ntBre on 2025-05-28 20:27_

---
