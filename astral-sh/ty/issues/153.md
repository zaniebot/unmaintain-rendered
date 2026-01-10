```yaml
number: 153
title: "supporting `@warnings.deprecated` and its backport in `typing_extensions`"
type: issue
state: closed
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-03-26T18:10:00Z
updated_at: 2025-07-18T23:50:31Z
url: https://github.com/astral-sh/ty/issues/153
synced_at: 2026-01-10T02:07:35Z
```

# supporting `@warnings.deprecated` and its backport in `typing_extensions`

---

_Issue opened by @carljm on 2025-03-26 18:10_

_No description provided._

---

_Comment by @Avasam on 2025-04-11 06:09_

Do you mean `@warnings.deprecated`? 

---

_Comment by @MichaReiser on 2025-04-11 06:11_

I think so, reference https://typing.python.org/en/latest/spec/directives.html#deprecated

---

_Renamed from "[red-knot] supporting @typing.deprecated" to "[red-knot] supporting `@warnings.deprecated` and its backport in `typing_extensions`" by @AlexWaygood on 2025-04-11 12:45_

---

_Renamed from "[red-knot] supporting `@warnings.deprecated` and its backport in `typing_extensions`" to "supporting `@warnings.deprecated` and its backport in `typing_extensions`" by @MichaReiser on 2025-05-07 15:25_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:55_

---

_Comment by @MichaReiser on 2025-07-08 14:42_

What I think is roughly needed here is to:

1. Add a new lint that warns about uses of deprecated functions, classes, and overloads
2. Either store whether the callable or class is deprecated as part of its type or lazily compute it on `Definition`
3. For calls (constructor or function calls), check if the used callable is deprecated and if so, emit a diagnostic. The diagnostic should add the `Deprecated` flag. 

---

_Comment by @AlexWaygood on 2025-07-08 14:45_

(Doesn't contradict anything you said above, just adding to it)

Note that a diagnostic stemming from `@deprecated` can also be triggered by an implicit call to a dunder method. For example, here pyright emits a diagnostic on `~ True`, because `bool.__invert__` is decorated with `@deprecated`, and `~` implicitly calls the `__invert__` dunder: https://pyright-play.net/?pythonVersion=3.12&strict=true&enableExperimentalFeatures=true&code=B4AgvCB%2BAqBOCuBTIA

---

_Comment by @dhruvmanila on 2025-07-09 08:49_

> * For calls (constructor or function calls), check if the used callable is deprecated and if so, emit a diagnostic. The diagnostic should add the `Deprecated` flag.

We also need to capture the deprecation message that's part of the decorator which should be shown to the user.



---

_Comment by @erictraut on 2025-07-09 14:36_

Another thing to consider: The deprecated state (and associated message) need to "ride along with" a function signature or class as it is transformed by function or class decorators. For example:

```python
from typing import Callable
from warnings import deprecated

def add_docs[T: Callable](f: T) -> T:
    return f

@add_docs
@deprecated("Use my_new_func instead")
def my_func(a: int, b: int) -> int:
    return 1

my_func(0, 0)
```

<img width="355" height="439" alt="Image" src="https://github.com/user-attachments/assets/40bb9c8a-3493-400d-85d2-285019b6e061" />

---

_Comment by @AlexWaygood on 2025-07-09 15:05_

> Another thing to consider: The deprecated state (and associated message) need to "ride along with" a function signature or class as it is transformed by function or class decorators.

An elegant way of achieving this in our model might be to have a new `Type::Deprecated` variant. The type of the `my_func` symbol in your example above might be inferred as `Type::Intersection([Type::FunctionLiteral("my_func"), Type::Deprecated("Use my_new_func instead")])`. Then we could piggyback on our existing implementation of intersections.

One question about that strategy however would be what type to display for `my_func` in error messages. `<function 'my_func'> & Deprecated("Use my_new_func instead")` probably isn't a very user-friendly display to show in `reveal_type` calls and similar. But maybe `<function 'my_func' (deprecated)>` might work? I think it would be reasonably easy to add some special-casing to `Type::display()` for intersections with `Type::Deprecated` to make that work.

---

_Comment by @carljm on 2025-07-09 15:25_

I suspect we shouldn't use intersections and a new type variant for this. It would require us to answer all kinds of questions that make no sense for the new "deprecated" variant (since it's not really a type at all), and would I think require a lot of special casing to get not just the display right for such intersections, but also all other type operations (member, call, etc).

I'm not sure we need to do anything special here for the given example, besides keeping the deprecated information on the FunctionLiteral type. In the given generic example, we should just solve T to the original FunctionLiteral type, and no information should be lost.

There are other cases where a decorator transforms the signature of the decorated function and returns a `Callable` type, and isn't simply a pass-through generic, where we might need to do something special. I think it might make sense for a callable type to optionally preserve a link back to its original FunctionLiteral, when it results from a decorator application?

(One other case is BoundMethod, where it wraps a deprecated function.)

---

_Comment by @AlexWaygood on 2025-07-09 15:28_

> I suspect we shouldn't use intersections and a new type variant for this. It would require us to answer all kinds of questions that make no sense for the new "deprecated" variant (since it's not really a type at all), and would I think require a lot of special casing to get not just the display right for such intersections, but also all other type operations (member, call, etc).

that's true, and we already map other non-type metadata through various operations, such as type qualifiers and boundness. (Though it feels somewhat painful every time we have to do so...)

---

_Comment by @AlexWaygood on 2025-07-09 15:36_

> since it's not really a type at all

(Well, it _could_ be a type if you think about it the right way: just as `AlwaysTruthy` is "the set of all possible objects that are guaranteed to be _always_ truthy in a boolean context, `Deprecated("Use my_new_func instead")` would be the set of all possible objects that are guaranteed to be deprecated with that particular message. But I take your point that special-casing the display for all possible types that could be deprecated might be a tall order.)

---

_Comment by @InSyncWithFoo on 2025-07-10 04:18_

Personally, I think of `@deprecated` as a kind of annotation. In other words, `@deprecated('Message')` `def f(): ...` is just syntactic sugar for what would otherwise look like `f: Annotated[TypeOf[f], Deprecated('Message')]`.

I'm not sure how hard it would be to implement things this way, but I think it makes more intuitive sense than making `Deprecated` a type variant.

---

_Closed by @Gankra on 2025-07-18 23:50_

---
