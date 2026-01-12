```yaml
number: 1686
title: Behaviour of Any subclasses when mocking final classes
type: issue
state: open
author: hauntsaninja
labels:
  - needs-decision
assignees: []
created_at: 2025-11-30T07:53:42Z
updated_at: 2025-12-02T03:00:01Z
url: https://github.com/astral-sh/ty/issues/1686
synced_at: 2026-01-12T15:54:25Z
```

# Behaviour of Any subclasses when mocking final classes

---

_@hauntsaninja_

### Summary

https://play.ty.dev/758115b1-501a-4a59-9cff-5e4de80fab1c

```py
from typing import Any, final

class Mock(Any): ...

def takes_bool(x: bool): ...
takes_bool(Mock())

class AX: ...
def takes_ax(x: AX): ...
takes_ax(Mock())

@final
class FX: ...
def takes_fx(x: FX): ...
takes_fx(Mock())
```

I guess I understand why ty is doing the thing it is currently doing, but curious how you would spell this kind of pattern in a way that works for ty (maybe the answer is just pull out `if TYPE_CHECKING`?)

---

_Label `question` added by @sharkdp on 2025-12-01 08:44_

---

_Comment by @sharkdp on 2025-12-01 09:00_

> I guess I understand why ty is doing the thing it is currently doing

Yes, it seems correct to insist that `Mock` could not possibly be a subclass of a final class.

> curious how you would spell this kind of pattern in a way that works for ty (maybe the answer is just pull out if TYPE_CHECKING?)

I'm afraid something like
```py
class Mock: ...

if TYPE_CHECKING:
    Mock = cast(Any, Mock)
```
might be necessary at the moment. It would be cool if we could do something like:
```py
def mock_class[C](cls: C) -> Intersection[C, Any]:
    return cls

@mock_class
class Mock: ...
```
That would give you more precise types and would enable things like auto-completion on mock objects. Unfortunately, there are a few things that we do not support here:
* We do not support arbitrary class decorators (that should hopefully be easy to add)
* We do not support intersection types in the generics solver
* We do not support instantiation of intersection types, so you currently get a `@Todo` type if you instantiate a `Mock & Any`

---

_Label `needs-decision` added by @carljm on 2025-12-02 02:57_

---

_Label `question` removed by @carljm on 2025-12-02 02:57_

---

_Added to milestone `Stable` by @carljm on 2025-12-02 02:58_

---

_Comment by @carljm on 2025-12-02 03:00_

Putting this in stable milestone with `needs-decision` tag. By stable we should either have a recommended pattern here (ideally one that doesn't require `cast`), or (if it seems this will impact a lot of people) we could decide to go for pragmatism and match other type checkers.

---
