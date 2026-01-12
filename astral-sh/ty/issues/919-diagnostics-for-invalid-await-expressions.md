```yaml
number: 919
title: "Diagnostics for invalid `await` expressions"
type: issue
state: closed
author: sharkdp
labels:
  - good first issue
  - diagnostics
assignees: []
created_at: 2025-07-31T10:55:59Z
updated_at: 2025-08-15T07:09:30Z
url: https://github.com/astral-sh/ty/issues/919
synced_at: 2026-01-12T15:54:24Z
```

# Diagnostics for invalid `await` expressions

---

_@sharkdp_

We currently don't emit any errors in `await xyz` expressions if `xyz` is not awaitable. We should probably do something similar to what we do for iterators or context managers, where we emit helpful diagnostics if calls to respective dunder methods fail. For example:

```py
class NotAwaitable: ...

class NotAwaitable2:
    # not async!
    def __await__(self):
        pass

async def main():
    x = await 1  # should be an error
    y = await NotAwaitable()  # should be an error
    z = await NotAwaitable2()  # should be an error
```

The entrypoint to implement this is here: https://github.com/astral-sh/ruff/blob/27b03a9d7bf779049585bb6cfbf229f18352572d/crates/ty_python_semantic/src/types.rs#L4868

---

_Label `good first issue` added by @sharkdp on 2025-07-31 10:56_

---

_Label `diagnostics` added by @sharkdp on 2025-07-31 10:56_

---

_Comment by @theammir on 2025-07-31 22:33_

I'd love to tackle this one and see where it gets me.

---

_Assigned to @theammir by @sharkdp on 2025-08-01 06:30_

---

_Comment by @theammir on 2025-08-02 19:57_

What lint do we actually report for awaiting an improperly `Awaitable` type?
For context managers, everything technically falls under `invalid-context-manager` (possibly unbound or unimplemented `__(a)enter__`/`__(a)exit__`, or implemented incorrectly).

As I see it, there are 3 erroneous cases for `__await__` as well: missing, possibly unbound, implemented incorrectly (cannot be called with no extra arguments, returns a type that is known not to be a generator).
I couldn't find a dedicated lint for `await`-related errors, so does it make sense to use a combination of `unresolved-attribute`, `possibly-unbound-implicit-call` etc., or do we need a new lint?

---

_Comment by @sharkdp on 2025-08-02 23:06_

Adding a new lint sounds like a good idea to me.

---

_Closed by @MichaReiser on 2025-08-15 07:09_

---
