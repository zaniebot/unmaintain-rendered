```yaml
number: 20510
title: PLR6201 should only apply to hashable types
type: issue
state: closed
author: JoElfner
labels:
  - documentation
assignees: []
created_at: 2025-09-22T08:45:49Z
updated_at: 2025-10-01T17:58:25Z
url: https://github.com/astral-sh/ruff/issues/20510
synced_at: 2026-01-10T11:09:59Z
```

# PLR6201 should only apply to hashable types

---

_Issue opened by @JoElfner on 2025-09-22 08:45_

### Summary

# Ruff version

ruff 0.12.12

# Ruff settings

PLR6201 enabled

# Problem

Fix safety description of PLR6201 is misleading:

> [Fix safety](https://docs.astral.sh/ruff/rules/literal-membership/#fix-safety)
> This rule's fix is marked as unsafe, as the use of a set literal will error at runtime if the sequence contains unhashable elements (like lists or dictionaries). While Ruff will attempt to infer the hashability of the elements, it may not always be able to do so.

This reads as if **only** the hashability of the rhs-sequence was the problem. But in my opinion, the hashability of the lhs is more problematic in most use cases. F.i.

```
def foo(val: str | list[str] | dict[str, str]) -> bool:
    return val in {"a", "b", "c"}
```

### Version

ruff 0.12.12

---

_Comment by @ntBre on 2025-09-22 16:14_

Thanks for the report! It looks like this is already the case in the rule's implementation, so we should definitely update the docs to reflect that.

https://github.com/astral-sh/ruff/blob/00a9e65d001082972f9c83ce8f405046985a26c6/crates/ruff_linter/src/rules/pylint/rules/literal_membership.rs#L73-L75

---

_Label `documentation` added by @ntBre on 2025-09-22 16:14_

---

_Comment by @JoElfner on 2025-09-23 10:51_

thanks!

---

_Closed by @ntBre on 2025-10-01 17:58_

---
