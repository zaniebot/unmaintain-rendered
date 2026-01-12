```yaml
number: 1176
title: Equality tests could narrow some types to Literals
type: issue
state: closed
author: MeGaGiGaGon
labels: []
assignees: []
created_at: 2025-09-13T00:41:18Z
updated_at: 2025-09-13T05:46:48Z
url: https://github.com/astral-sh/ty/issues/1176
synced_at: 2026-01-12T15:54:24Z
```

# Equality tests could narrow some types to Literals

---

_@MeGaGiGaGon_

### Summary

Sorry if this has already been reported/is being worked on, I could not find anything on it.

```py
x = int(input())
if x == 0:
    reveal_type(x)
```

In the [pyright playground](https://pyright-play.net/?code=B4AgvCCWB2AuAUMAOBXBBKdBYAUJAZiKGBAAwBcuI1IATgKYBu9AhgDYD6sAnkvfMHRA) the revealed type is `Literal[0]`.

In the [ty playground](https://play.ty.dev/ad4e7ba9-40ac-47b4-86a0-8b7892b36cee) the revealed type is `int`.

This would be a nice pattern to support, especially for exhaustedness checking/dead code analysis.

It's also odd to me that this pattern isn't supported, when the case of `!=` is:

```py
x = int(input())
if x != 0:
    reveal_type(x)  # Revealed type: `int & ~Literal[0]`
```

### Version

playground (174555480)

---

_Comment by @carljm on 2025-09-13 01:04_

We intentionally don't do this narrowing because it's unsound. The type `int` includes subtypes of `int`, which can override `__eq__` to do anything they like. 

The `!=` version is supported because it is sound. We know for sure that a literal zero cannot ever return true for `== 0`. 

---

_Comment by @MeGaGiGaGon on 2025-09-13 01:40_

I see, thank you for the explanation. So there is almost no situation where a narrowing with `==` or `in` is sound :(. Sorry for the churn

---

_Closed by @MeGaGiGaGon on 2025-09-13 01:40_

---

_Comment by @carljm on 2025-09-13 05:46_

There are cases where we can narrow from equality checks, eg we can narrow `Literal[0, 1, 2] | str` to `Literal[1] | str` after `== 1`. We can eliminate literal types that we know can't be equal. But in general we can't narrow non-final types from equality checks. 

---
