```yaml
number: 676
title: "`match` over `Never`"
type: issue
state: open
author: pfaion-soundhound
labels:
  - question
  - type-inference
assignees: []
created_at: 2025-06-18T12:07:08Z
updated_at: 2025-11-18T16:10:33Z
url: https://github.com/astral-sh/ty/issues/676
synced_at: 2026-01-12T15:54:23Z
```

# `match` over `Never`

---

_@pfaion-soundhound_

### Summary

Hey everyone,

I was reading some of the issues around dealing with `Never` and decided to play around with it a bit myself on the playground to get an understanding of how `Never` is actually working.

I came across the following example ([playground link](https://play.ty.dev/8892b6bf-232e-4a5c-a62a-88e69fa677fe)):
```python
from typing import Never

def f(v: Never):
    match v:
        case str():
            x = input()
        case _:
            x = int(input())
    reveal_type(x)     # Revealed type: `str` 
```

I'm not even sure what's supposed to happen here, but I feel somewhat confident that the current behavior is wrong? Apologies if it isn't, curious to learn what the intended behavior is here!

I didn't find any other issue with `match` and `Never` in the title, I hope this is not yet already known!

Very excited about `ty`, thanks everyone for working on this! 

### Version

_No response_

---

_Renamed from "`match` over Never" to "`match` over `Never`" by @pfaion-soundhound on 2025-06-18 12:07_

---

_Comment by @sharkdp on 2025-06-18 12:30_

Thank you for reporting this.

A function which accepts an argument of type `Never` can not be called. So the function body here is unreachable. No matter which we would infer here, it could not be observed.

The type inference result is somewhat defensible, I think? `Never` is assignable to everything, so in that sense, the first pattern "matches", which leads to a result of `str`.

---

_Label `question` added by @sharkdp on 2025-06-18 12:30_

---

_Label `type-inference` added by @sharkdp on 2025-06-18 12:30_

---

_Comment by @pfaion-soundhound on 2025-06-18 12:35_

I think I would have expected that none of the cases matches against `Never` and that `x` would also be `Never`. Or like "undefined"? Not sure how `Never` relates to undefined...

---

_Comment by @jelle-openai on 2025-06-18 14:27_

For comparison, on this sample:

- mypy emits `Cannot determine type of "x"` and reveals Any
- pyright treats the reveal_type as unreachable and doesn't emit anything
- pyrefly reveals `int | str`

I probably like mypy's behavior best but it doesn't feel important to enforce a particular behavior.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 15:12_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
