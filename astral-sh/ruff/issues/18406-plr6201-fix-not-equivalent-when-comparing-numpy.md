```yaml
number: 18406
title: "`PLR6201` fix not equivalent when comparing `numpy.dtype` with numpy scalar types"
type: issue
state: open
author: jorenham
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-05-31T22:15:12Z
updated_at: 2025-06-02T12:59:45Z
url: https://github.com/astral-sh/ruff/issues/18406
synced_at: 2026-01-12T15:54:56Z
```

# `PLR6201` fix not equivalent when comparing `numpy.dtype` with numpy scalar types

---

_@jorenham_

### Summary

For example:

```py
import numpy as np

dtype = np.dtype(np.float64)
_ = dtype == np.float64 or dtype == np.float32
```

reports

```
Consider merging multiple comparisons: `dtype in (np.float64, np.float128)`. Use a `set` if the elements are hashable. (PLR1714) [Ln 4, Col 5]
```

[ruff play](https://play.ruff.rs/a5f7a81e-563d-4edf-9e67-ee871178addf)

But doing so would lead to different outcomes, because

```pycon
>>> import numpy as np
>>> np.dtype(np.float64) == np.float64
True
>>> np.dtype(np.float64) in {np.float64}
False
```

Personally, I'm not a big fan of this numpy behavior. But it's not something that is easy to change, unfortunately. So perhaps an exception could be made for this in ruff?

See https://github.com/numpy/numpy/issues/17864 for a discussion of this behavior





---

_Comment by @rkern on 2025-06-01 17:28_

`PLR1714` seems to be working fine in this case. The quick-fix'll make it the `tuple` version, not the `set`, which works fine.

It's the subsequent `PLR6201` that converts it to a set that's dodgy. But `PLR6201` makes the `set` recommendation inappropriately in other places, too, and I'm not sure that's 100% fixable. Other than certain simple cases with builtin types, it can't really know whether or not an object is going to be appropriate for a `set` (and even within builtin types, there are corner cases like `([],)`).

---

_Renamed from "`PLR1714` fix not equivalent when comparing `numpy.dtype` with numpy scalar types" to "`PLR6201` fix not equivalent when comparing `numpy.dtype` with numpy scalar types" by @jorenham on 2025-06-01 18:10_

---

_Comment by @jorenham on 2025-06-01 18:15_

> Other than certain simple cases with builtin types, it can't really know whether or not an object is going to be appropriate for a `set` (and even within builtin types, there are corner cases like `([],)`).

Perhaps at some point `ty`'s inference could be used to determine whether the type of LHS matches that of the RHS values? 

Otherwise, maybe `PLR6201` could have a configurable ignore-list of types, that by default contains `numpy.dtype` and scalar types like `numpy.float64` (subtypes of `numpy.generic`)?

---

_Comment by @rkern on 2025-06-01 20:36_

Personally, I don't think it's worth chasing any farther. It'll likely never be a 100% safe fix to apply blind (and even when safe, quite likely not desirable; a tuple is a great option for 2 items), but it's just worded as a "Consider ...", which is perfectly fine. It's good for it to highlight opportunities, and trying to be 100% safe would mean less real opportunities get spotted.

---

_Comment by @jorenham on 2025-06-02 01:22_

I understand that a 100% safe fix is not realistic, and it's not what I was trying to ask. My motivation for opening this issue, is that I have seen this go wrong in a very specific situation. So that's when a numpy dtype instances are compared with "dtype-likes" with different hash values, which I explained in description of this issue. Here's an example of this going wrong in the wild: https://github.com/numpy/numpy/pull/28755#discussion_r2049543299. 

It's also worth pointing out that the different outcome that's caused by this fix, is very easy to miss. I'm guessing that the majority of numpy users wouldn't know about this `__eq__` and `__hash__` incompatibility. And if you later discover a new bug in your codebase, I can imagine that debugging it could be pretty difficult.

And as far as I'm concerned, I think it would be a lot better to fix it on our side. But because that would be a backwards-incompatible change, we would have to deprecate it first. In the most optimistic scenario, it would take two years until we can actually fix this.

So given all of those beautiful numpy-specific rules that are already in `ruff`, I was hoping that you might be willing to special-case this specific scenario :)

---

_Comment by @ntBre on 2025-06-02 12:59_

Thanks for the report and for this discussion, it was helpful for me to read!

I wouldn't be opposed to adding a special case or at least a configuration option here, but I'm worried it might be a bit difficult to implement a reliable type check, so it may require ty's type inference, as you said.

The fix for PLR6201 should already be marked as unsafe, and it's also a preview rule, so this is useful feedback that might prevent its stabiliization.

---

_Label `rule` added by @ntBre on 2025-06-02 12:59_

---

_Label `type-inference` added by @ntBre on 2025-06-02 12:59_

---
