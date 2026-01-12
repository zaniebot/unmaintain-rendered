```yaml
number: 692
title: too many cycle iterations in cyclic dependent attributes
type: issue
state: closed
author: Glyphack
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-06-23T07:01:07Z
updated_at: 2025-08-28T12:34:50Z
url: https://github.com/astral-sh/ty/issues/692
synced_at: 2026-01-12T15:54:23Z
```

# too many cycle iterations in cyclic dependent attributes

---

_@Glyphack_

### Summary

This was found by mypy primer. The original code is in this file:
https://github.com/caronc/apprise/blob/ce90151051f630803cba755fc9f06e16f7d8590b/apprise/plugins/email/base.py#L244

I think this is a minimal reproduction. Although the `secure` attribute is defined in parent class in the original code but while playing this code fails(with defined it fails as well I just don't know why):
```py
from typing import Self

class A:
    def __init__(self: Self, secure_mode=None):
        if secure_mode:
            self.secure_mode = secure_mode.lower()
        else:
            self.secure_mode = "insecure" \
                if not self.secure else "ssl"

        if self.secure_mode not in ["secure"]:
            raise TypeError("invalid")

        # Next line is when the member lookup happens and the cycle starts
        if not self.secure and self.secure_mode != "insecure":
            self.secure = True
```

The cycle happens in member lookup with policy on `self.secure` The fixed point iteration does not converge. These are the two values that are returned from `member_lookup_with_policy`:
```
member_lookup_with_policy: PlaceAndQualifiers {
    place: Unbound,
    qualifiers: TypeQualifiers(
        0x0,
    ),
}

member_lookup_with_policy: PlaceAndQualifiers {
    place: Type(
        Union(
            UnionType {
                elements: [
                    Dynamic(
                        Unknown,
                    ),
                    BooleanLiteral(
                        true,
                    ),
                ],
            },
        ),
        PossiblyUnbound,
    ),
    qualifiers: TypeQualifiers(
        0x0,
    ),
}
```

### Version

9570d39f9 2025-06-23

---

_Label `bug` added by @AlexWaygood on 2025-06-23 07:06_

---

_Label `fatal` added by @AlexWaygood on 2025-06-23 07:06_

---

_Renamed from "too many cycle iterations in dependent attributes" to "too many cycle iterations in cyclic dependent attributes" by @Glyphack on 2025-06-23 07:06_

---

_Comment by @dcreager on 2025-06-23 21:06_

Removing the explicit `: Self` annotation also causes this to succeed [[playground](https://play.ty.dev/ef637619-2d17-415d-b3f9-b870c0d38f63)]

---

_Comment by @dcreager on 2025-06-23 21:15_

And removing the ternary operator (but keeping the `Self` annotation) succeeds [[playground](https://play.ty.dev/ef637619-2d17-415d-b3f9-b870c0d38f63)]

So the cycle looks like it's because the definitions of `self.secure` and `self.secure_mode` are each guarded by an `if` statement involving the other.  Removing the ternary breaks the cycle since the definition of `self.secure_mode` is no longer guarded by a reference to `self.secure`.  I'm not sure yet why removing the `Self` annotation helps. Presumably because there's a salsa cycle handler that doesn't engage when the annotation is present?

---

_Comment by @carljm on 2025-06-23 21:34_

Looking at the code it makes sense to me why there's a cycle. What isn't clear to me is why the cycle doesn't converge.

I presume the explicit `Self` annotation is required to repro, on main, because we haven't yet landed the implicit-Self PR, so without that annotation on main `self` would just be `Unknown`, so `self.secure` is just `Unknown` without triggering any cycle.

---

_Comment by @sharkdp on 2025-06-24 10:41_

> What isn't clear to me is why the cycle doesn't converge.

Hah, this is pretty interesting. It doesn't converge, and it doesn't diverge. Instead, it alternates indefinitely between `Unknown | Literal[True]` and `Unbound`.

Here's an easier example that demonstrates the same issue:
```py
from typing import Literal

class Toggle:
    def __init__(self: "Toggle"):
        if not self.x:
            self.x: Literal[True] = True
```

If `self.x` is `Never` (initial cycle) or unbound, the `if` condition is inferred as `bool`, and consequently, `self.x` is inferred as (a possibly-unbound) `Literal[True]`. But in the next iteration, if `self.x` is `True`, then we infer that branch as unreachable, and `self.x` is unbound!

---

_Comment by @sharkdp on 2025-06-24 11:42_

I considered a few solutions here:
* Never to treat an attribute as definitely-unbound if we see a `self.x = …` binding, even in an unreachable branch. This doesn't help here, unless we would treat that branch as reachable. Otherwise, we infer `Unknown` for the attribute, and then it alternates between `Unknown` and `Literal[True]`.
* To treat the truthiness of `Never` as `AlwaysTrue` instead of `Ambiguous`. This does not help and only changes with which type we enter this cycle.
* To special-case a value of "unbound" in `member_lookup_cycle_recover`, by trying to return `CycleRecoveryAction::Fallback(unknown)`. But that fallback doesn't converge either.

It makes me think again of some kind of `CycleRecoveryAction::FallbackImmediate` that wouldn't check for convergence. But I know that this has implications for determinism.

Another thing that came to my mind would be some kind of detection of this alternate pattern, that would allow us to fallback to some kind of "union" of the two alternating values. Unfortunately, there is no such union here that would make this cycle converge, at least in this `Toggle` example with the explicit `Literal[True]` annotation.

---

_Comment by @sharkdp on 2025-06-24 11:51_

Would it make sense to only evaluate a reachability constraint to a definite (non-ambiguous) value if all symbols are definitely bound? This could potentially solve this, but I'm not sure what the impact would be. It also doesn't seem correct in all cases?
```py
def _(flag: bool):
    if flag:
        x = True

    if x:
        # we treat this as definitely reachable, but if `x` is unbound,
        # the runtime would thrown a `NameError` and we would not enter
        # this branch
        pass
    
    if not x:
        # this branch, however, *is* definitely unreachable. If `x` is
        # bound, we don't enter it, and if `x` is unbound, we also don't
        # enter it due to a `NameError`.
        pass
```

---

_Comment by @carljm on 2025-06-26 16:33_

> Would it make sense to only evaluate a reachability constraint to a definite (non-ambiguous) value if all symbols are definitely bound?

This seems the most promising to me as a general solution to this category of problem.

I don't think it can ever be described as incorrect to conclude that a branch is maybe-reachable, it's just a question of precision. No other type checker attempts to detect this unreachability at all!

---

_Comment by @sharkdp on 2025-06-27 07:43_

Together with Carl, we considered a few solutions to this problem.

One option might be to generally disallow certain patterns (such as attribute accesses on `self` and function calls) in reachability constraints. We could detect them during semantic index building and either not record a reachability constraint at all, or just record `AMBIGUOUS`. This might be a rather simple solution to the problem here, but it's possible that similar problems would arise in loops once we implement #232. And here, the pattern-based approach would fail.
```py
while True:
    if not x:
        x = True
```

Another option would be what was proposed above. To keep track of boundness in type inference, and then to incorporate that in reachability constraint evaluation (basically falling back to `AMBIGUOUS` whenever a non-definitely-bound symbol is encountered).

---

_Added to milestone `Beta` by @carljm on 2025-06-27 14:28_

---

_Comment by @Glyphack on 2025-07-23 15:43_

Is this issue fixed? I can't reproduce it in [playground](https://play.ty.dev/3f79ec67-b5b0-4dd1-b215-cc4cc1dd2de0) anymore.

Also out of curiosity I tried out [the first suggestion](https://github.com/astral-sh/ty/issues/692#issuecomment-3012051741) in my implicit self pr. It reduced the number of cycles in this [case](https://github.com/astral-sh/ty/issues/758).

But it caused a different kind of cycle.
For example one case is [this code](https://github.com/pallets/werkzeug/blob/504a8c4fbda9b8b2fd09e817544ffd228f23458e/src/werkzeug/serving.py#L171). The type of `self.client_address` explodes into
```
tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown | tuple[tuple[str, int] | Unknown, Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]], Literal[0]]
```
instead of just `tuple[str, int] | Unknown`.

Did I understand the suggestion here correctly? The code I implemented is [here](https://github.com/astral-sh/ruff/pull/18473/files#diff-c340eca6210b3af8607376e62a8db4729277e27c635ea0e40c86965e933839a3R1089). If yes then I try option 2.

---

_Comment by @carljm on 2025-07-23 15:53_

I don't think this issue is fixed. [This minimized example](https://github.com/astral-sh/ty/issues/692#issuecomment-2999760547) still fails. And the OP example [also still fails](https://play.ty.dev/557ae5e9-5c50-486a-bf5c-9a3831ea21f6) if we add an extra line querying the type of `A().secure`.

---

_Comment by @sharkdp on 2025-07-24 07:05_

> Did I understand the suggestion here correctly? The code I implemented is [here](https://github.com/astral-sh/ruff/pull/18473/files#diff-c340eca6210b3af8607376e62a8db4729277e27c635ea0e40c86965e933839a3R1089). If yes then I try option 2.

Yes, I think that's what we had in mind. Thank you for trying that approach.

---

_Assigned to @Glyphack by @carljm on 2025-08-15 15:56_

---

_Closed by @sharkdp on 2025-08-28 12:34_

---
