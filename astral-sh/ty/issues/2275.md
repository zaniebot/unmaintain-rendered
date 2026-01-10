```yaml
number: 2275
title: "allowing `@type_check_only` subclasses of a `@final` class"
type: issue
state: closed
author: jorenham
labels: []
assignees: []
created_at: 2025-12-30T12:37:45Z
updated_at: 2025-12-30T13:33:16Z
url: https://github.com/astral-sh/ty/issues/2275
synced_at: 2026-01-10T01:56:41Z
```

# allowing `@type_check_only` subclasses of a `@final` class

---

_Issue opened by @jorenham on 2025-12-30 12:37_

NumPy's `ufunc` class is `@final`, but in order to meaningfully annotate its specializations, we use `@type_check_only` "phantom" subclasses that override e.g. `__call__` with tailor-made callable signatures. For example, `numpy.add` and `numpy.strings.isupper` are both instances of `numpy.ufunc`, but their callable signatures are very different.
This is also an issue in scipy-stubs, as there are even more ufuncs in `scipy.special` than in all of numpy. See for example https://github.com/scipy/scipy-stubs/blob/4b2ee9a7a917ebd8918caf1ab51146ba4898a34b/scipy-stubs/special/_ufuncs.pyi .

Somewhere in between 0.0.5 and 0.0.8, ty started reporting `subclass-of-final-class` for subclasses of `ufunc`, regardless of whether these subclasses are `@type_check_only`. But as far as I can tell, this cannot lead to any problems at runtime, so I don't think there's any utility in reporting this error in situations like this. Thoughts?

---

_Renamed from "allow `@type_check_only` subclass of `@final` class" to "allowing `@type_check_only` subclasses of a `@final` class" by @jorenham on 2025-12-30 12:37_

---

_Comment by @AlexWaygood on 2025-12-30 13:21_

I'm not sure we should do this. `@final` doesn't just signify to the user that you'll get a runtime error if you actually try to subclass that type. It also signifies to the type checker that they are "permitted" to make certain assumptions, simplifications, and optimisations that they cannot otherwise make. ty's special handling of `@final` classes permeates our whole architecture at this point. Here's a demo of some of the many ways in which we treat `@final` classes differently to other classes:

```py
from typing import final

@final
class A: ...
@final
class A2: ...
@final
class A3: ...

class B: ...
class C: ...
class D: ...

def f(t1: C, t2: type[B], t3: type[A], union1: A | A2 | A3, union2: B | C | D):
    if isinstance(t1, B):
        reveal_type(t1)  # C & B (a class could multiple-inherit from both)

    if isinstance(t1, A):
        reveal_type(t1)  # Never (no class could exist that simultaneously inherits from both)

    reveal_type(t2)  # type[B] (could be any subclass of B)
    reveal_type(t3)  # <class 'A'> (must be the class object `A` exactly, not a subclass)
    
    if isinstance(union1, A):
        reveal_type(union1)  # A
    elif isinstance(union1, A2):
        reveal_type(union1)  # A2
    else:
        reveal_type(union1)  # A3
    
    if isinstance(union2, B):
        reveal_type(union2)  # B
    elif isinstance(union2, C):
        reveal_type(union2)  # C & ~B  (any subtype of `C`, except for the possible
                             #          subtypes that overlap with `B`, as those must
                             #          have been excluded by the previous `if` check
    else:
        reveal_type(union2)  # D & ~B & ~C
```



---

_Comment by @AlexWaygood on 2025-12-30 13:29_

@jorenham, do any other type checkers accept these `@type_check_only` subclasses? 

---

_Comment by @jorenham on 2025-12-30 13:30_

> [@jorenham](https://github.com/jorenham), do any other type checkers accept these `@type_check_only` subclasses?

Pyrefly is the only one that doesn't complain, but that might just be a missing feature.

---

_Closed by @AlexWaygood on 2025-12-30 13:31_

---

_Comment by @jorenham on 2025-12-30 13:33_

But yea, this probably isn't wise to do then... It's too bad that there are no (official) intersection types, or a runtime-only `@final` or something. I guess I'll stick to `# ty:ignore`-ing them then for now, for the lack of a better alternative.

---
