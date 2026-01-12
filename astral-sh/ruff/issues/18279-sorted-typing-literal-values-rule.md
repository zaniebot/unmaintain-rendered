```yaml
number: 18279
title: "Sorted `typing.Literal` Values Rule"
type: issue
state: open
author: TimothyWillard
labels:
  - rule
  - needs-design
assignees: []
created_at: 2025-05-23T15:26:07Z
updated_at: 2025-05-25T15:32:20Z
url: https://github.com/astral-sh/ruff/issues/18279
synced_at: 2026-01-12T15:54:56Z
```

# Sorted `typing.Literal` Values Rule

---

_@TimothyWillard_

### Summary

It would be helpful for consistency in function signatures to have sorted literals. For example:

```python
# Okay because `sorted(["a", "b", "c"]) == ['a', 'b', 'c']`
def f(x: Literal["a", "b", "c"]) -> None:
  print(x)

# Warning because `sorted(["c", "b", "a"]) != ["c", "b", "a"]`
def g(x: Literal["c", "b", "a"]) -> int:
  return ["a", "b", "c"].index(x)
```

This is similar to [RUF022](https://docs.astral.sh/ruff/rules/unsorted-dunder-all/), but in a different context. I think there are conceptual complications with literals of mixed types (i.e. `Literal[0, 1, "a", "b"]`) that I don't have great thoughts on how to handle. Maybe skip for now or only apply the rule to strings? As far as I know this is not implemented by any other formatters/linters.

---

_Comment by @MichaReiser on 2025-05-25 10:56_

Thank you for this rule suggestion. 

This is a very opinionated rule in the sense that the only motivation is consistency/I like it better this way. We're currently not adding any such new rules until we complete https://github.com/astral-sh/ruff/issues/1774 which will give us a dedicated category for such rules.  

---

_Label `rule` added by @MichaReiser on 2025-05-25 10:56_

---

_Label `needs-design` added by @MichaReiser on 2025-05-25 10:56_

---

_Comment by @TimothyWillard on 2025-05-25 15:32_

That's a very fair characterization, I'll keep an eye on #1774. Thanks!

---
