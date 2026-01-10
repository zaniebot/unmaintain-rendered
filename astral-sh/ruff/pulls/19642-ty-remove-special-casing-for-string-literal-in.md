```yaml
number: 19642
title: "[ty] Remove special casing for string-literal-in-tuple `__contains__`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/tuple-contains
created_at: 2025-07-30T16:47:50Z
updated_at: 2025-07-31T11:46:47Z
url: https://github.com/astral-sh/ruff/pull/19642
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Remove special casing for string-literal-in-tuple `__contains__`

---

_Pull request opened by @AlexWaygood on 2025-07-30 16:47_

## Summary

This PR rips out the special casing for `<string literal> in <tuple>` that was added in https://github.com/astral-sh/ruff/pull/18251.

We were unsure at the time whether it was worth it to add this special casing, since it added complexity to our type inference machinery but didn't appear to remove any false positives or false negatives when checking user code (the mypy_primer report on that PR is an empty diff). At the time, we thought that the benefits outweighed the costs, however, since the added complexity wasn't _very_ large, and it allowed us to test our new `ide_support::all_members` API in an elegant way in mdtests.

I think we should reconsider this, however. It's fundamentally inconsistent behaviour for us to infer the expression `"foo" in ("foo",)` as evaluating to `Literal[True]` if we do not also infer the expression `"foo" in SingleElementTupleSubclass(("foo",))` as evaluating to `Literal[True]` (where `SingleElementTupleSubclass` is considered by ty to be a subtype of `tuple[Literal["foo"]]`). We therefore have three options:
1. Convert the special casing to use synthesized `__contains__` overloads, similar to what was done for `__getitem__` in https://github.com/astral-sh/ruff/pull/19493
2. Adapt the existing special casing in `TypeInferenceBuilder::infer_binary_type_comparison` so that it also applies to tuple subclasses
3. Remove the special casing

(1) and (2) are both viable paths here, but they would both add significant complexity to the implementation of the special case, and I'm not sure it's worth doing that without evidence of patterns in user code that this enables us to understand better. Additionally, if we went with option (2) we would have to forbid users from overriding `__contains__` on tuple subclasses in order for us to make tuple subclasses sound. If we went with option (1) we would not have to _forbid_ overrides of `__contains__` on user subclasses of `tuple`; but in practice, we'd make it very difficult for users to do so after our implementation of the Liskov Substitution Principle has landed. Either forbidding `__contains__` overrides outright, or de-facto forbidding them by requiring the override to duplicate complicated overloads from the base class, feels overly pedantic to me. This PR therefore goes with option (3): ripping out the special casing.

The remaining question was, therefore: what to do about our `all_members.md` test? The solution this PR proposes is to adapt that test by adding a new `ty_extensions.has_member()` function that returns `Literal[True]` if our `ide_support_all_members` routine considers an object to have a particular member, and `Literal[False]` if not.

## Test Plan

Mdtests


---

_Label `ty` added by @AlexWaygood on 2025-07-30 16:47_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-30 16:47_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-30 16:47_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-30 16:47_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-07-30 16:47_

---

_Comment by @github-actions[bot] on 2025-07-30 16:50_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-30 16:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @sharkdp on 2025-07-31 07:34_

> It's fundamentally inconsistent behaviour for us to infer the expression `"foo" in ("foo",)` as evaluating to `Literal[True]` if we do not also infer the expression `"foo" in SingleElementTupleSubclass(("foo",))` as evaluating to `Literal[True]`

Not sure I'm totally following here. Why do you think that this is fundamentally inconsistent? *Inferring* something more precise for the supertype doesn't necessarily imply LSP incompatibility, right?

---

_Comment by @AlexWaygood on 2025-07-31 10:16_

We discussed this in person in our 1:1 today -- for completeness, here's an (admittedly slightly contrived) example where the current special casing would lead us to infer `Literal[True]` even though the result at runtime is `False`:

```py
from typing import Literal

class Bar(tuple[Literal["foo"]]):
    def __contains__(self, item) -> Literal[False]:
        return False

def is_foo_in_tuple(x: tuple[Literal["foo"]]) -> Literal[True]:
    return "foo" in x

reveal_type(is_foo_in_tuple(Bar(("foo",))))
```

We would continue to let this example pass type checking without diagnostics even after https://github.com/astral-sh/ty/issues/166 is implemented, because our current special casing does not synthesize a precise `__contains__` method for tuple types, it just applies the special casing directly in `infer_binary_type_comparison`.

Even putting aside soundness issues, it feels very strange to me that you'd get a more precise result here by upcasting a tuple subclass to its supertype (and a less precise result after narrowing the type) -- you would usually expect the opposite:

```py
from typing import Literal

class Bar(tuple[Literal["foo"]]): ...

x = ("foo",)

reveal_type("foo" in x)  # Literal[True]

if isinstance(x, Bar):
    reveal_type("foo" in x)  # now bool, even though the type of `x` is now more precise

def upcast(x: tuple[Literal["foo"]]) -> tuple[Literal["foo"]]:
    return x

def _(x: Bar):
    reveal_type("foo" in x)  # revealed: bool
    reveal_type("foo" in upcast(x))  # Literal[True], even though the `upcast()` function makes the type of `x` less precise!
```

---

_@sharkdp approved on 2025-07-31 10:26_

Thank you

---

_Merged by @AlexWaygood on 2025-07-31 10:28_

---

_Closed by @AlexWaygood on 2025-07-31 10:28_

---

_Branch deleted on 2025-07-31 10:28_

---
