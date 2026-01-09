---
number: 20001
title: Rule to lint usages of mutable types with cached_property
type: issue
state: open
author: DavideCanton
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-08-20T10:19:36Z
updated_at: 2025-09-01T08:26:44Z
url: https://github.com/astral-sh/ruff/issues/20001
synced_at: 2026-01-07T13:12:16-06:00
---

# Rule to lint usages of mutable types with cached_property

---

_Issue opened by @DavideCanton on 2025-08-20 10:19_

### Summary

Would it make sense to have a rule to disallow using mutable containers with `@cached_property`?

I don't expect it to be able to detect all cases, but the builtin collections like `list`, `set` and `dict` should be easy to detect. It may also suggest a fix for list and set, respectively tuple and frozenset.

---

_Comment by @ntBre on 2025-08-20 12:51_

This would be a lint on the return type of the function decorated with `@cached_property`? That makes some sense to me and sounds related to our `mutable-*-default` rules, but it might be difficult to flag unless the function is annotated or with ty-style type inference.

---

_Label `rule` added by @ntBre on 2025-08-20 12:51_

---

_Label `needs-decision` added by @ntBre on 2025-08-20 12:51_

---

_Comment by @DavideCanton on 2025-09-01 08:26_

Correct! Probably a good starting point may be to disallow known mutable types like list, set and dict?

Another alternative (that AFAIK is what python dataclasses do) may be using "is hashable" as condition to disallow mutable default values for the fields, but I'm not really +1 on that and I don't know if ruff is able to do that check.

---
