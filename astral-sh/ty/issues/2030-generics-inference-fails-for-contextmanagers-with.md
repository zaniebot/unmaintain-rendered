```yaml
number: 2030
title: "Generics inference fails for @contextmanagers with TypeVars."
type: issue
state: open
author: amol-mandhane
labels:
  - generics
assignees: []
created_at: 2025-12-17T19:06:59Z
updated_at: 2026-01-05T19:36:11Z
url: https://github.com/astral-sh/ty/issues/2030
synced_at: 2026-01-10T01:56:41Z
```

# Generics inference fails for @contextmanagers with TypeVars.

---

_Issue opened by @amol-mandhane on 2025-12-17 19:06_

### Summary

Repro:

```py
from contextlib import contextmanager
from collections.abc import Generator

class Base:
    pass

class Derived(Base):
    pass


def factory[T: Base](cls: type[T]) -> T:
    return cls()

d = factory(Derived)

@contextmanager
def yielder[T: Base](cls: type[T]) -> Generator[T]:
    yield cls()

with yielder(Derived) as d:  # error [invalid-argument-type] "Argument is incorrect: Expected `type[T@yielder]`, found `<class 'Derived'>`! 
    print(d)
```

https://play.ty.dev/b74578ac-adaf-4c71-a316-e813c7ad701c

Calling `yielder(Derived)` should infer `T` as `Derived` (since `Derived` is a subclass of `Base`), matching the behavior of the regular function `factory(Derived)`.

---

_Renamed from "Generic inference fails for @contextmanagers with TypeVars." to "Generics inference fails for @contextmanagers with TypeVars." by @amol-mandhane on 2025-12-17 19:07_

---

_Comment by @amol-mandhane on 2025-12-17 20:30_

Thinking about it, this might be because ty doesn't resolve type of contextmanager decorator as expected rather than the typevar resolution itself.

---

_Label `generics` added by @carljm on 2025-12-17 20:38_

---

_Added to milestone `Stable` by @carljm on 2025-12-17 20:38_

---

_Comment by @carljm on 2025-12-17 20:38_

Thanks for the nicely minimized report! Yeah, it does seem like we are failing to properly bind the typevar to the returned callable type from the decorator here.

---

_Comment by @eclbg on 2025-12-19 09:17_

Another similar minimized report with the workaround we've seen that works: https://play.ty.dev/dac79a5c-09b9-4d75-ba07-e1d59e474700

I think it's the same issue as in OP's playground.

---

_Comment by @montasaurus on 2025-12-31 04:48_

Not sure if this is the same issue or not, but generators yielding `Self` are inferred as `None`

```python
class Z:
    @classmethod
    @asynccontextmanager
    async def init_async(cls) -> AsyncGenerator[Self]:
        await asyncio.sleep(0.1)
        yield cls()

    @classmethod
    @contextmanager
    def init_sync(cls) -> Generator[Self]:
        yield cls()
    
    @classmethod
    def get(cls) -> Self:
        return cls()


async def check_context_manager():
    async with Z.init_async() as g:
        reveal_type(g) # Revealed type: `None`

    with Z.init_sync() as gs:
        reveal_type(gs) # Revealed type: `None`
    
    gg = Z.get()
    reveal_type(gg) # Revealed type: `Z`
```

https://play.ty.dev/7c4bb2d5-6cb9-458f-bc2b-ae792358a8eb

---

_Comment by @eclbg on 2026-01-02 19:52_

I'm working on this one. I have a working solution and I'm reviewing it again before I submit the PR in the next few days. It will solve both the TypeVar and the Self issues with contextmanager.

Update: splitting in 2 PRs. First one for the Self fix submitted.

---
