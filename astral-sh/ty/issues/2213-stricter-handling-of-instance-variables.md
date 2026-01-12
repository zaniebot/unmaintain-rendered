```yaml
number: 2213
title: Stricter handling of instance variables
type: issue
state: closed
author: hauntsaninja
labels:
  - question
  - configuration
assignees: []
created_at: 2025-12-24T23:15:12Z
updated_at: 2025-12-26T17:58:02Z
url: https://github.com/astral-sh/ty/issues/2213
synced_at: 2026-01-12T15:54:26Z
```

# Stricter handling of instance variables

---

_@hauntsaninja_

### Question

```python
class X:
    def __init__(self, foo: int) -> None:
        self.foo = foo

    def whatever(self) -> None:
        self.foo + 1

def main(mfoo: int | None) -> None:
    x = X(5)
    x.foo = mfoo
    w.whatever()
```

https://play.ty.dev/32c2417c-5f39-4170-9f72-81efca658ce3

I would like a way to run ty such that it complains about the above program.

All of mypy, pyright, pyrefly complain about this.

(In general, I think many users would consider this to be a fully typed program. If ty is going to have novel interpretations of gradual typing (yay! innovation!), it becomes really important to have documented configurations that users can set which ensures that they don't get bitten, and I don't see one for this)

This has to be a duplicate, but I couldn't find one — sorry about that!

### Version

_No response_

---

_Label `question` added by @hauntsaninja on 2025-12-24 23:15_

---

_Comment by @hauntsaninja on 2025-12-24 23:16_

The type inlays in the LSP don't even show the Unknown, making this extra pernicious

---

_Comment by @hauntsaninja on 2025-12-24 23:18_

I guess this is maybe a duplicate of this comment? https://github.com/astral-sh/ty/issues/1240#issuecomment-3322174806

---

_Comment by @MichaReiser on 2025-12-25 17:14_

I assume this is about the widening of `foo`. There has been some discussion about a strict-mode setting in https://github.com/astral-sh/ty/issues/527, which this relates to . 

I'm sure Carl or Alex will give you a more detailed answer once they're back from their PTO. 

---

_Label `configuration` added by @MichaReiser on 2025-12-25 17:15_

---

_Comment by @carljm on 2025-12-26 17:58_

Thanks -- I agree that we should emit a diagnostic on this program. There's two ways that might happen: one is discussed in #1515, which is that we'd have a special case to consider an implicit instance attribute "annotated" if it is directly assigned from an annotated `__init__` argument. That's sufficient for this example, but it's fairly narrow, and would be easy to construct slightly more complex examples where it would no longer apply -- and then that unexpected "cliff" would also become a foot-gun. The other is #1240, where we plan to add an option to turn off the gradual-guarantee unioning with `Unknown` entirely; and I think that should probably be the default, at least for CLI use.

I don't think the gradual guarantee is novel (it's the standard definition of gradual typing in the academic literature), but it is novel for Python type checkers. I think it was an interesting and worthwhile experiment, but my interpretation of the results of that experiment at this point is that it doesn't work well for users who want to write typed Python -- it requires too many verbose/redundant annotations in order to get a fully typed program.

---

_Closed by @carljm on 2025-12-26 17:58_

---
