---
number: 19484
title: "Function arguments of type `str` that should be `Literal`"
type: issue
state: open
author: jlopezpena
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-22T12:39:06Z
updated_at: 2025-07-29T16:14:25Z
url: https://github.com/astral-sh/ruff/issues/19484
synced_at: 2026-01-07T13:12:16-06:00
---

# Function arguments of type `str` that should be `Literal`

---

_Issue opened by @jlopezpena on 2025-07-22 12:39_

### Summary

Hi ruff team!

I find myself seeing a pattern like this many times:

```python
def foo(bar: str) -> None:
    if bar == "something":
        print("This is something")
    elif bar == "something_else":
        print("And this is something else!")
    else:
        raise ValueError(f"I don't know how to deal with {bar}!")
```
(or the same thing using `match` instead of `if-elif` chains)

Whenever I see this pattern in code reviews, I ask members of my team to change the type to `Literal["something", "something_else"]` as good practise. It would be awesome to have the option to enable that as a `ruff` rule!

---

_Label `rule` added by @ntBre on 2025-07-22 12:56_

---

_Label `needs-decision` added by @ntBre on 2025-07-22 12:56_

---

_Renamed from "Function arguments ot type `str` that should be `Literal`" to "Function arguments of type `str` that should be `Literal`" by @AlexWaygood on 2025-07-22 16:09_

---

_Comment by @MichaReiser on 2025-07-23 06:49_

I think what's tricky about this rule is that it also requires analyzing all call sites to know if they pass a literal or a `str`. Some might also argue that this should be an enum ;)

---

_Comment by @jlopezpena on 2025-07-23 09:17_

I think it would be enough to check the function call where the value dispatching pattern is found. If there are calls inside that use the parameter, the type checker will complain if something is amiss.

In many cases, yes, implementing a custom `Enum` or `StrEnum` is the "correct" approach, but that requires changing code in a way that can potentially break things, whereas changing type annotation should not have effects in runtime. My personal rule of thumb that I try to drill into my team is to define an enum if the same value checking happens in more than one function.

---

_Comment by @MichaReiser on 2025-07-23 09:25_

> I think it would be enough to check the function call where the value dispatching pattern is found. If there are calls inside that use the parameter, the type checker will complain if something is amiss.

We strive to avoid rules that conflict with type checkers. They tend to be annoying because a user then has to choose between satisfying Ruff or the type checker. That's why the rule either has to be clever enough to avoid making type checkers angry (which we don't have the tooling for just yet) 


---

_Comment by @jlopezpena on 2025-07-25 09:53_

I understand. Regardless of what the final decision is, thanks for taking my proposal into consideration!

---

_Comment by @nickdrozd on 2025-07-29 16:14_

This is a cool idea, could be very useful. But also it is sure to prone to false positives and generally very "noisy". So I think it would be a great candidate for a "restriction" lint class like Clippy has.

---
