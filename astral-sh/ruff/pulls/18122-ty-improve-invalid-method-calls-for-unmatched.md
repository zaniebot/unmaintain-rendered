```yaml
number: 18122
title: "[ty] Improve invalid method calls for unmatched overloads"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/diag-method-overload
created_at: 2025-05-15T14:15:34Z
updated_at: 2025-05-15T15:46:36Z
url: https://github.com/astral-sh/ruff/pull/18122
synced_at: 2026-01-12T15:56:12Z
```

# [ty] Improve invalid method calls for unmatched overloads

---

_@BurntSushi_

This makes an easy tweak to allow our diagnostics for unmatched
overloads to apply to method calls. Previously, they only worked for
function calls.

There is at least one other case worth addressing too, namely, class
literals. e.g., `type()`. We had a diagnostic snapshot test case to
track it.

Closes astral-sh/ty#274


---

_Review requested from @carljm by @BurntSushi on 2025-05-15 14:15_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-05-15 14:15_

---

_Review requested from @sharkdp by @BurntSushi on 2025-05-15 14:15_

---

_Review requested from @dcreager by @BurntSushi on 2025-05-15 14:15_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-05-15 14:15_

---

_Review request for @dcreager removed by @BurntSushi on 2025-05-15 14:15_

---

_Review request for @carljm removed by @BurntSushi on 2025-05-15 14:15_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-05-15 14:15_

---

_Comment by @github-actions[bot] on 2025-05-15 14:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @BurntSushi on 2025-05-15 14:20_

(If we go ahead with this, I'll create a new issue to track further improvements here.)

---

_Comment by @MichaReiser on 2025-05-15 14:24_

@BurntSushi I sort of like to include the crate (or part) that gets changed in a PR but it does have the downside that the person writing the changelog has to editorialize your PR title. That's why I settled to use `[ty]` followed by what's changing.

---

_Renamed from "ty_python_semantic: improve invalid method calls for unmatched overloads" to "[ty] improve invalid method calls for unmatched overloads" by @BurntSushi on 2025-05-15 14:25_

---

_Comment by @BurntSushi on 2025-05-15 14:25_

Happy to make changelog editorializing easier!

---

_Renamed from "[ty] improve invalid method calls for unmatched overloads" to "[ty] Improve invalid method calls for unmatched overloads" by @BurntSushi on 2025-05-15 14:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/diagnostics/no_matching_overload.md`:306 on 2025-05-15 14:34_

nit: I'd sort-of prefer it if we used a custom class with an overloaded constructor method rather than using one from typeshed. Using one from typeshed has the downside that it makes updating our vendored typeshed harder, because the line numbers in the snapshots often change when typeshed changes. E.g. we couldn't just merge https://github.com/astral-sh/ruff/pull/18110 straight away -- Carl had to update a bunch of snapshots due to line numbers changing 

---

_@AlexWaygood approved on 2025-05-15 14:34_

LGTM! There's lots of cases this doesn't cover, but this will improve the user experience a lot, so it's definitely worth it

---

_Label `ty` added by @MichaReiser on 2025-05-15 14:37_

---

_Label `diagnostics` added by @MichaReiser on 2025-05-15 14:37_

---

_@dhruvmanila approved on 2025-05-15 14:54_

Thank you!

---

_@BurntSushi reviewed on 2025-05-15 15:27_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/diagnostics/no_matching_overload.md`:306 on 2025-05-15 15:27_

Can you help with this? I've tried a bunch of things and I can't seem to re-create effect of getting a `Type(ClassLiteral(...))` that you get with `type()` or even `str()`. One thing I tried:

```python
from typing import Any, overload

class Foo:
    @overload
    def __new__(self, x: str): ...
    @overload
    def __new__(self, x: int): ...
    def __new__(self, x: str | int): ...

foo = Foo(b"wat")
```

Gets:

```
error[no-matching-overload]: No overload of function `__new__` matches arguments
  --> main.py:12:7
   |
10 |     def __new__(self, x: str | int): ...
11 |
12 | foo = Foo(b"wat")
   |       ^^^^^^^^^^^
   |
info: First overload defined here
 --> main.py:7:9
  |
5 | class Foo:
6 |     @overload
7 |     def __new__(self, x: str): ...
  |         ^^^^^^^^^^^^^^^^^^^^^
8 |     @overload
9 |     def __new__(self, x: int): ...
  |
info: Possible overloads for function `__new__`:
info:   (self, x: str) -> Unknown
info:   (self, x: int) -> Unknown
info: Overload implementation defined here
  --> main.py:10:9
   |
 8 |     @overload
 9 |     def __new__(self, x: int): ...
10 |     def __new__(self, x: str | int): ...
   |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^
11 |
12 | foo = Foo(b"wat")
   |
info: rule `no-matching-overload` is enabled by default

Found 1 diagnostic
```

I also tried pulling from parts of [`str`](https://github.com/python/typeshed/blob/337fd828e819988af2d3600283d8068bbbab7f50/stdlib/builtins.pyi#L445) and [`type`](https://github.com/python/typeshed/blob/337fd828e819988af2d3600283d8068bbbab7f50/stdlib/builtins.pyi#L174) from typeshed, but no such luck.

Moreover, it seems that _all_ instances of `no-matching-overload` for a `ClassLiteral` in mdtest as it stands today are using either `type` or `str` builtins.

---

_@AlexWaygood reviewed on 2025-05-15 15:31_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/diagnostics/no_matching_overload.md`:306 on 2025-05-15 15:31_

Ah! I totally forgot that we currently heavily special-case `str` and `type` currently. In that case, it's fine to just keep what you have for now -- sorry!!

We assign these overloads manually to the class for `str` and `type` -- I can't remember exactly why, but it probably makes up for some missing features elsewhere: https://github.com/astral-sh/ruff/blob/c066bf0127482fc18c1337b50502cc63bdf805b5/crates/ty_python_semantic/src/types.rs#L3865-L3908

---

_@BurntSushi reviewed on 2025-05-15 15:38_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/diagnostics/no_matching_overload.md`:306 on 2025-05-15 15:38_

Ah okay phew. I thought I was losing my mind. :-)

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/diagnostics/no_matching_overload.md`:306 on 2025-05-15 15:38_

(My next step was to trace through ty and figure out where and how `type()` was getting typed as `Type(ClassLiteral(..))`. So I guess I probably would have eventually found that. Thank you for pointing it out!)

---

_@BurntSushi reviewed on 2025-05-15 15:38_

---

_Merged by @BurntSushi on 2025-05-15 15:39_

---

_Closed by @BurntSushi on 2025-05-15 15:39_

---

_Branch deleted on 2025-05-15 15:39_

---

_@AlexWaygood reviewed on 2025-05-15 15:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/diagnostics/no_matching_overload.md`:306 on 2025-05-15 15:39_

> Ah okay phew. I thought I was losing my mind. :-)

Sorry again!! Another argument for keeping special-casing to a minimum: preserving @BurntSushi's sanity 😄

---

_@BurntSushi reviewed on 2025-05-15 15:46_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/diagnostics/no_matching_overload.md`:306 on 2025-05-15 15:46_

Yes sanity is important! lol

---
