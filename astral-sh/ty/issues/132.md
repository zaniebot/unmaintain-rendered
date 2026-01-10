```yaml
number: 132
title: "Consider custom `getattr_static` method"
type: issue
state: open
author: dcreager
labels:
  - internal
assignees: []
created_at: 2025-04-09T13:51:35Z
updated_at: 2025-11-18T16:10:20Z
url: https://github.com/astral-sh/ty/issues/132
synced_at: 2026-01-10T02:06:24Z
```

# Consider custom `getattr_static` method

---

_Issue opened by @dcreager on 2025-04-09 13:51_

In https://github.com/astral-sh/ruff/pull/17023/files#r2033191626 @sharkdp called out that with generic classes, our implementation of `getattr_static` does not match its runtime behavior, since at runtime, generic aliases are instances of the special `_GenericAlias` class. We treat generic aliases as a specialized instance of the underlying class literal.

This `getattr_static` behavior is arguably better for our type checker, but @carljm suggested that we might consider a custom `getattr_static` in `knot_extensions` (instead of piggy-backing on the name of the runtime's `inspect.getattr_static`) to clarify this difference in behavior.

---

_Comment by @sharkdp on 2025-04-09 15:34_

I'm personally fine with the deviation that we currently have, i.e. I'm not convinced that we need to do anything here. If anyone wants to work on this, I think it should be discussed first.

---

_Renamed from "[red-knot] Consider custom `getattr_static` method" to "Consider custom `getattr_static` method" by @MichaReiser on 2025-05-07 15:25_

---

_Label `internal` added by @AlexWaygood on 2025-05-11 07:39_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:24_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
