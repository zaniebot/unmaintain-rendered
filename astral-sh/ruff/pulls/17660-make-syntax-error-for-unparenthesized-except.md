```yaml
number: 17660
title: Make syntax error for unparenthesized except tuples version specific to before 3.14
type: pull_request
state: merged
author: dylwil3
labels:
  - parser
  - python314
assignees: []
merged: true
base: main
head: unparenthesized-except
created_at: 2025-04-27T21:51:38Z
updated_at: 2025-04-29T12:55:32Z
url: https://github.com/astral-sh/ruff/pull/17660
synced_at: 2026-01-12T15:56:03Z
```

# Make syntax error for unparenthesized except tuples version specific to before 3.14

---

_@dylwil3_

What it says on the tin 😄 

---

_Label `parser` added by @dylwil3 on 2025-04-27 21:51_

---

_Label `python314` added by @dylwil3 on 2025-04-27 21:51_

---

_Comment by @github-actions[bot] on 2025-04-27 21:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-27 22:01_

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

_Marked ready for review by @dylwil3 on 2025-04-28 21:40_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-04-28 21:40_

---

_Review requested from @dhruvmanila by @dylwil3 on 2025-04-28 21:40_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:850 on 2025-04-28 23:20_

bikeshedding: what about `UnparenthesizedExceptionTypes` which is what we also use in the diagnostic message? No strong preference, what you have works as well.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1596 on 2025-04-28 23:22_

nit: ref: https://github.com/python/cpython/blob/68a737691b0fd591de00f4811bb23a5c280fe859/Grammar/python.gram#L1370
```suggestion
                            "Multiple exception types must be parenthesized when using `as`"
```

---

_@dhruvmanila approved on 2025-04-28 23:31_

Looks good!

---

_@MichaReiser approved on 2025-04-29 06:16_

Not directly related to the PR itelf but related to the syntax change. We should decide if we want to change the formatter when targeting Python 3.14 to preserve parentheses, always remove them when they fit on a single line, or always add them (what it currently does)

---

_Merged by @dylwil3 on 2025-04-29 12:55_

---

_Closed by @dylwil3 on 2025-04-29 12:55_

---
