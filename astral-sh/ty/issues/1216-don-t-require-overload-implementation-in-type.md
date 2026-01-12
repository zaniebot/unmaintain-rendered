```yaml
number: 1216
title: "don't require overload implementation in `TYPE_CHECKING` block / treat them the same as stub files"
type: issue
state: closed
author: KotlinIsland
labels:
  - overloads
assignees: []
created_at: 2025-09-20T07:12:41Z
updated_at: 2025-12-15T08:56:36Z
url: https://github.com/astral-sh/ty/issues/1216
synced_at: 2026-01-12T15:54:24Z
```

# don't require overload implementation in `TYPE_CHECKING` block / treat them the same as stub files

---

_@KotlinIsland_

### Summary

```py
from typing import TYPE_CHECKING, overload

if TYPE_CHECKING:
    @overload
    def f(x: int) -> int: ...

    @overload
    def f(x: str) -> str: ...  # Overloaded non-stub function `f` must have an implementation
```

### Version

_No response_

---

_Renamed from "don't require overload implementation in `TYPE_CHECKING` block" to "don't require overload implementation in `TYPE_CHECKING` block / treat them the same as stub files" by @KotlinIsland on 2025-09-20 07:13_

---

_Label `overloads` added by @sharkdp on 2025-09-22 09:26_

---

_Comment by @sharkdp on 2025-09-22 09:30_

Thank you for reporting this.

I agree that the implementation shouldn't necessarily appear within the `TYPE_CHECKING` block, but shouldn't there still be *some* implementation in a non-stub file? We would not issue a diagnostic if the implementation of `f` would follow somewhere later in that file.

Mypy, pyright, pyrefly all issue the same diagnostic here. Do you have a concrete use case?

---

_Comment by @AlexWaygood on 2025-09-22 12:13_

I'm sympathetic to this request. I think the most consistent way to treat `if TYPE_CHECKING` blocks (even if it's not what mypy/pyright do in all situations) is to treat them as "inlined stub files". Most special casing we apply to stub files should also be applied to `if TYPE_CHECKING` blocks, because these blocks of code (like stub files) are not executed at runtime, so not all the "normal" rules of runtime semantics need to apply to them in the same way.

> I agree that the implementation shouldn't necessarily appear within the `TYPE_CHECKING` block, but shouldn't there still be _some_ implementation in a non-stub file? We would not issue a diagnostic if the implementation of `f` would follow somewhere later in that file.

Right, but something like this seems reasonable? And seems quite analogous to having a stub file that lives separately to the "real" runtime implementation:

```py
if TYPE_CHECKING:
    @overload
    def f(a: int) -> str: ...
    @overload
    def f(a: str) -> int: ...
else:
    def f(a: int | str) -> int | str: ...
```

---

_Comment by @sharkdp on 2025-09-22 12:37_

> Right, but something like this seems reasonable?

Maybe? But now it even looks like you're actively trying to hide the implementation from your type checker (it has been argued elsewhere that a typechecker potentially shouldn't even look at the `else` branch of a `if TYPE_CHECKING` construct).

---

_Comment by @AlexWaygood on 2025-09-22 12:46_

> it has been argued elsewhere that a typechecker potentially shouldn't even look at the `else` branch of a `if TYPE_CHECKING` construct)

Right! That's why I'm saying that we shouldn't emit an error if there's no overload implementation when the overloads are in an `if TYPE_CHECKING` block. The implementation might be "invisible" to us, because it might be in an `else` block, or an `if not TYPE_CHECKING` block.

It's the same reason why we don't emit an error for there being no overload implementation in a `.pyi` file -- these overloads do have implementations, but the implementations are "invisible" to us when we're analysing the `.pyi` file. They're in a totally different file (a `.py` file).

---

_Comment by @sharkdp on 2025-09-22 13:20_

I don't really see the stub file analogy. A stub file is provided as a supplement, or potentially in an entirely different (`xyz-types`) package. There's a reason why the implementation is not part of the stub.

But if you are adding type annotations to the very same file that the implementation lives in, then why would you actively try to hide the implementation from the type checker? Why is it inside the `else` block? 

---

_Comment by @KotlinIsland on 2025-09-22 13:26_

> I think the most consistent way to treat `if TYPE_CHECKING` blocks

that's how i handled them in basedmypy

i think the case of "there should be something in the other branch" is a separate issue

i consider `if TYPE_CHECKING` to be a primitive, with structures built on-top of it, not into it

we could have a separate rule "symbol declared within TYPE_CHECKING requires a declaration outside of it", there are many use cases for `TYPE_CHECKING`, and to presume that they __all__ need a definition isn't quite right to me

---

_Comment by @KotlinIsland on 2025-09-22 13:27_

i've had many times where i needed to define overloads within `TYPE_CHECKING` for one reason or another

---

_Comment by @AlexWaygood on 2025-09-22 13:27_

I think the advantage of consistently treating `if TYPE_CHECKING` blocks as "inlined stub files" everywhere is it provides an easy way to understand our behaviour and it makes it easy to explain our behaviour to users in issues and documentation.

I also think that using `if not TYPE_CHECKING` blocks to hide highly dynamic code from a type checker is a valid use of the feature. Not all Python plays well with static type-checking; on some functions, a type checker is going to emit a prohibitive number of false positives, and you just want to silence all errors in that function. But maybe we should just say that the way you should deal with that is to add `@no_type_check` to the implementation function? I do agree that _usually_ there's no good reason to hide an overload implementation from a type checker.

---

_Closed by @AlexWaygood on 2025-12-15 08:56_

---
