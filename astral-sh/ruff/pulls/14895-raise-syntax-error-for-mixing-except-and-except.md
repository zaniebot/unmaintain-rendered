```yaml
number: 14895
title: "Raise syntax error for mixing `except` and `except*`"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: star-except
created_at: 2024-12-10T17:56:58Z
updated_at: 2024-12-10T23:51:01Z
url: https://github.com/astral-sh/ruff/pull/14895
synced_at: 2026-01-12T15:55:49Z
```

# Raise syntax error for mixing `except` and `except*`

---

_@dylwil3_

This PR adds a syntax error if the parser encounters a `TryStmt` that has except clauses both with and without a star.

The displayed error points to each except clause that contradicts the original except clause kind. So, for example,

```python
try:
    ....
except:     #<-- we assume this is the desired except kind
    ....
except*:    #<---  error will point here
    ....
except*:    #<--- and here
    ....
```

Closes #14860


---

_Comment by @github-actions[bot] on 2024-12-10 18:05_

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

_Label `parser` added by @AlexWaygood on 2024-12-10 18:32_

---

_Label `bug` added by @AlexWaygood on 2024-12-10 18:32_

---

_Marked ready for review by @dylwil3 on 2024-12-10 19:29_

---

_Review requested from @MichaReiser by @dylwil3 on 2024-12-10 19:29_

---

_Review requested from @dhruvmanila by @dylwil3 on 2024-12-10 19:29_

---

_Comment by @dylwil3 on 2024-12-10 19:31_

It's not clear to me if it would be better to just put a single displayed error on the entire `TryStmt`? Happy to change it.

---

_Renamed from "[WIP] Raise syntax error for mixing `except` and `except*`" to "Raise syntax error for mixing `except` and `except*`" by @dylwil3 on 2024-12-10 19:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1373 on 2024-12-10 21:39_

Nit
```suggestion
            if is_star.is_none() {
                is_star = Some(kind.is_star());
            } else if is_star != Some(kind.is_star()) {
                mixed_except_ranges.push(handler.range());
            }
            handler
        });
```

---

_@MichaReiser approved on 2024-12-10 21:41_

I think the syntax error is fine the way it is. Thank you

---

_Merged by @dylwil3 on 2024-12-10 23:50_

---

_Closed by @dylwil3 on 2024-12-10 23:50_

---

_Branch deleted on 2024-12-10 23:51_

---
