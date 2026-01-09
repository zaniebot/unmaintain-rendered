---
number: 15275
title: "Rule request: Detect dataclass() usage with Enum and its subclasses"
type: issue
state: closed
author: correctmost
labels:
  - rule
  - accepted
assignees: []
created_at: 2025-01-05T19:43:11Z
updated_at: 2025-01-06T13:44:22Z
url: https://github.com/astral-sh/ruff/issues/15275
synced_at: 2026-01-07T13:12:16-06:00
---

# Rule request: Detect dataclass() usage with Enum and its subclasses

---

_Issue opened by @correctmost on 2025-01-05 19:43_

### Motivation

There was a [bug in archinstall](https://github.com/archlinux/archinstall/issues/3055) because a `dataclass` decorator was incorrectly applied to an `Enum`:

```python
@dataclass
class Audio(Enum):
    NoAudio = 'No audio server'
    Pipewire = 'pipewire'
    ...
```

The [Python docs](https://docs.python.org/3/howto/enum.html#dataclass-support) warn about such usage, but I am not aware of any linter that currently catches the error:

> Adding dataclass() decorator to Enum and its subclasses is not supported. It will not raise any errors, but it will produce very strange results at runtime, such as members being equal to each other

Note: The docs mention that dataclass mixins can be used with enums, so there are some valid combinations of the constructs.

### Related discussions
- https://github.com/python/cpython/issues/114803
- https://github.com/python/cpython/pull/114891

### Documentation
- https://docs.python.org/3/howto/enum.html#dataclass-support

### Real world bugs
- https://github.com/archlinux/archinstall/pull/3058

---

_Comment by @InSyncWithFoo on 2025-01-05 21:45_

There do seem to be [quite a few bugs](https://sourcegraph.com/search?q=context:global+%40%28%3F:dataclasses%5C.%29%3Fdataclass%5Cn%5B+%5Ct%5D*class%5Cs%2B%5Cw%2B%5C%28%5B%5E%28%29%5D*Enum%5Cb&patternType=regexp&case=yes&sm=0) of this pattern.

---

_Comment by @MichaReiser on 2025-01-06 08:59_

Thanks, this makes sense to me

---

_Label `rule` added by @MichaReiser on 2025-01-06 08:59_

---

_Label `accepted` added by @MichaReiser on 2025-01-06 08:59_

---

_Referenced in [astral-sh/ruff#15299](../../astral-sh/ruff/pulls/15299.md) on 2025-01-06 11:51_

---

_Closed by @MichaReiser on 2025-01-06 13:44_

---

_Referenced in [archlinux/archinstall#3055](../../archlinux/archinstall/issues/3055.md) on 2025-01-06 19:31_

---
