```yaml
number: 665
title: Return intersection instead of Any for ambiguous overload match
type: issue
state: open
author: dhruvmanila
labels:
  - needs-design
  - overloads
  - typing semantics
assignees: []
created_at: 2025-06-17T04:55:21Z
updated_at: 2025-11-18T16:10:32Z
url: https://github.com/astral-sh/ty/issues/665
synced_at: 2026-01-12T15:54:23Z
```

# Return intersection instead of Any for ambiguous overload match

---

_@dhruvmanila_

Consider the following example:

`overloaded.pyi`:

```pyi
from typing import overload
from typing_extensions import LiteralString

@overload
def f(x: LiteralString) -> LiteralString: ...
@overload
def f(x: str) -> str: ...
```

```py
from typing import Any
from typing_extensions import LiteralString

from overloaded import f

def _(literal: LiteralString, string: str, any: Any):
    reveal_type(f(literal))  # revealed: LiteralString
    reveal_type(f(string))  # revealed: str

    # `Any` matches both overloads, but the return types are not equivalent.
    # Pyright and mypy both reveal `str` here, contrary to the spec.
    reveal_type(f(any))  # revealed: Any
```

For the last call, ty reveals `Any` while Pyright and mypy reveals `str`. The reason ty reveals `Any` is because the overload matching is ambiguous according to the spec. Here's how the algorithm detects that:

1. Arity does not eliminate any overloads
2. Type checking does not eliminate any overloads
3. Step 3 is skipped since step 2 did not result in errors for all overloads
4. Step 4 has no effect, since neither overload has a variadic parameter
5. All materialization of `Any` are not assignable to either of the overloads since it could materialize into anything other than `LiteralString` or `str`. The return types of the remaining overloads are not equivalent so the overload matching is ambiguous.

More formally, at the end of step 5, before concluding that the overload matching is ambiguous, ty should check whether the return types are overlapping and return the widest type of them instead of `Any`.

I'm not exactly sure if this is what Pyright / mypy does so it might be useful to hear from someone who knows about this.

---

_Label `needs-design` added by @dhruvmanila on 2025-06-17 04:55_

---

_Label `overloads` added by @dhruvmanila on 2025-06-17 04:55_

---

_Label `typing semantics` added by @dhruvmanila on 2025-06-17 05:18_

---

_Comment by @erictraut on 2025-06-17 15:09_

> I'm not exactly sure if this is what Pyright / mypy does so it might be useful to hear from someone who knows about this.

See [this comment](https://github.com/astral-sh/ty/issues/552#issuecomment-2959627420) for more details.

---

_Comment by @carljm on 2025-06-17 15:16_

On second thought, I think perhaps we should not make this change (especially given that Eric intends to change pyright behavior to match the spec.) Even when the return types do have a subtype relationship, assuming the wider type still does violate the gradual guarantee and can still create false positives. Perhaps the calling code does expect/need the `LiteralString` return, but because the argument is typed as `Unknown` we get `str` -- this can cause false positives.

---

_Comment by @jelle-openai on 2025-06-17 16:29_

Returning Any feels unfortunate though in cases like [Carl's example](https://github.com/astral-sh/ty/issues/552#issuecomment-2959444394), where `'%s %s' % (Any, Any)` gets inferred as Any. Users would surely expect type checkers to understand this is a str.

I think if we had intersection support, we could do better here while preserving the gradual guarantee. In this example, we could infer `str & Any`, which means `str` or some unknown subtype of `str`, and is therefore assignable to `LiteralString`. More generally when there are multiple possible return types that are not subtypes of each other, we could infer `AnyOf[T1, T2]`, which I [showed](https://github.com/python/typing/issues/566#issuecomment-2972730463) can be implemented using intersections. However, this would require a change to the spec.

---

_Renamed from "Consider checking for subtyping for ambiguous overload matching" to "Return intersection instead of Any for ambiguous overload match" by @carljm on 2025-11-14 15:07_

---

_Comment by @carljm on 2025-11-14 15:08_

I think that using intersection instead of `Any` is the right approach here, and we could certainly do that, but it will decrease our spec compliance unless we first land an update to the spec allowing this option.

I think this is interesting but not an immediate priority.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 15:08_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
