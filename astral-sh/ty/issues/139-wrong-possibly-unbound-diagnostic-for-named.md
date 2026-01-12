```yaml
number: 139
title: Wrong possibly-unbound diagnostic for named expression in Boolean chain
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - control flow
  - runtime semantics
assignees: []
created_at: 2025-04-03T08:21:14Z
updated_at: 2025-06-17T08:12:20Z
url: https://github.com/astral-sh/ty/issues/139
synced_at: 2026-01-12T15:54:22Z
```

# Wrong possibly-unbound diagnostic for named expression in Boolean chain

---

_@sharkdp_

This seems like a control flow / statically-known-branches bug?
```py
def _(flag: bool):
    if flag and (x := 1):
        x  # possibly-unresolved-reference
```
(https://play.ty.dev/cc0c8c06-d78f-4b7c-9b3b-d81fdbcea0e2)

---

_Label `bug` added by @sharkdp on 2025-04-03 08:21_

---

_Renamed from "[red-knot] Wrong possibly-unbound diagnostic in named expression" to "[red-knot] Wrong possibly-unbound diagnostic for named expression in Boolean chain" by @sharkdp on 2025-04-03 08:22_

---

_Comment by @AlexWaygood on 2025-04-03 11:38_

Added it to the "GA" milestone as it's important we fix this, but I don't think walruses are common enough to warrant prioritising it. LMK if you disagree!

---

_Label `bug` added by @MichaReiser on 2025-05-07 15:20_

---

_Renamed from "[red-knot] Wrong possibly-unbound diagnostic for named expression in Boolean chain" to "Wrong possibly-unbound diagnostic for named expression in Boolean chain" by @MichaReiser on 2025-05-07 15:25_

---

_Comment by @nickdrozd on 2025-05-08 02:47_

I don't know about others, but I use this pattern all the time. Therefore, currently ty blasts me with false positives.

Here is an example from some code that does algebra stuff:

```python
    def __floordiv__(self, other: Count) -> Count:
        if other == 1:
            return self

        assert isinstance(other, int)

        if (((lgcd := gcd(other, l := self.l)) == 1
                or (rgcd := gcd(other, r := self.r))) == 1
                or (div := gcd(lgcd, rgcd)) == 1):
            return make_div(self, other)

        return ((l // div) + (r // div)) // (other // div)
```

Admittedly this snippet goes a little overboard with assignment expressions. Still, it works fine. ty flags five variables as possibly unresolved, all false positives.

---

_Comment by @gusutabopb on 2025-05-09 07:47_

I hit this issue when trying out `ty` on an existing code base.


Sample code:

```python
import random

if (random.random() > 0.5) and (m := random.random()):
    # m is always defined, but ty warns it may not be (incorrect)
    print(m)
```
ty output

```
warning: lint:possibly-unresolved-reference: Name `m` used when possibly not defined
 --> r_and_m.py:5:11
  |
3 | if (random.random() > 0.5) and (m := random.random()):
4 |     # m is always defined. ty warns it may not be (incorrect)
5 |     print(m)
  |           ^
  |
info: `lint:possibly-unresolved-reference` is enabled by default
```

Flipping the order of the operators "fixes" the issue though:

```python
import random

if (m := random.random()) and (random.random() > 0.5):
    # m is always defined. ty is silent (correct)
    print(m)
```

ty output

```
All checks passed!
```


---

_Label `control flow` added by @AlexWaygood on 2025-05-11 07:32_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:57_

---

_Comment by @danielhollas on 2025-06-07 01:31_

I've tested all the snippets here (both on the playground and latest alpha.8 version) and none of them report any diagnostics. 🎉 

---

_Comment by @carljm on 2025-06-07 01:36_

I think this bug still exists. It was fixed in https://github.com/astral-sh/ruff/pull/18010, but that caused a performance regression and had to be rolled back, and I don't think we've investigated the root cause of the regression yet in order to try to re-land the fix. The reason there are no longer any diagnostics is that we decided to disable the `possibly-unbound-reference` diagnostic by default, since it will inevitably have some false positives.

---

_Comment by @danielhollas on 2025-06-07 01:44_

>  The reason there are no longer any diagnostics is that we decided to disable the possibly-unbound-reference diagnostic by default, since it will inevitably have some false positives.

Ah, that's what I was missing, thanks!

---

_Comment by @sharkdp on 2025-06-17 08:12_

Closing this in favor of https://github.com/astral-sh/ty/issues/626, which has a bit more details on `possibly-unresolved-reference`.

---

_Closed by @sharkdp on 2025-06-17 08:12_

---
