```yaml
number: 1644
title: "Improve Liskov diagnostics to say *why* a method override is incompatible"
type: issue
state: open
author: tronical
labels:
  - diagnostics
assignees: []
created_at: 2025-11-26T09:13:50Z
updated_at: 2025-12-26T17:22:52Z
url: https://github.com/astral-sh/ty/issues/1644
synced_at: 2026-01-10T01:56:40Z
```

# Improve Liskov diagnostics to say *why* a method override is incompatible

---

_Issue opened by @tronical on 2025-11-26 09:13_

### Summary

`uvx ty@0.0.1a28 check` yields:

```
error[invalid-method-override]: Invalid override of method `set_row_data`
  --> slint/models.py:90:9
   |
88 |         return self.list[row]
89 |
90 |     def set_row_data(self, row: int, data: T) -> None:
   |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Definition is incompatible with `Model.set_row_data`
91 |         self.list[row] = data
92 |         super().notify_row_changed(row)
   |
  ::: slint/models.py:36:9
   |
34 |         return ModelIterator(self)
35 |
36 |     def set_row_data(self, row: int, value: T) -> None:
   |         ---------------------------------------------- `Model.set_row_data` defined here
37 |         """Call this method on mutable models to change the data for the given row.
38 |         The UI will also call this method when modifying a model's data.
   |
info: This violates the Liskov Substitution Principle
info: rule `invalid-method-override` is enabled by default

Found 1 diagnostic
```

The signatures are identical, so I'm not sure what's wrong here :)

For reference (with code removed):

```python
class Model[T](native.PyModelBase, Iterable[T]):
   
    def set_row_data(self, row: int, value: T) -> None:
   
class ListModel[T](Model[T]):
  
    def set_row_data(self, row: int, data: T) -> None:
        ...
```

### Version

0.0.1a28

---

_Comment by @AlexWaygood on 2025-11-26 09:22_

Thanks for the report! The issue here is that the third parameter has a different name in the method on your subclass than it does in the method on your superclass (`data` on the subclass, `value` on the superclass). That means that you could potentially pass a `value=...` keyword argument to the method if called on an instance of the superclass, but the exact same set of arguments would fail if called on the subclass method. That violates the Liskov Substitution Principle.

Apologies that our diagnostic isn't great here right now. We'd really love to tell you in the diagnostic _why_ the signature of the subclass method is incompatible with the signature of the superclass method, but this is unfortunately quite a significant task that we don't expect to start until after our beta release is out

---

_Label `question` added by @MichaReiser on 2025-11-26 09:23_

---

_Comment by @tronical on 2025-11-26 10:31_

Oh that's awesome. I can't believe I overlooked that, but I understand that this may be difficult to fix on your side. Now I can fix the signature on our side and use the current release. Thanks a lot for the quick response and help!

(please feel free to close this issue)

---

_Comment by @nyssance on 2025-11-26 14:35_

> Thanks for the report! The issue here is that the third parameter has a different name in the method on your subclass than it does in the method on your superclass (`data` on the subclass, `value` on the superclass). That means that you could potentially pass a `value=...` keyword argument to the method if called on an instance of the superclass, but the exact same set of arguments would fail if called on the subclass method. That violates the Liskov Substitution Principle.
> 
> Apologies that our diagnostic isn't great here right now. We'd really love to tell you in the diagnostic _why_ the signature of the subclass method is incompatible with the signature of the superclass method, but this is unfortunately quite a significant task that we don't expect to start until after our beta release is out

However, it conflicts with ruff's ARG002. If a parameter is unused, it will add an underscore prefix or directly use _, for example in a subclass method inheriting from a parent class.

---

_Comment by @AlexWaygood on 2025-11-26 14:48_

> However, it conflicts with ruff's ARG002. If a parameter is unused, it will add an underscore prefix or directly use _, for example in a subclass method inheriting from a parent class.

@nyssance you can suppress this Ruff error by explicitly decorating the subclass method with `@override`: https://play.ruff.rs/694ced62-acc1-4a4a-b5d0-df9389637bd4. Ruff then knows that it should not check the method for ARG002 violations, because it knows that (1) the method is intended to override a method from a parent class, (2) that type checkers may complain if the subclass method has different parameter names to the superclass method, and (3) that the type checker will enforce that `@override` is doing correctly (see https://docs.astral.sh/ty/reference/rules/#invalid-explicit-override for the relevant ty rule).

However, it doesn't look like the Ruff docs at https://docs.astral.sh/ruff/rules/unused-method-argument/#unused-method-argument-arg002 currently mention that it will ignore methods decorated with `@override`. We should fix that.

---

_Comment by @AlexWaygood on 2025-11-26 17:38_

> (please feel free to close this issue)

Instead, I'm going to repurpose the issue, since we don't currently have an issue open to track improving the quality of our Liskov diagnostics, and we need one :-)

---

_Renamed from "0.0.1a28 yields error about invalid method override" to "Improve Liskov diagnostics to say *why* a method override is incompatible" by @AlexWaygood on 2025-11-26 17:39_

---

_Label `question` removed by @AlexWaygood on 2025-11-26 17:39_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-26 17:39_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-11-26 17:39_

---

_Comment by @pjonsson on 2025-12-25 13:12_

The code that created this issue would have been easier to diagnose if the highlight pointed to the incompatible part of the definition, like this:
```
90 |     def set_row_data(self, row: int, data: T) -> None:
   |                                      ^^^^^^^          Definition is incompatible with `Model.set_row_data`
91 |         self.list[row] = data
92 |         super().notify_row_changed(row)
   |
  ::: slint/models.py:36:9
   |
34 |         return ModelIterator(self)
35 |
36 |     def set_row_data(self, row: int, value: T) -> None:
   |                                      --------          `Model.set_row_data` defined here
```
I know that it's difficult to pinpoint the root cause in constraint-based systems, so consider whether shrinking the highlighted parts of the signature would be easy to implement and could work as a stop-gap solution.

---

_Comment by @carljm on 2025-12-26 17:22_

Just want to note that the implementation here should generalize to providing these details for any assignability check between callables, not just for Liskov override checks specifically. That is, if you try passing a callback to some function that takes one, and it's not assignable, you should get the same details explaining why as you would get for an invalid-override between those same two callables.

(This may just be another way of saying that this is blocked on #163 -- and it's already marked as such.)

---
