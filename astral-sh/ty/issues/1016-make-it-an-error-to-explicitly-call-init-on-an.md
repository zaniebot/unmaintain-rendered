```yaml
number: 1016
title: "make it an error to explicitly call `__init__` on an existing object"
type: issue
state: open
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-08-15T21:36:13Z
updated_at: 2025-10-06T16:39:17Z
url: https://github.com/astral-sh/ty/issues/1016
synced_at: 2026-01-12T15:54:24Z
```

# make it an error to explicitly call `__init__` on an existing object

---

_@carljm_

This is unsafe because Liskov is not enforced on `__init__` methods. It also makes the exclusion of `__init__` from variance-inference unsafe.

---

_Added to milestone `GA` by @carljm on 2025-08-15 21:36_

---

_Comment by @AlexWaygood on 2025-08-15 21:41_

You could argue it's safe from a liskov perspective on an instance of a final class? But maybe the variance thing still holds 

---

_Comment by @carljm on 2025-08-15 21:44_

Yes, the variance thing is still an issue for a generic class, even if its final.

We could in theory allow it for final non-generic classes, but I'm really not sure that's worth the extra complexity, for something that's rarely done (and never a good idea) anyway. In any case where you really want to do this, you can extract the stuff you want to re-call into a separate method called from `__init__`.

---

_Comment by @MatthewMckee4 on 2025-08-17 20:54_

why is Liskov not enforced on `__init__` methods?

---

_Comment by @AlexWaygood on 2025-08-17 21:44_

> why is Liskov not enforced on `__init__` methods?

I had the same question a few years ago! There's some great answers here: https://softwareengineering.stackexchange.com/questions/431549/the-liskov-substitution-principle-and-python/

---

_Comment by @MatthewMckee4 on 2025-08-17 23:25_

Thank you, that makes perfect sense

---

_Label `typing semantics` added by @AlexWaygood on 2025-10-06 16:39_

---
