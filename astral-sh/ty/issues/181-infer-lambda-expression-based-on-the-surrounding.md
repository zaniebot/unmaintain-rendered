```yaml
number: 181
title: "Infer `lambda` expression based on the surrounding context"
type: issue
state: open
author: dhruvmanila
labels:
  - bidirectional inference
assignees: []
created_at: 2025-03-13T03:31:46Z
updated_at: 2025-12-27T19:11:27Z
url: https://github.com/astral-sh/ty/issues/181
synced_at: 2026-01-10T01:56:40Z
```

# Infer `lambda` expression based on the surrounding context

---

_Issue opened by @dhruvmanila on 2025-03-13 03:31_

Currently, the inference of `lambda` expression is done in isolation which isn't very useful because parameters in a `lambda` expression cannot be annotated. For example,

```py
reveal_type(lambda x: x)  # revealed: (x) -> Unknown
```

Here, we infer the type of `x` parameter as `Unknown` and we also infer the return type as `Unknown` by default for now. But, consider the following example:

```py
def foo(c: Callable[[int], str]):
    return

foo(lambda x: x)
```

In this case, as the `lambda` expression is used in the context of a `Callable` annotation, we can infer the argument type as `int` and thus the return type would be `int` as well but that is not assignable to the return type `str` as mentioned in the `Callable` annotation which should then raise an error.

Another example would be:
```py
f = lambda x: x
reveal_type(f(1))
```

Here, as the lambda expression is being called with a `Literal[1]` parameter, the return type would be inferred as `Literal[1]` as well.

---

_Comment by @sharkdp on 2025-03-13 07:32_

This is interesting. Inlining is one way to solve this problem, but it's probably limited? Both mypy and pyright see the problem in
```py
foo(lambda x: x)
```
but they do not see a problem with
```py
c = lambda x: x
foo(c)
```
They probably don't inline variables, and then just use the gradual `Unknown -> Unknown` type here.

In a language with full type inference, you would infer a generic `forall T. T -> T` type for `c` and then recognize the problem at the `foo` call site. Obviously, this would only work in very limited cases in Python. Inferring a generic type for something like `lambda x: x + 1` might still be possible with (custom) protocols but in general, it's not a feasible approach.

I still wonder if it would be possible to use an approach like Rust or C++ where you would infer some opaque lambda/callable type for `c`, and only try to match that against `int -> str` later. That opaque type could potentially store the reference to the lambda expression internally and then still perform the match-by-inlining operation when used through a binding like this?

---

_Comment by @AlexWaygood on 2025-03-14 18:04_

I don't believe mypy inlines lambdas at all, FWIW; I think mypy detects the error here because it makes heavy use of "type context" (also called bidirectional type inference) when inferring types. Here it sees that the code needs the lambda to have a type of `Callable[[int], str]` (or a subtype thereof) in order for it to type-check, so it tries its best to use `Callable[[int], str]` as its inferred type of the lambda -- but realises it can't, and so emits an error

---

_Renamed from "[red-knot] Support inlining of `lambda` expressions" to "[red-knot] Infer `lambda` expression based on the surrounding context" by @dhruvmanila on 2025-03-18 11:23_

---

_Comment by @dhruvmanila on 2025-03-18 11:24_

I've update the issue to reflect it as more around inferring it based on the context and not using a specific implementation.

---

_Renamed from "[red-knot] Infer `lambda` expression based on the surrounding context" to "Infer `lambda` expression based on the surrounding context" by @MichaReiser on 2025-05-07 15:26_

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-05-11 10:52_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:49_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 14:47_

---

_Added to milestone `GA` by @carljm on 2025-08-15 14:47_

---

_Comment by @MatthewMckee4 on 2025-12-27 13:28_

Would we ever consider special casing an example like `lambda x: x` and inferring `(T) -> T` for this?

---

_Comment by @carljm on 2025-12-27 19:11_

> Would we ever consider special casing an example like `lambda x: x` and inferring `(T) -> T` for this?

Yes, that's definitely one strategy that can be used here, although I think it has more limited applicability than other strategies (type context, "inlining").

---
