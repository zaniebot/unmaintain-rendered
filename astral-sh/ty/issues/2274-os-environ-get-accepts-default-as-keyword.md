```yaml
number: 2274
title: "os.environ.get() accepts `default` as keyword argument, but ty reports no-matching-overload"
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
  - generics
assignees: []
created_at: 2025-12-30T10:54:48Z
updated_at: 2026-01-02T07:36:24Z
url: https://github.com/astral-sh/ty/issues/2274
synced_at: 2026-01-12T15:54:26Z
```

# os.environ.get() accepts `default` as keyword argument, but ty reports no-matching-overload

---

_@adamtheturtle_

### Summary

```python
import os

os.environ.get("FOO", default="BAR")
```

This code works but `ty` errors.
`mypy` and `pyright` pass.

### Version

ty 0.0.8 (aa7559db8 2025-12-29)

---

_Comment by @adamtheturtle on 2025-12-30 10:56_

Perhaps related: https://github.com/python/cpython/issues/124675#issuecomment-2560672965 - `dict` is not strictly a `Mapping`.

---

_Comment by @MichaReiser on 2025-12-30 11:10_

I suspect this is https://github.com/astral-sh/ty/issues/1714, given that `Mapping` is a `Protocol`.

---

_Comment by @AlexWaygood on 2025-12-30 13:28_

> I suspect this is [#1714](https://github.com/astral-sh/ty/issues/1714), given that `Mapping` is a `Protocol`.

No, I don't think #1714 is related here. `Mapping` is not a protocol, and ty doesn't think it is either:

```py
from typing import Mapping, is_protocol, reveal_type

reveal_type(is_protocol(Mapping))  # Literal[False]
```

`Mapping` is an ABC, rather than a protocol. And even if it were a protocol, #1714 only concerns types that _implicitly_ subtype protocols -- `os._Environ` [_explicitly_ inherits](https://github.com/astral-sh/ruff/blob/0edd97dd41bd0529f439a38d41831fa68866d201/crates/ty_vendored/vendor/typeshed/stdlib/os/__init__.pyi#L723) from `MutableMapping[AnyStr, AnyStr]` in typeshed, and `MutableMapping` in turn [explicitly inherits](https://github.com/astral-sh/ruff/blob/0edd97dd41bd0529f439a38d41831fa68866d201/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi#L1285) from `Mapping`.

I'm not sure exactly what the issue is here, but I suspect it's some strangeness to do with constrained TypeVars.

---

_Label `generics` added by @AlexWaygood on 2025-12-30 13:28_

---

_Label `bug` added by @AlexWaygood on 2025-12-30 13:28_

---

_Comment by @RasmusNygren on 2025-12-30 17:00_

I think this is just typeshed being incorrect. Mypy also fails (https://mypy-play.net/?mypy=master&python=3.12&gist=49d939f7441874364a23bb2e937d4032) if you run it towards the master branch that has the latest typeshed stubs.

https://github.com/python/typeshed/pull/15085 changed the default parameter of`Mapping.get` to be positional-only and did not make any changes to `os._Environ` to accept default as a keyword.

Edit: opened up a typeshed issue for it: https://github.com/python/typeshed/issues/15193

---

_Comment by @carljm on 2025-12-30 22:15_

Will leave this open so we remember to validate the fix when an updated typeshed arrives, but it seems like the fix is only in typeshed. Thanks @RasmusNygren!

---

_Added to milestone `Stable` by @carljm on 2025-12-30 22:15_

---

_Comment by @AlexWaygood on 2026-01-01 01:30_

I confirmed that this was fixed by https://github.com/astral-sh/ruff/pull/22321.

Thanks again for the report @adamtheturtle, and thanks for the typeshed fix @RasmusNygren!

---

_Closed by @AlexWaygood on 2026-01-01 01:30_

---

_Comment by @adamtheturtle on 2026-01-02 07:36_

Thanks folks for digging and fixing!

I hit this because I (built and) use [mypy-strict-kwargs](https://github.com/adamtheturtle/mypy-strict-kwargs) - a `mypy` plugin to enforce using keyword arguments where possible. That's one use of the mypy plugin system that I don't think is ever going to be possible with public plans for ty (which I understand to be "no plugins, but support the ecosystem so that plugins aren't necessary").

---
