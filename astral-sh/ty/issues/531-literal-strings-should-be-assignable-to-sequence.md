```yaml
number: 531
title: "Literal strings should be assignable to `Sequence[Unknown]`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - generics
assignees: []
created_at: 2025-05-28T09:15:43Z
updated_at: 2025-06-01T15:39:57Z
url: https://github.com/astral-sh/ty/issues/531
synced_at: 2026-01-12T15:54:23Z
```

# Literal strings should be assignable to `Sequence[Unknown]`

---

_@sharkdp_

Found this while analyzing common ecosystem false positives. I think this should work?
```py
from typing import Sequence, Any

seq: Sequence[Any] = "abc"  #  `Literal["abc"]` is not assignable to `Sequence[Any]`
```
https://play.ty.dev/1059069f-082c-4e2f-a643-92b3f0d8583c

Both of these cases work:
```py
seq: Sequence[Any] = some_str
seq: Sequence[str] = "abc"
```

---

_Label `bug` added by @sharkdp on 2025-05-28 09:15_

---

_Label `generics` added by @sharkdp on 2025-05-28 09:15_

---

_Comment by @MatthewMckee4 on 2025-05-28 10:33_

Should literal strings be subtypes of `Sequence[Unknown]`?

---

_Comment by @AlexWaygood on 2025-05-28 10:44_

nothing should be a subtype of `Sequence[Unknown]` because it's not a fully static type; only fully static types participate in subtyping

---

_Comment by @MatthewMckee4 on 2025-05-28 10:45_

ah yeah sorry, thanks

---

_Comment by @MatthewMckee4 on 2025-05-28 10:47_

Okay, ill submit a PR for this

---

_Closed by @AlexWaygood on 2025-06-01 15:39_

---
