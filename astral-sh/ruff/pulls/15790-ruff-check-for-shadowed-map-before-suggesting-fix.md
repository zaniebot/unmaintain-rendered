```yaml
number: 15790
title: "[`ruff`] Check for shadowed `map` before suggesting fix (`RUF058`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: brent/shadow-map
created_at: 2025-01-28T17:07:29Z
updated_at: 2025-01-28T19:15:39Z
url: https://github.com/astral-sh/ruff/pull/15790
synced_at: 2026-01-12T15:55:52Z
```

# [`ruff`] Check for shadowed `map` before suggesting fix (`RUF058`)

---

_@ntBre_

## Summary

Fixes #15786 by not suggesting a fix if `map` doesn't have its builtin binding.

## Test Plan

New test taken from the report in #15786.


---

_Label `bug` added by @ntBre on 2025-01-28 17:07_

---

_Label `fixes` added by @ntBre on 2025-01-28 17:07_

---

_Label `preview` added by @ntBre on 2025-01-28 17:07_

---

_Comment by @github-actions[bot] on 2025-01-28 17:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-01-28 17:29_

Nice! While we're here, as an optional extra: this rule is lacking docs on why the fix is sometimes marked as unsafe (the reason looks like it's because it could destroy comments in some situations). We should probably add a `## Fix safety` section to the docs that explains this (see https://github.com/astral-sh/ruff/issues/15584)

---

_Comment by @ntBre on 2025-01-28 17:53_

Good catch! I adapted the fix safety section from RUF055:

```rust
/// ## Fix safety
///
/// This rule's fix is marked as unsafe if the `starmap` or `zip` expressions contain comments that
/// would be deleted by applying the fix. Otherwise, the fix can be applied safely.
```

What about a `## Fix availability` section? I only found one example of that in RUF052 with a quick grep. Maybe it's not usually in a separate section.

---

_Comment by @AlexWaygood on 2025-01-28 17:55_

> What about a `## Fix availability` section? I only found one example of that in RUF052 with a quick grep. Maybe it's not usually in a separate section.

It's true that we don't document it as often currently, but I think it's nice to document that too when we can, for sure!

---

_Comment by @ntBre on 2025-01-28 18:03_

Sounds good! I added both new sections. I put safety first (pun somewhat intended), then availability, but happy to reverse it too.

---

_@AlexWaygood approved on 2025-01-28 18:09_

---

_Merged by @ntBre on 2025-01-28 19:15_

---

_Closed by @ntBre on 2025-01-28 19:15_

---

_Branch deleted on 2025-01-28 19:15_

---
