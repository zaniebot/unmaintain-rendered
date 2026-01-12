```yaml
number: 18851
title: Which bare tuples are in scope for COM818?
type: issue
state: open
author: dscorbett
labels:
  - documentation
  - rule
  - help wanted
assignees: []
created_at: 2025-06-21T15:57:15Z
updated_at: 2025-06-23T14:46:29Z
url: https://github.com/astral-sh/ruff/issues/18851
synced_at: 2026-01-12T15:54:56Z
```

# Which bare tuples are in scope for COM818?

---

_@dscorbett_

### Summary

[`trailing-comma-on-bare-tuple` (COM818)](https://docs.astral.sh/ruff/rules/trailing-comma-on-bare-tuple/) is documented as:
> Checks for the presence of trailing commas on bare (i.e., unparenthesized) tuples.

but it only checks for a comma before a statement-final newline. The documentation should be more precise as to which bare tuples are in scope for this rule.

Bare tuples in subscripts are subject to [`incorrectly-parenthesized-tuple-in-subscript` (RUF031)](https://docs.astral.sh/ruff/rules/incorrectly-parenthesized-tuple-in-subscript/). They are out of scope for COM818.

COM818 does not check bare tuples in `yield` expressions. Maybe it should, or maybe this should be called out as another exception in the documentation.
```python
def f(x): x.append((yield x[-1],))
```

COM818 should also check for a comma before a semicolon, as in this bare tuple:
```python
print("Hello,"),;
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17)

---

_Label `documentation` added by @ntBre on 2025-06-23 12:17_

---

_Label `rule` added by @ntBre on 2025-06-23 12:17_

---

_Label `help wanted` added by @ntBre on 2025-06-23 12:17_

---

_Comment by @ntBre on 2025-06-23 12:18_

Thanks! We should update the documentation at the very least (including a mention of RUF031), but I think it also makes sense to extend the rule to handle the last two examples.

---

_Comment by @MichaReiser on 2025-06-23 14:46_

We should double-check what the upstream lint rule does before changing the rule (and see if there's a reason or if it was just overlooked)

---
