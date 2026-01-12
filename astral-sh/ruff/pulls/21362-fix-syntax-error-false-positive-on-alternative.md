```yaml
number: 21362
title: "Fix syntax error false positive on alternative `match` patterns"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/alt-patterns-2
created_at: 2025-11-10T13:28:04Z
updated_at: 2025-11-10T15:51:53Z
url: https://github.com/astral-sh/ruff/pull/21362
synced_at: 2026-01-12T15:57:21Z
```

# Fix syntax error false positive on alternative `match` patterns

---

_@ntBre_

Summary
--

Fixes #21360 by using the union of names instead of overwriting them, as Micha suggested originally on #21104.

This avoids overwriting the `n` name in the `Subscript` by the empty set of names visited in the nested OR pattern before visiting the other arm of the outer OR pattern.

Test Plan
--

A new inline test case taken from the issue

---

_Label `bug` added by @ntBre on 2025-11-10 13:28_

---

_Comment by @astral-sh-bot[bot] on 2025-11-10 13:39_


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

_Marked ready for review by @ntBre on 2025-11-10 13:39_

---

_Review requested from @MichaReiser by @ntBre on 2025-11-10 13:39_

---

_Review requested from @dhruvmanila by @ntBre on 2025-11-10 13:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1880 on 2025-11-10 13:46_

Can you say a bit more about the fix. My understanding (and why I recommended it) was that using union or intersection would only matter for the error recovery case. But it seems that using union over intersection is important for valid cases too or are we only fixing a symptom here?

---

_@MichaReiser reviewed on 2025-11-10 13:46_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1880 on 2025-11-10 14:05_

I think the union is correct because there can be existing patterns in `self.names` before visiting nested alternative patterns. We need to compare all of the names we visit in that arm to all of the names we visit in the next arm, not just the set of names visited in the last, innermost alternative pattern.

That's what's happening in this case because we first visit the outer class pattern, which captures `n`, then we were overwriting that `n` with the empty set when visiting the `Constant | Slice` part of the pattern. Instead we need the union of `n` and the empty set to compare to the `Attribute(n)` branch.

```py
match 42:
    case ast.Subscript(n, ast.Constant() | ast.Slice()) 
       | ast.Attribute(n): ...
```

---

_@ntBre reviewed on 2025-11-10 14:05_

---

_@MichaReiser approved on 2025-11-10 15:21_

---

_Merged by @ntBre on 2025-11-10 15:51_

---

_Closed by @ntBre on 2025-11-10 15:51_

---

_Branch deleted on 2025-11-10 15:51_

---
