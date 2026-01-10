```yaml
number: 2134
title: Support deeper-nested attribute type narrowing
type: issue
state: open
author: pasky
labels:
  - narrowing
  - attribute access
assignees: []
created_at: 2025-12-20T19:55:10Z
updated_at: 2025-12-27T00:41:06Z
url: https://github.com/astral-sh/ty/issues/2134
synced_at: 2026-01-10T01:56:41Z
```

# Support deeper-nested attribute type narrowing

---

_Issue opened by @pasky on 2025-12-20 19:55_

### Summary

Common `unittest.mock` patterns used in (at least my) test suite(s) produce false positives in ty, while pyright handles them correctly. This makes it difficult to type-check test files with ty.

See the full reproducer at https://play.ty.dev/d95a7a51-6af0-486d-8ebb-a3b0ca0d0ea6

## 1. `possibly-missing-attribute` after mock assignment

When a typed attribute is replaced with `AsyncMock()` or `MagicMock()`, ty still tracks the union of possible types (`Unknown | OriginalType`). When accessing mock-specific attributes like `.assert_called()`, `.call_args`, or `.call_count` on child attributes of the mock, ty reports `possibly-missing-attribute` because it sees the original method signature in the union.

```
    agent.irc_monitor.varlink_sender = AsyncMock()
    await agent.irc_monitor.varlink_sender.send_message("#test", "hello", "server")
    agent.irc_monitor.varlink_sender.send_message.assert_called()  # ty: possibly-missing-attribute
```
produces
```
warning[possibly-missing-attribute]: Attribute `assert_called` may be missing on object of type `Unknown | (bound method VarlinkSender.send_message(target: str, message: str, server: str) -> CoroutineType[Any, Any, bool])`
```

## 2. `invalid-assignment` for lambda stubs

Monkeypatching a method with a lambda that has a compatible but not identical signature (e.g., `lambda: False` vs `def check_limit(self) -> bool`) triggers `invalid-assignment`. This is a common pattern for quickly stubbing out methods in tests.

```
class RateLimiter:
    def check_limit(self) -> bool:
        return True
agent.irc_monitor.rate_limiter.check_limit = lambda: False  # ty: invalid-assignment
```
produces
```
error[invalid-assignment]: Object of type `() -> Unknown` is not assignable to attribute `check_limit` on type `Unknown | RateLimiter
```

Both of these cases work just fine with `pyright` (1.1.407) and I think having to exclude them manually seems unreasonable. I tried to look for some best practices around ty and tests, but didn't really find much so far...

### Version

0.0.4

---

_Comment by @carljm on 2025-12-23 21:08_

Thanks for the report!

I've opened #2193 to separately track the method-patching, since the two issues reported here have separate causes and fixes. This issue can track the `AsyncMock` patching.

We do support the `AsyncMock` pattern, but currently only for one level of attribute. So `agent.irc_monitor = AsyncMock()` works, but deeper-nested attributes don't. I think this is a pretty arbitrary limitation that we can lift, though.

---

_Renamed from "Tests monkeypatching generate type errors" to "Support deeper-nested attribute type narrowing" by @carljm on 2025-12-23 21:08_

---

_Added to milestone `Stable` by @carljm on 2025-12-23 21:08_

---

_Assigned to @mtshiba by @mtshiba on 2025-12-24 11:27_

---

_Label `narrowing` added by @mtshiba on 2025-12-24 11:27_

---

_Label `attribute access` added by @mtshiba on 2025-12-24 11:27_

---

_Comment by @mtshiba on 2025-12-24 14:57_

For the first issue, the immediate workaround is to use explicit typing, which will allow narrowing to work and eliminate the false-positive errors.

```python
from unittest.mock import AsyncMock

class VarlinkSender:
    async def send_message(self, target: str, message: str, server: str) -> bool:
        return True

class IRCMonitor:
    def __init__(self) -> None:
        # explicit typing
        self.varlink_sender: VarlinkSender = VarlinkSender()

class Agent:
    def __init__(self) -> None:
        # explicit typing
        self.irc_monitor: IRCMonitor = IRCMonitor()

async def test_mock_nested_attribute():
    agent = Agent()

    agent.irc_monitor.varlink_sender = AsyncMock()
    await agent.irc_monitor.varlink_sender.send_message("#test", "hello", "server")
    # No errors
    agent.irc_monitor.varlink_sender.send_message.assert_called()
    agent.irc_monitor.varlink_sender.send_message.assert_called_once()
    args = agent.irc_monitor.varlink_sender.send_message.call_args
    count = agent.irc_monitor.varlink_sender.send_message.call_count
```

---

What follows is a detailed explanation of what's going on here (ty users don't need to read).

We're recording attribute assignments at any level, but we can't use them to perform narrowing when they're implicit. This is because we can't rule out the possibility that the member is a data descriptor when the result of `class_member` is `Defined(Unknown)`.

```python
class Foo: ...
class Mock: ...

class Bar:
    def __init__(self):
        self.foo = Foo()

class Baz:
    def __init__(self):
        self.bar = Bar()

baz = Baz()
reveal_type(baz.bar)  # Unknown | Bar
# The type of `baz` is determined to be `Baz`, so `Baz.class_member(bar) == Undefined`
# If the class member is `Undefined`, data descriptor check is not performed.
baz.bar = Mock()
reveal_type(baz.bar)  # Mock

baz = Baz()
reveal_type(baz.bar.foo)  # Unknown | Foo
# The type of `baz.bar` is `Unknown | Bar`, so `(Unknown | Bar).class_member(foo) == Defined(Unknown)`
# If the class member is `Defined`, data descriptor check is performed (and `Unknown` is a data descriptor type!)
baz.bar.foo = Mock()
reveal_type(baz.bar.foo)  # Unknown | Foo
```

Therefore, to enable such narrowing, it would be necessary to give up strict soundness and relax the narrowing application requirements.

---

_Comment by @carljm on 2025-12-27 00:41_

Thanks @mtshiba for the deeper dive, that makes sense.

Yet another reason to just eliminate `Unknown |` widening, IMO.

---
