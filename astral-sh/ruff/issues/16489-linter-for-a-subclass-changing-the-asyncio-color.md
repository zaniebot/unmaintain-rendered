```yaml
number: 16489
title: "linter for a subclass changing the asyncio \"color\" of a method"
type: issue
state: closed
author: NorthIsUp
labels:
  - type-inference
assignees: []
created_at: 2025-03-04T03:39:08Z
updated_at: 2025-03-04T09:31:23Z
url: https://github.com/astral-sh/ruff/issues/16489
synced_at: 2026-01-10T11:09:57Z
```

# linter for a subclass changing the asyncio "color" of a method

---

_Issue opened by @NorthIsUp on 2025-03-04 03:39_

### Summary

I don't know if this describes it well, but I'd love a lint error for if a subclass changes the aio typing of a method.


```
class Foo:
    async def run(self) -> None:
        await asyncio.sleep(1)
        return


class Bar(Foo):
    def run(self) -> None:  # <- lint error, changes from awaitable to callable
        return None
```

and vice versa, where the superclass is `def whatever` and the subclass is `async def whatever`


thanks!

---

_Comment by @Avasam on 2025-03-04 08:02_

Unless your class and subclasses are defined in the same file, Ruff won't be able to check this for a while still (see: multifile analysis and redknot).

This is more in the ballpark of a type-checker (mypy/pyright)

---

_Comment by @MichaReiser on 2025-03-04 09:31_

I think this makes sense to me as a rule that flags incompatible overrides but Ruff currently lacks the capabilities to do this cross-module (as @Avasam pointed out). 

I think we should add this once we integrate our new semantic multi-file analysis engine into Ruff but I'll close this for now, as it isn't actionable for us in the near future.

---

_Closed by @MichaReiser on 2025-03-04 09:31_

---

_Label `type-inference` added by @MichaReiser on 2025-03-04 09:31_

---
