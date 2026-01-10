```yaml
number: 9396
title: Parenthesize breaking named expressions in match guards
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/parens
created_at: 2024-01-05T00:38:51Z
updated_at: 2024-01-08T14:51:54Z
url: https://github.com/astral-sh/ruff/pull/9396
synced_at: 2026-01-10T22:57:09Z
```

# Parenthesize breaking named expressions in match guards

---

_Pull request opened by @charliermarsh on 2024-01-05 00:38_

## Summary

This is an attempt to solve https://github.com/astral-sh/ruff/issues/9394 by avoiding breaks in named expressions when invalid.

---

_@charliermarsh reviewed on 2024-01-05 00:39_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:687 on 2024-01-05 00:39_

This is now parenthesized, whereas it wasn't before...

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:707 on 2024-01-05 00:39_

Black formats as:

```python
    case {
        "long_long_long_key": str(long_long_long_key)
    } if value := "long long long long long long long long long long long value":
        pass
```

---

_@charliermarsh reviewed on 2024-01-05 00:39_

---

_Label `bug` added by @charliermarsh on 2024-01-05 00:39_

---

_Label `formatter` added by @charliermarsh on 2024-01-05 00:39_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-01-05 00:39_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:707 on 2024-01-05 00:40_

I'm not quite sure how to solve this though, we'd need a way to tell the named expression to render without breaks. Does that exist?

---

_@charliermarsh reviewed on 2024-01-05 00:40_

---

_Comment by @github-actions[bot] on 2024-01-05 00:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser reviewed on 2024-01-05 01:28_

I feel the same as you. Maybe parenthesze is mainly intended to use for top level expressions, not for nested sub expressions.

I haven't looked at the issue (on my phone) but this is probably something that needs solving in the case formatting or we should use in_parentheses_only_softline_break in the if expression formatting 

---

_Comment by @charliermarsh on 2024-01-05 01:33_

Let me try `in_parentheses_only_softline_break`, that's interesting.

---

_Marked ready for review by @charliermarsh on 2024-01-05 02:12_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-01-05 02:12_

---

_Comment by @charliermarsh on 2024-01-05 02:13_

Ok this now looks right...

---

_Comment by @zanieb on 2024-01-05 02:16_

Nice!

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_named_expr.rs`:32 on 2024-01-08 08:13_

Nit
```suggestion
                group(&format_args![
                    target.format(),
                    in_parentheses_only_soft_line_break_or_space()
                ]),
```

---

_@MichaReiser approved on 2024-01-08 08:13_

---

_Comment by @MichaReiser on 2024-01-08 08:13_

Thank you

---

_Merged by @charliermarsh on 2024-01-08 14:47_

---

_Closed by @charliermarsh on 2024-01-08 14:47_

---

_Branch deleted on 2024-01-08 14:47_

---
