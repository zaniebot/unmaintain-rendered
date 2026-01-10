```yaml
number: 16292
title: "[red-knot] Short-circuit bool calls on bool"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/short-circuit-bool-of-bool
created_at: 2025-02-20T21:52:27Z
updated_at: 2025-02-20T23:09:12Z
url: https://github.com/astral-sh/ruff/pull/16292
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Short-circuit bool calls on bool

---

_Pull request opened by @sharkdp on 2025-02-20 21:52_

## Summary

This avoids looking up `__bool__` on class `bool` for every `Type::Instance(bool).bool()` call. 1% performance win on cold cache, 4% win on incremental performance.

---

_Label `red-knot` added by @sharkdp on 2025-02-20 21:52_

---

_Marked ready for review by @sharkdp on 2025-02-20 22:00_

---

_Review requested from @carljm by @sharkdp on 2025-02-20 22:00_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-20 22:00_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-20 22:00_

---

_@MichaReiser approved on 2025-02-20 22:02_

---

_Merged by @sharkdp on 2025-02-20 22:06_

---

_Closed by @sharkdp on 2025-02-20 22:06_

---

_Branch deleted on 2025-02-20 22:06_

---

_Comment by @carljm on 2025-02-20 22:08_

There are probably some other very common known classes we could match on here and short-circuit looking up `__bool__`? Like `int` and `str`...

---

_Comment by @sharkdp on 2025-02-20 22:12_

> There are probably some other very common known classes we could match on here and short-circuit looking up `__bool__`? Like `int` and `str`...

Yes. I spoke to @MichaReiser today and I really think we should make our benchmarks more diverse before we further spend so much time on performance optimization. I'm afraid we might be overfitting on tomllib.

I'll try to get some benchmarks on tomllib and maybe black tomorrow and see if adding int/str helps. Thanks for the suggestion.

---

_Comment by @sharkdp on 2025-02-20 22:19_

Another option would be to try and make `Type::bool` a salsa query? 

---

_Comment by @carljm on 2025-02-20 23:07_

> make our benchmarks more diverse before we further spend so much time on performance optimization. I'm afraid we might be overfitting on tomllib.

Yes, this is a great point and I agree! Feel free to drop this topic (or just file an issue for later).

> Another option would be to try and make `Type::bool` a salsa query?

Yes, the problem here is that there is almost certainly a long tail of types for which it does not pay to cache (because there isn't enough repeat usage), and a few very common types for which it does. Which is why culling some of the very common types (where we also already know the answer) seems like an easy win.

---
