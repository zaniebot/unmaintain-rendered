```yaml
number: 1797
title: Confusing diagnostic for assignment with trailing comma
type: issue
state: open
author: Gankra
labels:
  - diagnostics
assignees: []
created_at: 2025-12-07T20:11:36Z
updated_at: 2025-12-07T20:14:51Z
url: https://github.com/astral-sh/ty/issues/1797
synced_at: 2026-01-12T15:54:25Z
```

# Confusing diagnostic for assignment with trailing comma

---

_@Gankra_

https://play.ty.dev/2fdc0a03-ef37-44f1-8074-f8558ff7a582

```py
coords: tuple[int, int] = (0, 0),
```

> Object of type `tuple[tuple[Literal[0], Literal[0]]]` is not assignable to `tuple[int, int]` (invalid-assignment) 

I ran into this while turning a class into a `@dataclass`, and almost filed a bug about dataclasses not computing assignability correctly... and only while writing this just now did I process that that says `tuple[tuple[...`.

This message is totally correct (this value prints `((0, 0),)`) but I wonder if we could notice this situation and be like "hey you have a trailing comma, did you... mean that?"

---

_Label `diagnostics` added by @Gankra on 2025-12-07 20:11_

---

_Comment by @MichaReiser on 2025-12-07 20:14_

You should give ruff format a try

---
