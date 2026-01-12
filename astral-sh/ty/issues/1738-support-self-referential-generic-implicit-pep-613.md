```yaml
number: 1738
title: Support self-referential generic implicit/PEP-613 type aliases
type: issue
state: open
author: sharkdp
labels:
  - generics
  - type aliases
assignees: []
created_at: 2025-12-03T08:40:12Z
updated_at: 2025-12-26T12:25:43Z
url: https://github.com/astral-sh/ty/issues/1738
synced_at: 2026-01-12T15:54:25Z
```

# Support self-referential generic implicit/PEP-613 type aliases

---

_@sharkdp_

We currently handle self-referential generic implicit (and PEP 613) type aliases by [falling back to `Divergent`](https://github.com/astral-sh/ruff/blob/f4e4229683936a486f689aebb9d7d06b3985952d/crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md?plain=1#L1532-L1548). This prevents infinite recursion and false positive diagnostics, but we should properly support them. For example, this should be an error, but currently is not:
```py
from typing import TypeVar

T = TypeVar("T")
NestedDict = dict[str, "NestedDict[T]"]

n: NestedDict[int] = {"foo": b"wrong"}
```

An example case where this is relied on in typeshed is the annotation for the second argument to `isinstance`, which is a recursive tuple-of-tuples. Currently we don't catch the type error in `isinstance(None, (int, None))` (though we do catch it in `isinstance(None, None)`), due to not fully supporting the recursion.

(Note: we [already support this](https://play.ty.dev/22eb39cd-098f-4b84-80e9-70bc088740d5) for PEP 695 type aliases)

---

_Label `generics` added by @sharkdp on 2025-12-03 08:40_

---

_Renamed from "Support self-referential generic implicit type aliases" to "Support self-referential generic implicit/PEP-613 type aliases" by @sharkdp on 2025-12-03 09:41_

---

_Added to milestone `Stable` by @sharkdp on 2025-12-03 09:45_

---

_Label `type aliases` added by @AlexWaygood on 2025-12-19 12:11_

---

_Comment by @mtshiba on 2025-12-26 12:13_

If we can support implicit recursive type aliases, we should also be able to automatically support PEP-613-style type aliases.
To support implicit recursive type aliases, they must be lazily evaluated if named.

---

_Assigned to @mtshiba by @mtshiba on 2025-12-26 12:25_

---
