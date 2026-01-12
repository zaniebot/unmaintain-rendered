```yaml
number: 17499
title: "[`refurb`] Add fix safety section (`FURB105`)"
type: pull_request
state: merged
author: Kalmaegi
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc_fix_safety_for_print_empty_string
created_at: 2025-04-20T15:41:55Z
updated_at: 2025-08-29T13:41:06Z
url: https://github.com/astral-sh/ruff/pull/17499
synced_at: 2026-01-12T15:56:02Z
```

# [`refurb`] Add fix safety section (`FURB105`)

---

_@Kalmaegi_

## Summary

This PR add the `fix safety` section for rule `FURB105` in `print_empty_string.rs` for #15584

Before:
```
def get_sep():
    print("side effect")
    return ""
    
print("", sep=get_sep())
```

After:
```
def get_sep():
    print("side effect")
    return ""
    
print()
```

---

_Comment by @github-actions[bot] on 2025-04-20 15:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @ntBre by @ntBre on 2025-04-21 13:58_

---

_Label `documentation` added by @ntBre on 2025-04-21 13:58_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/print_empty_string.rs`:36 on 2025-04-21 14:09_

How does this sound? I think the rule only looks for `""` (empty string) positional arguments, so the unsafety is only applied to the `sep` keyword argument, as far as I can tell. This one is also only sometimes unsafe, so I tried to reflect that in the beginning of the first sentence.

```suggestion
/// This fix is marked as unsafe if it removes an unused `sep` keyword argument
/// that may have side effects. Removing such arguments may change the program's
/// behavior by skipping the execution of those side effects.
```

---

_@ntBre approved on 2025-04-21 14:10_

Thanks! The example in the PR summary was very helpful! I have one suggestion for tweaking the wording a bit.

---

_Comment by @Kalmaegi on 2025-04-21 14:57_

> Thanks! The example in the PR summary was very helpful! I have one suggestion for tweaking the wording a bit.

Thank you so much for your help! I'm still in the process of learning English, and my English isn't very good. You've helped me a lot.

---

_Merged by @ntBre on 2025-08-29 13:41_

---

_Closed by @ntBre on 2025-08-29 13:41_

---
