```yaml
number: 1516
title: Omit type inlay hints for initializer calls
type: issue
state: closed
author: Gankra
labels:
  - server
assignees: []
created_at: 2025-11-10T16:04:55Z
updated_at: 2025-12-08T15:54:32Z
url: https://github.com/astral-sh/ty/issues/1516
synced_at: 2026-01-10T01:56:40Z
```

# Omit type inlay hints for initializer calls

---

_Issue opened by @Gankra on 2025-11-10 16:04_

Maybe there's some corner cases where it is important, but it's really not helpful to tell me a Rect(..) call produced something of type Rect:

<img width="425" height="53" alt="Image" src="https://github.com/user-attachments/assets/393c8a27-e25e-4f9b-918d-ade88756f72a" />

---

_Added to milestone `GA` by @Gankra on 2025-11-10 16:04_

---

_Label `server` added by @Gankra on 2025-11-10 16:04_

---

_Comment by @AlexWaygood on 2025-11-10 16:50_

Not sure about this -- they seem pretty useful for generic types where type parameters are inferred from initializer arguments?

<img width="1244" height="376" alt="Image" src="https://github.com/user-attachments/assets/a5113dc1-8b04-41c7-acbd-a73125291f9a" />

And when we implement https://github.com/astral-sh/ty/issues/281, they'll be useful for pointing out that not all initializer calls actually return an instance of the class at all!

I do agree that they seem a bit redundant for what's maybe the most common case, though (nongeneric classes that return an instance of the class from `__new__`).

---

_Comment by @MichaReiser on 2025-11-14 08:16_

I'm ovarall leaning to hide an inlay when it's unclear whether it's useful as the same information can always be retrieved by hovering but there's no option to suppress an inlay. That's why I think we should implement this (excluding generics) and we can keep iterating on it

---

_Assigned to @Gankra by @Gankra on 2025-11-24 15:08_

---

_Comment by @AlexWaygood on 2025-12-08 13:00_

I think we should also hide the inlay for "initializer-like" `NewType` calls. The `MinimizedSourceCode` inlays here don't feel useful:

<img width="1890" height="662" alt="Image" src="https://github.com/user-attachments/assets/c5923546-38bb-426c-8de0-43a46c9a96dc" />

`MinimizedSourceCode` is defined at the top of the file with:

```py
from typing import NewType

MinimizedSourceCode = NewType("MinimizedSourceCode", int)
```

---

_Closed by @Gankra on 2025-12-08 15:54_

---
