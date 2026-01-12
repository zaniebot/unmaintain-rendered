```yaml
number: 13910
title: "Fix `normalize` arguments when `fstring_formatting` is disabled"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: micha/fix-normalize-call-arguments
created_at: 2024-10-24T13:02:47Z
updated_at: 2024-10-24T13:13:37Z
url: https://github.com/astral-sh/ruff/pull/13910
synced_at: 2026-01-12T15:55:46Z
```

# Fix `normalize` arguments when `fstring_formatting` is disabled

---

_@MichaReiser_

## Summary

This fixes the invocation of `normalize_string` in the flat implicit concatenation formatting to respect whether
f-string formatting is enabled or not.

## Test Plan

I disabled f-string formatting by changing `is_f_string_formatting_enabled` to return `false`. 
All tests were passing except formatting `f"{'Hy "User"'}" f'{"Hy 'User'"}'`. It gets formatted as

```
f"{'Hy 'User''}{'Hy 'User''}"
```

which is not valid syntax.

I looked into fixing it but it's kind of hard because it requires detecting that this is not a valid pre 312 f-string and we should avoid flipping the quotes
for the second expression.... 


I suggest that we look into fixing this if we decide to only ship implicit concatenated string formatting but not f-string formatting (our current plan is to ship both).
There's a test that fails if we decide to only promote one style.



---

_Label `internal` added by @MichaReiser on 2024-10-24 13:02_

---

_Label `formatter` added by @MichaReiser on 2024-10-24 13:02_

---

_Merged by @MichaReiser on 2024-10-24 13:07_

---

_Closed by @MichaReiser on 2024-10-24 13:07_

---

_Branch deleted on 2024-10-24 13:07_

---

_Comment by @github-actions[bot] on 2024-10-24 13:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
