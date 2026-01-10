```yaml
number: 14968
title: "RUF052: Don't flag assignments to private function arguments"
type: issue
state: closed
author: aiudirog
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2024-12-14T06:36:32Z
updated_at: 2025-01-10T09:09:27Z
url: https://github.com/astral-sh/ruff/issues/14968
synced_at: 2026-01-10T11:09:56Z
```

# RUF052: Don't flag assignments to private function arguments

---

_Issue opened by @aiudirog on 2024-12-14 06:36_

This is a follow up to #14796: attempting to assign a private function parameter still gets flagged in Ruff v0.8.3 ([playground](https://play.ruff.rs/e233076b-9184-492f-aa36-94306356bf12))

My primary use case for private parameters is when I need to track what I've seen when recursing through a potentially circular graph:

```python
class Node:

    connected: list[Node]

    def recurse(self, *, _seen: set[Node] | None = None):
        if _seen is None:
            _seen = set()  # Local dummy variable `_seen` is accessed (RUF052)
        elif self in _seen:
            return
        _seen.add(self)
        for other in self.connected:
            other.recurse(_seen=_seen)
```

In most cases, I wouldn't want the caller to have to seed the `_seen` param or even know that it exists, as imo, that's just an implementation detail.

---

_Label `rule` added by @dylwil3 on 2024-12-14 07:04_

---

_Label `help wanted` added by @AlexWaygood on 2024-12-14 13:27_

---

_Label `accepted` added by @AlexWaygood on 2024-12-14 13:27_

---

_Assigned to @dylwil3 by @dylwil3 on 2024-12-14 18:49_

---

_Comment by @dylwil3 on 2024-12-14 18:51_

Incidentally, in this example the rule is _not_ complaining about the binding for the function parameter, but rather about the rebinding in the if-statement. So I think we need to add some logic that skips if the offending binding is a re-binding of a private parameter declaration.

Unrelated - would it be more helpful if the diagnostic pointed to the problematic _use_ rather than the _binding_?

---

_Comment by @aiudirog on 2024-12-14 19:24_

> Incidentally, in this example the rule is _not_ complaining about the binding for the function parameter, but rather about the rebinding in the if-statement. So I think we need to add some logic that skips if the offending binding is a re-binding of a private parameter declaration.

Yeah on 0.8.2 the rule flagged both the function definition and the reassignment in the body separately, so that seems like the appropriate solution.

> Unrelated - would it be more helpful if the diagnostic pointed to the problematic _use_ rather than the _binding_?

That might just flip it to error on `_seen.add(self)` and, if there's a lot of usages in the same function, could output a lot of messages

---

_Closed by @dylwil3 on 2025-01-10 09:09_

---
