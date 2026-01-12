```yaml
number: 17440
title: "[`pyflakes`] Add fix safety section to docs (`F601`, `F602`)"
type: pull_request
state: merged
author: Kalmaegi
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc_fix_safety_for_repeated_keys
created_at: 2025-04-17T02:30:35Z
updated_at: 2025-04-18T18:31:56Z
url: https://github.com/astral-sh/ruff/pull/17440
synced_at: 2026-01-12T15:56:02Z
```

# [`pyflakes`] Add fix safety section to docs (`F601`, `F602`)

---

_@Kalmaegi_

## Summary

add fix safety section to repeated_keys_docs, for #15584 



---

_Assigned to @ntBre by @ntBre on 2025-04-18 17:55_

---

_Label `documentation` added by @ntBre on 2025-04-18 17:55_

---

_@ntBre approved on 2025-04-18 18:08_

Thanks! I also tested this one, and it definitely removes comments.

It could also technically be unsafe if the values have side effects:

```py
foo = {
    2: print(2),
    2: print(2),
}
```

The rule only offers a fix if the values are equal, but it doesn't consider potential side effects.

I think mentioning the comments is sufficient, though. This example is pretty contrived, and the comment case seems most common.

---

_Comment by @github-actions[bot] on 2025-04-18 18:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-04-18 18:24_

On second thought, I went ahead and added the mention of side effects, just in case. I'll merge once CI finishes!

---

_Renamed from "add fix safety section to docs" to "[`pyflakes`] Add fix safety section to docs (`F601`, `F602`)" by @ntBre on 2025-04-18 18:25_

---

_Merged by @ntBre on 2025-04-18 18:27_

---

_Closed by @ntBre on 2025-04-18 18:27_

---
