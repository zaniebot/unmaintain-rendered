```yaml
number: 13513
title: "Parenthesize `match..case` `if` guards"
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/case-header-if-guard
created_at: 2024-09-25T16:17:30Z
updated_at: 2025-01-03T15:48:09Z
url: https://github.com/astral-sh/ruff/pull/13513
synced_at: 2026-01-12T15:55:44Z
```

# Parenthesize `match..case` `if` guards

---

_@MichaReiser_

## Summary

Implements the black `remove_redundant_guard_parens` and `parens_for_long_if_clauses_in_case_block` preview styles. 

## Black deviation

Like assignments, ruff prefers splitting the right side (if guard) over splitting the left (case).

**Black**
```python
match 1:
    case (
        _
    ) if True:  # comment over the line limit and parens should go to next line
        pass

    case {
        "long_long_long_key": str(long_long_long_key)
    } if value := "long long long long long long long long long long long value":
        pass
```

**Ruff**

```python
match 1:
    case _ if (
        True
    ):  # comment over the line limit and parens should go to next line
        pass

    case {"long_long_long_key": str(long_long_long_key)} if (
        value := "long long long long long long long long long long long value"
    ):
        pass
```

## Test Plan

There are a few existing tests that cover this already nicely. I added a few new tests to ensure that comments are handled correctly.


---

_Label `formatter` added by @MichaReiser on 2024-09-25 16:19_

---

_Label `preview` added by @MichaReiser on 2024-09-25 16:19_

---

_Comment by @github-actions[bot] on 2024-09-25 16:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @charliermarsh by @MichaReiser on 2024-09-25 16:40_

---

_Marked ready for review by @MichaReiser on 2024-09-25 16:40_

---

_@charliermarsh approved on 2024-09-25 22:56_

Nice.

---

_Merged by @MichaReiser on 2024-09-26 06:44_

---

_Closed by @MichaReiser on 2024-09-26 06:44_

---

_Branch deleted on 2024-09-26 06:44_

---

_Comment by @MichaReiser on 2025-01-03 15:48_

Ugh, why is there no ecosystem change... Do I now need to come up with a good example all by myself 😆 

---
