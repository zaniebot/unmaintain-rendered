```yaml
number: 521
title: "Support truthiness type narrowing for checks involving `all(…)` / `any(…)`"
type: issue
state: open
author: Julian-J-S
labels:
  - wish
  - narrowing
assignees: []
created_at: 2025-05-27T09:05:44Z
updated_at: 2026-01-05T20:17:02Z
url: https://github.com/astral-sh/ty/issues/521
synced_at: 2026-01-12T15:54:23Z
```

# Support truthiness type narrowing for checks involving `all(…)` / `any(…)`

---

_@Julian-J-S_

A python concept I often see in functions is checking if a certain set of variables is not None.

Checking that individually with `if not x: return` works fine.

But checking for multiple variables at a time with `if not all(...)` seems to break currently type checkers which is unfortunate.

```python
data: dict[str, str] = {
    "a": "hello",
    "b": "world",
}

a_val = data.get("a")
b_val = data.get("b")

def test1():  # >>> This works 👍 
    if not a_val or not b_val:
        return

    _ = a_val * 2  # ✅ 
    _ = b_val * 2  # ✅ 


def test2():  # This breaks 💥 
    if not all([a_val, b_val]):
        return

    _ = a_val * 2  # 🛑 
    _ = b_val * 2  # 🛑 

```

Any way this could be achieved? 🤓 

---

_Label `narrowing` added by @AlexWaygood on 2025-05-27 11:15_

---

_Comment by @sharkdp on 2025-05-27 11:52_

Thank you for reporting this.

This is *technically* possible, but would require special-casing `all(…)` calls, and explicit matching on various kinds of inner iterables.

Given that no other type checker supports this either, I'm questioning if this really something that we should implement. The alternative of `not a or not b or not c or not …` or `not (a and b and c and …)` does not seem much more verbose than `not all([a, b, c, …])`? Is this really a common pattern (in code that would benefit from this kind of type narrowing)?

---

_Renamed from "Infer "Nullability" (non-None) from `not all(...)`" to "Support truthiness type narrowing for checks involving `all(…)` / `any(…)`" by @sharkdp on 2025-05-27 11:53_

---

_Label `wish` added by @MichaReiser on 2025-05-27 12:02_

---

_Comment by @Julian-J-S on 2025-05-27 14:03_

Interesting thoughts 🤔 

I have seen this a few times and many ai tools / copilots seem to suggest that regularly so this must be based on something 😆 .

But I agree, this is not ideal.
Maybe a ruff rule could actualy suggest and auto-fix that.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Comment by @jelle-openai on 2026-01-05 17:23_

I ran into some errors where it would be helpful if ty supported narrowing on `all()`.

Code looks like this:

```python
def f(x: list[str]): ...

def _(value: object):
    if isinstance(value, list) and all(isinstance(item, str) for item in value):
        f(value)
```

(And similarly with using `isinstance(..., dict)` and then passing the object to something that needs `dict[str, object]`.)

While other type checkers do not support this, narrowing here is uniquely useful with `ty` because it narrows `value` to `Top[list[Any]]` instead of `list[Any]`, so operations that other type checkers would accept are rejected by ty.



---

_Comment by @carljm on 2026-01-05 20:16_

I think this code example is still unsound, though? Just because `value` is a list at runtime and it happens to contain all strings at runtime, does not mean we can type it as `list[str]`. It may elsewhere (that is, in the caller) be typed as e.g. `list[Literal["foo" | "bar"]]`, or `list[LiteralString]`.

I think that we could safely type `value` within the `if` as `Top[list[Unknown]] & Iterable[str]`? Which I think should then be assignable to e.g. `Sequence[str]`, though this probably requires some logic we don't currently have.

---
