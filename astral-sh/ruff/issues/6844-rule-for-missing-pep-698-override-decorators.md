```yaml
number: 6844
title: "Rule for missing PEP 698 `@override` decorators?"
type: issue
state: open
author: guillp
labels:
  - rule
  - type-inference
  - python312
assignees: []
created_at: 2023-08-24T08:07:36Z
updated_at: 2025-10-29T10:53:43Z
url: https://github.com/astral-sh/ruff/issues/6844
synced_at: 2026-01-10T11:09:49Z
```

# Rule for missing PEP 698 `@override` decorators?

---

_Issue opened by @guillp on 2023-08-24 08:07_

An idea for a new rule: checking that methods in subclasses that override methods from the parent are decorated with `@override`.
This decorator is defined in [PEP 698](https://peps.python.org/pep-0698/) and is meant to help both linters and developpers to recognize methods that are overriding methods from a parent class.

Example:

```python

class A:
    def foo(self) -> None:
        print("foo from class A!")

class B(A):
    def foo(self) -> None:
        print("foo from class B!")
```

A rule would detect that the `@override` decorator is missing on `B.foo()` since it overrides `A.foo()`.

Note that until python 3.12, `override` is only available from `typing-extensions`.

---

_Comment by @zanieb on 2023-08-24 20:01_

TIL!

This sounds good to me as a great Python 3.12 milestone although I do not think it should be enabled by default.



---

_Label `rule` added by @zanieb on 2023-08-24 20:01_

---

_Label `python312` added by @zanieb on 2023-08-24 20:01_

---

_Comment by @charliermarsh on 2023-08-24 20:31_

The challenge here is that we won’t be able to enforce this except for cases in which the parent class and subclass are defined in the same file (until we have multi-file analysis).

---

_Comment by @zanieb on 2023-08-24 20:41_

Great point, it may not be worth prioritizing until then since there will be many false negatives

---

_Label `multifile-analysis` added by @charliermarsh on 2023-09-21 00:54_

---

_Comment by @JonathanPlasse on 2023-09-22 11:19_

There is also the inverse problem of having `@override` on a class without bases, or `@overload` on a function without the function without `@overload`.

---

_Comment by @aaronsteers on 2023-12-21 19:00_

I found this thread while looking for the same. Are there any other linters that currently support this? 

Asked also here:

- https://github.com/mkorpela/overrides/issues/120



---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:33_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---

_Comment by @illusional on 2025-01-31 17:45_

Is this something that might be more appropriate for red-knot?

Edit: I see that it was tagged as type-inference by Micha, who is working on red-knot.

---

_Comment by @folkvir on 2025-10-29 10:53_

up! 🫡  Is there is any work on this since then? We're also debating on how to properly define subclassed methods and having a rule about enforcing the usage of `@override` would be very appreciated! 

---
