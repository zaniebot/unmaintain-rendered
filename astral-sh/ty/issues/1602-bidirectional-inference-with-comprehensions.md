```yaml
number: 1602
title: Bidirectional inference with comprehensions
type: issue
state: closed
author: gjcarneiro
labels:
  - bidirectional inference
assignees: []
created_at: 2025-11-20T17:07:10Z
updated_at: 2026-01-20T20:25:10Z
url: https://github.com/astral-sh/ty/issues/1602
synced_at: 2026-01-20T20:55:26Z
```

# Bidirectional inference with comprehensions

---

_@gjcarneiro_

### Summary

With Ty a27, a simple dict literal `{"foo": "abc"}` is interpreted as `dict[Unknown | str, Unknown | str]`, which causes downstream problems.

https://play.ty.dev/9134dfe2-2020-4bcd-8549-4d22e562a562

### Version

ty 0.0.1-alpha.27

---

_Comment by @carljm on 2025-11-20 17:13_

Thanks for the report! This is intentional behavior, though we're continuing to evaluate it. If you haven't annotated the dict, it seems unwarranted to assume that just because the initial contents were `str: str`, that those are supposed to be the only possible contents.

Can you say more about specifically what the "downstream problems" are?

---

_Comment by @gjcarneiro on 2025-11-20 17:38_

Maybe it's unrelated to the `Unknown`, it's just that `Unknown` confused me greatly.  Here's the scenario: https://play.ty.dev/36b11db1-359e-41b6-b90f-0fe5acd9e27c

It seems like the list comprehension generating a list of dicts doesn't work so well.  Maybe I wrongly assumed the `Unknown` was to blame.

```
discounts: list[CreateParamsDiscount] = [
        {"coupon": coupon_id} for coupon_id in apply_stripe_coupons
    ]
```


---

_Comment by @AlexWaygood on 2025-11-20 17:52_

cc. @sharkdp / @ibraheemdev -- it looks like we're possibly not propagating the type context fully into the list comprehension? (Was this one of the things you discussed at the on-site? I vaguely remember there being some talk about it being hard to propagate type context into a completely different scope, because it needs to travel from one `TypeInferenceBuilder` to another...?)

---

_Renamed from "`{"foo": "abc"}` type revealed as `dict[Unknown | str, Unknown | str]`" to "dictionary literal inside comprehension not inferred as TypedDict, even with annotation" by @carljm on 2025-11-20 17:58_

---

_Comment by @carljm on 2025-11-20 18:00_

Yeah, I think the `| Unknown` part is unrelated here; `dict[str, str]` would fail the assignment to `CreateParamsDiscount` too. The issue is we're not applying the type context from the annotation and inferring `CreateParamsDiscount` in the comprehension.

---

_Label `bidirectional inference` added by @carljm on 2025-11-20 18:00_

---

_Added to milestone `Stable` by @carljm on 2025-11-20 18:00_

---

_Comment by @carljm on 2025-11-20 18:01_

(I put this in stable as a P0 but I suspect it will come up a lot; if there's a reasonable fix available it would be great to get it into the beta)

---

_Label `typeddict` added by @AlexWaygood on 2025-11-20 18:07_

---

_Comment by @sharkdp on 2025-11-20 18:26_

> cc. @sharkdp / @ibraheemdev -- it looks like we're possibly not propagating the type context fully into the list comprehension? (Was this one of the things you discussed at the on-site? I vaguely remember there being some talk about it being hard to propagate type context into a completely different scope, because it needs to travel from one TypeInferenceBuilder to another...?)

Exactly. I was aware about this limitation in type inference for comprehensions. After we saw that the "outer" type context handled a lot of cases, @ibraheemdev and I decided that it was fine to postpone this (see https://github.com/astral-sh/ruff/pull/20962#discussion_r2482437556). I did add tests with a TODO [here](https://github.com/astral-sh/ruff/blob/c4767f5aa89df66815736376127a71ab49777fe9/crates/ty_python_semantic/resources/mdtest/comprehensions/basic.md?plain=1#L177-L191) that are very similar to the scenario described in this issue. But I failed to create a ticket for the follow-up, so thanks for opening this @gjcarneiro.

---

_Label `typeddict` removed by @sharkdp on 2025-11-20 18:26_

---

_Renamed from "dictionary literal inside comprehension not inferred as TypedDict, even with annotation" to "Bidirectional inference with comprehensions" by @ibraheemdev on 2025-11-27 02:07_

---

_Removed from milestone `Stable` by @carljm on 2026-01-08 18:46_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-08 18:46_

---

_Assigned to @ibraheemdev by @ibraheemdev on 2026-01-09 05:27_

---

_Closed by @ibraheemdev on 2026-01-20 20:25_

---
