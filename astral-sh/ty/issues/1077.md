```yaml
number: 1077
title: "Do AsyncGenerators always need `None` to be part of the SendType?"
type: issue
state: closed
author: pfaion
labels:
  - question
assignees: []
created_at: 2025-08-21T09:04:33Z
updated_at: 2025-08-22T06:59:55Z
url: https://github.com/astral-sh/ty/issues/1077
synced_at: 2026-01-10T02:06:24Z
```

# Do AsyncGenerators always need `None` to be part of the SendType?

---

_Issue opened by @pfaion on 2025-08-21 09:04_

### Question

Hi all,

I'm running into an issue with `AsyncGenerator`s requiring to send an initial `None` value to "prime" them. But this `None` will never be seen inside the generator. I don't understand if I have to essentially do a `None` check every time I receive the value in the generator? I've tried to use a SendType that does not include `None`, but then ty complains...

Maybe this is by design? It smells like bad API design on the generator interface to be honest, but wanted to know if this is something that could be circumvented or made easier by ty as well.

Here's an example that outlines the issue ([playground link](https://play.ty.dev/91607ed4-3c24-4736-80b7-1fbb4acbab48))

```python
from collections.abc import AsyncGenerator
import asyncio

### this is how I would like to type my generator, only dealing with ints
### but this is not usable see A() and B() below
async def pure_int_generator() -> AsyncGenerator[int, int]:
    for i in range(10):
        incoming = yield(i)
        print(i * incoming)

### priming with `asend(None)` works at runtime, but ty throws an error
async def A():
    gen = pure_int_generator()
    await gen.asend(None)  # Argument to bound method `asend` is incorrect: Expected `int`, found `None`
asyncio.run(A())

### priming with an int throws a runtime TypeError
async def B():
    gen = pure_int_generator()
    await gen.asend(0)     # runtime TypeError: can't send non-None value to a just-started async generator
asyncio.run(B())


### this is the only way I managed to make it work, but it always requires None-checking all received values
async def cumbersome_generator() -> AsyncGenerator[int, int | None]:
    for i in range(10):
        incoming = yield(i)
        if incoming is None:
            raise ValueError
        print(i * incoming)

async def C():
    gen = cumbersome_generator()
    await gen.asend(None)


asyncio.run(C())     # works, but cumbersome non-check!
```

### Version

_No response_

---

_Label `question` added by @pfaion on 2025-08-21 09:04_

---

_Comment by @snus-kin on 2025-08-21 14:30_

Could you use `await anext(gen)` to reach the first yield perhaps? You avoid the send typing issue then. 

```py
async def pure_int_generator() -> AsyncGenerator[int, int]:
    for i in range(10):
        incoming = yield(i)
        print(i * incoming)

async def A():
    gen = pure_int_generator()
    await anext(gen)
asyncio.run(A())
```

```bash
$ uvx ty check test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                               All checks passed!
```

Generators only take input when 'paused' at a yield.

---

_Comment by @pfaion on 2025-08-21 15:36_

Riiight, that makes sense! I wonder why the [PEP](https://peps.python.org/pep-0525/#asynchronous-generator-object) does not mention this, it only shows calling ` .send(None)` for the initial step.

Thanks, this was my missing knowledge about how to use an AsyncGenerator properly!

---

_Closed by @pfaion on 2025-08-21 15:36_

---

_Comment by @snus-kin on 2025-08-21 15:49_

From the [docs](https://docs.python.org/3/reference/expressions.html#agen.asend):

> When [asend()](https://docs.python.org/3/reference/expressions.html#agen.asend) is called to start the asynchronous generator, it must be called with [None](https://docs.python.org/3/library/constants.html#None) as the argument, because there is no yield expression that could receive the value.

---

_Comment by @pfaion on 2025-08-22 06:57_

Yeah I don't know why they wouldn't mention that you can start the generator with `await anext(gen)` as well. Because then you don't need to include `None` in the SendType! (Which is essentially the answer to my question)

---
