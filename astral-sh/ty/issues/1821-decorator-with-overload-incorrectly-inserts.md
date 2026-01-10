```yaml
number: 1821
title: "decorator with overload incorrectly inserts `Unknown` into the decorated function's type"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - calls
assignees: []
created_at: 2025-12-09T07:07:07Z
updated_at: 2025-12-12T03:33:59Z
url: https://github.com/astral-sh/ty/issues/1821
synced_at: 2026-01-10T01:56:41Z
```

# decorator with overload incorrectly inserts `Unknown` into the decorated function's type

---

_Issue opened by @DetachHead on 2025-12-09 07:07_

### Summary

```py
from typing import overload, Callable

@overload
def asdf(fn: Callable[[], str]) -> str: ...


@overload
def asdf(fn: Callable[[], None]) -> None: ...


def asdf(fn: Callable[[], str | None]) -> str | None:
    return ""


@asdf
def foo() -> None: ...


reveal_type(foo)  # expected: `None`, actual: `None | Unknown`
```
https://play.ty.dev/eef6acf1-7d7c-4114-8ab9-c6e7c308e0e5

### Version

0.0.1-alpha.32

---

_Label `bug` added by @sharkdp on 2025-12-09 08:20_

---

_Label `calls` added by @sharkdp on 2025-12-09 08:20_

---

_Comment by @sharkdp on 2025-12-09 08:21_

Thank you for reporting this.

Interesting. Overload call resolution seems to work just fine if we explicitly call `asdf(foo)`, so apparently we call `asdf` differently somehow when applying the decorator.

---

_Comment by @DetachHead on 2025-12-09 08:52_

is it possible to make ty treat decorators the same way as a normal function call? it always annoyed me in mypy & pyright how there were so many subtle differences between decorator calls and regular function calls even though they do the exact same thing at runtime

---

_Comment by @sharkdp on 2025-12-09 09:07_

> is it possible to make ty treat decorators the same way as a normal function call?

That's how we implemented decorators, which is why I'm so confused by this bug 😄. Looking into it.

---

_Assigned to @sharkdp by @sharkdp on 2025-12-09 09:07_

---

_Comment by @MichaReiser on 2025-12-09 11:13_

> Interesting. Overload call resolution seems to work just fine if we explicitly call asdf(foo), so apparently we call asdf differently somehow when applying the decorator.

@dhruvmanila ran into a similar issue just recently and I think he narrowed it down to the cycle handling when inferring function literals https://github.com/astral-sh/ty/issues/1729#issuecomment-3612108797

---

_Comment by @dhruvmanila on 2025-12-09 15:29_

Yeah, there is a cycle similar to #1729 looking at the logs:
```
2025-12-09 20:59:06.597968 DEBUG calling arguments arguments=def foo() -> Divergent signature=Overload[(fn: () -> str) -> str, (fn: () -> None) -> None]
2025-12-09 20:59:06.598389 DEBUG after step 1 matching_overload_index=Multiple([0, 1]) signature=Overload[(fn: () -> str) -> str, (fn: () -> None) -> None]
2025-12-09 20:59:06.598472 DEBUG after step 2 matching_overload_index=Multiple([0, 1]) signature=Overload[(fn: () -> str) -> str, (fn: () -> None) -> None]
2025-12-09 20:59:06.598502 DEBUG after step 4 matching_overload_index=Multiple([0, 1]) signature=Overload[(fn: () -> str) -> str, (fn: () -> None) -> None]
2025-12-09 20:59:06.598778 DEBUG after step 5 matching_overload_index=Multiple([0, 1]) signature=Overload[(fn: () -> str) -> str, (fn: () -> None) -> None]
2025-12-09 20:59:06.598812 DEBUG return_ty: Unknown
2025-12-09 20:59:06.598905 DEBUG calling arguments arguments=def foo() -> None signature=Overload[(fn: () -> str) -> str, (fn: () -> None) -> None]
2025-12-09 20:59:06.599166 DEBUG after step 1 matching_overload_index=Multiple([0, 1]) signature=Overload[(fn: () -> str) -> str, (fn: () -> None) -> None]
2025-12-09 20:59:06.599203 DEBUG after step 2 matching_overload_index=Single(1) signature=Overload[(fn: () -> str) -> str, (fn: () -> None) -> None]
2025-12-09 20:59:06.599223 DEBUG return_ty: None
2025-12-09 20:59:06.599277 DEBUG calling arguments arguments=def foo() -> None signature=Overload[(fn: () -> str) -> str, (fn: () -> None) -> None]
2025-12-09 20:59:06.599309 DEBUG after step 1 matching_overload_index=Multiple([0, 1]) signature=Overload[(fn: () -> str) -> str, (fn: () -> None) -> None]
2025-12-09 20:59:06.59934 DEBUG after step 2 matching_overload_index=Single(1) signature=Overload[(fn: () -> str) -> str, (fn: () -> None) -> None]
2025-12-09 20:59:06.599358 DEBUG return_ty: None
```

You can see that the return type of the callable argument is `Divergent`. So, what's happening is that when trying to infer the function definition `foo`, we're calling `asdf(foo)` which needs the return type and that calls `definition_expression_ty` on the function definition itself forming a cycle.

---

_Unassigned @sharkdp by @sharkdp on 2025-12-09 16:34_

---

_Assigned to @dhruvmanila by @sharkdp on 2025-12-09 16:34_

---

_Added to milestone `Beta` by @carljm on 2025-12-09 17:26_

---

_Comment by @dhruvmanila on 2025-12-12 03:33_

This is now resolved on `main`: https://play.ty.dev/dee3f534-cab7-4b4d-bd8b-3821dfa46b69 🎉 

---

_Closed by @dhruvmanila on 2025-12-12 03:33_

---
