```yaml
number: 1647
title: Incorrect type narrowing under the presence of complex Union type
type: issue
state: closed
author: mflova
labels: []
assignees: []
created_at: 2025-11-26T17:39:10Z
updated_at: 2025-11-26T19:26:37Z
url: https://github.com/astral-sh/ty/issues/1647
synced_at: 2026-01-10T01:58:59Z
```

# Incorrect type narrowing under the presence of complex Union type

---

_Issue opened by @mflova on 2025-11-26 17:39_

### Summary

I simplified as much as I could the given example. I guess that `ty` gets a bit lost under this `Union` type. Two errors are reported here:

```py
from collections.abc import Mapping, Sequence
from typing import TypeAlias

RecommendationT: TypeAlias = "str | Mapping[str, str]"

class CustomRecommendation: ...

class CustomError(Exception):
    def __init__(
        self,
        message: str,
        *,
        initial_recommendations: (RecommendationT | Sequence[RecommendationT] | CustomRecommendation | None) = None,
    ) -> None:
        super().__init__(message)
        self.recommendations: list[RecommendationT] = []
        if isinstance(initial_recommendations, Mapping):
            self.add_recommendation(initial_recommendations) #  error[invalid-argument-type] Argument to bound method `add_recommendation` is incorrect: Expected `str | Mapping[str, str] | CustomRecommendation`, found `(Sequence[str | Mapping[str, str]] & Top[Mapping[Unknown, object]]) | Mapping[str, str] | (CustomRecommendation & Top[Mapping[Unknown, object]])`
        elif isinstance(initial_recommendations, Sequence):
            for r in initial_recommendations or []:
                self.add_recommendation(r)  # error[invalid-argument-type] Argument to bound method `add_recommendation` is incorrect: Expected `str | Mapping[str, str] | CustomRecommendation`, found `object`

    def add_recommendation(self, recommendation: RecommendationT) -> None:
        ...
```

None of `mypy` nor `pyight` report any error. It also seems logical to me. The problem is also happening if I replace `Mapping` by `dict`. So it has nothing to do with `collections.abc` being used

### Version

ty 0.0.1-alpha.28 (8c342496a 2025-11-25)

---

_Comment by @carljm on 2025-11-26 19:09_

Thanks for the report!

Ty isn't getting lost in the union type; rather, it is telling you accurately that this code is not type-safe, in a way that mypy and pyright prefer to ignore.

I can create a subclass of `CustomRecommendation` which satisfies `Sequence` but is not a sequence of `RecommendationT`, it's a sequence of something else. Thus, after `elif isinstance(initial_recommendations, Sequence)` it is not actually safe to assume that iterating over `initial_recommendations` must yield `RecommendationT`.

If you want to express that this is not possible in your codebase, you could decorate `CustomRecommendation` with `@typing.final`, and then ty will understand that such a subclass is not possible, and the second error in this example will go away.

This alone isn't sufficient to fix the first error, because it is _also_ possible that some hypothetical type which satisfies `Sequence[RecommendationT]` _also_ satisfies `Mapping`, but not necessarily a `Mapping[str, str]`. So similarly, after `if isinstance(initial_recommendations, Mapping)` we can't rule out such a type, so we can't be sure that `initial_recommendations` is a `Mapping[str, str]`.

This can be solved by just reordering the if/elif, so that we've already ruled out `Sequence`.

With both of those fixes, this code passes checking in ty, and is in fact more type safe: https://play.ty.dev/5bb218e9-f627-4cd2-93e4-ee98e7a68e9b

---

_Comment by @carljm on 2025-11-26 19:17_

This is similar to the cases discussed in #1578, will close as duplicate of that. 

---

_Closed by @carljm on 2025-11-26 19:17_

---

_Comment by @mflova on 2025-11-26 19:17_

Thanks for the very nice and detailed explanation! It is appreciated.

I can understand and it is a smart treatment. However, what if CustomRecommendation is meant to be an abstract interface where any user can derive from to create custom recommendations? Isn't there a way to indicate `ty` that any subclasses that are derived from this abstract interface cannot have any Sequence-based type in any of its bases? Although `ty` treatment is the most correct implementation, it is also not so easy to deal with when we are working with cases like these (abstract interfaces)

As far as I am aware, I cannot think about any tool within the typing or typing-extensions ecosystem.

Thank you so much!

---

_Comment by @carljm on 2025-11-26 19:24_

@mflova Yes, I don't think there's currently a nice way to express such limitations on subclasses of `CustomRecommendation`. I can imagine possibly expressing this by defining `__getitem__` and/or `__iter__` using `Never` types, meaning that a subclass could never override them in a usable way? Currently ty would not understand this expression as making such a subclass impossible, though I suspect that could be added.

The simpler solution here is just to first rule out `CustomRecommendation` via checking `isinstance(initial_recommendations, CustomRecommendation)`, which also fixes the errors: https://play.ty.dev/031de150-e8a0-4c84-86b3-54d5b3d36520

---
