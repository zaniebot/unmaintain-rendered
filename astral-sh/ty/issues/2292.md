```yaml
number: 2292
title: Wrong class seen from asyncio.create_datagram_endpoint
type: issue
state: closed
author: romuald
labels: []
assignees: []
created_at: 2025-12-31T15:37:09Z
updated_at: 2026-01-06T21:10:53Z
url: https://github.com/astral-sh/ty/issues/2292
synced_at: 2026-01-10T01:56:41Z
```

# Wrong class seen from asyncio.create_datagram_endpoint

---

_Issue opened by @romuald on 2025-12-31 15:37_

### Summary

When using protocol instances returned from `asyncio.create_datagram_endpoint`, it seems like ty is guessing the wrong (parent's parent) class instead of the actual one being used

Here is a minimal example, (almost) copied from python documentation

```python
import asyncio
from time import time

from typing import reveal_type


class EchoServerProtocol(asyncio.DatagramProtocol):
    start: float

    def connection_made(self, transport):
        self.transport = transport

    def datagram_received(self, data, addr):
        message = data.decode()
        print('Received %r from %s' % (message, addr))
        print('Send %r to %s' % (message, addr))
        self.transport.sendto(data, addr)

    def __init__(self):
        super().__init__()
        self.start = time()


async def main():
    print("Starting UDP server")

    loop = asyncio.get_event_loop()

    transport, protocol = await loop.create_datagram_endpoint(
        EchoServerProtocol,
        local_addr=('127.0.0.1', 9999))

    reveal_type(protocol)  # BaseProtocol instead of EchoServerProtocol

    print(protocol.start)

    try:
        await asyncio.sleep(3600)
    finally:
        transport.close()


if __name__ == '__main__':
    asyncio.run(main())

```

When analysed, ty guesses the type of `protocol` as BaseProtocol (the parent class of DatagramProtocol), the error is `error[unresolved-attribute]: Object of type `BaseProtocol` has no attribute `start``

The actual MRO at runtime is:
```
<class '__main__.EchoServerProtocol'>,
<class 'asyncio.protocols.DatagramProtocol'>,
<class 'asyncio.protocols.BaseProtocol'>,
<class 'object'>
```

Note that **not** inheriting from `asyncio.DatagramProtocol` will not raise an issue but the type will then be `Unknown`

### Version

0.0.8

---

_Added to milestone `Stable` by @carljm on 2026-01-06 01:38_

---

_Comment by @carljm on 2026-01-06 01:40_

Thanks for the report! I'm pretty sure we already have a report of this issue (base classes being wrongly inferred instead of a subclass, when matching a type object against a generic `Callable` annotation), but I'm not able to find it at the moment.

---

_Closed by @carljm on 2026-01-06 21:10_

---
