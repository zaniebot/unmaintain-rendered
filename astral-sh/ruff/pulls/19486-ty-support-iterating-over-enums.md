```yaml
number: 19486
title: "[ty] Support iterating over enums"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/enum-iteration
created_at: 2025-07-22T13:15:54Z
updated_at: 2025-07-22T18:10:31Z
url: https://github.com/astral-sh/ruff/pull/19486
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Support iterating over enums

---

_@sharkdp_

## Summary

Infer the correct type in a scenario like this:

```py
class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3

for color in Color:
    reveal_type(color)  # revealed: Color
```

We should eventually support this out-of-the-box when https://github.com/astral-sh/ty/issues/501 is implemented. For this reason, @AlexWaygood would prefer to keep things as they are (we currently infer `Unknown`, so false positives seem unlikely). But it seemed relatively easy to support, so I'm opening this for discussion.

part of https://github.com/astral-sh/ty/issues/183

## Test Plan

Adapted existing test.

## Ecosystem analysis

```diff
- warning[unused-ignore-comment] rotkehlchen/chain/aggregator.py:591:82: Unused blanket `type: ignore` directive
```

This `unused-ignore-comment` goes away due to a new true positive.

---

_Review requested from @carljm by @sharkdp on 2025-07-22 13:15_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-22 13:15_

---

_Review requested from @dcreager by @sharkdp on 2025-07-22 13:15_

---

_Label `ty` added by @sharkdp on 2025-07-22 13:15_

---

_Comment by @github-actions[bot] on 2025-07-22 13:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
rotki (https://github.com/rotki/rotki)
- warning[unused-ignore-comment] rotkehlchen/chain/aggregator.py:591:82: Unused blanket `type: ignore` directive
- Found 1580 diagnostics
+ Found 1579 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@AlexWaygood reviewed on 2025-07-22 13:35_

The code looks good but you already know my opinion on whether it's worth adding this branch in the first place :-)

---

_@MichaReiser approved on 2025-07-22 13:59_

Alex said the code looks good ;)

---

_Comment by @sharkdp on 2025-07-22 14:09_

It doesn't look like a huge maintenance burden to me, so I'm merging this to complete my list of "high priority" enum tasks.

---

_Merged by @sharkdp on 2025-07-22 14:09_

---

_Closed by @sharkdp on 2025-07-22 14:09_

---

_Branch deleted on 2025-07-22 14:09_

---

_Comment by @AlexWaygood on 2025-07-22 16:31_

I suppose the logic here doesn't account for the fact that enums can have subclasses of `EnumMeta` as their metaclass. E.g. we reveal `Foo` here, but at runtime iterating over this enum class yields `int`s:

```py
from typing import Iterator, reveal_type
import enum

class Annoying(enum.EnumMeta):
    def __iter__(cls) -> Iterator[int]:
        yield from range(4)

class Foo(enum.Enum, metaclass=Annoying):
    A = 1
    B = 2

for f in Foo:
    reveal_type(f)
```

https://play.ty.dev/3347bb64-35ba-469a-9359-b5fe504bb81d

Having said that, we'll emit a Liskov violation on `Annoying.__iter__` once we've implemented https://github.com/astral-sh/ty/issues/166. In fact, I don't think it's possible to override `EnumMeta.__iter__` _and change its return type_ in a Liskov-compatible way. So this probably isn't a serious issue.

---

_Comment by @carljm on 2025-07-22 18:09_

FWIW, mypy issues the Liskov error, pyright does not. Both mypy and pyright do correctly infer that the iteration yields `int` in that example.

---

_Comment by @carljm on 2025-07-22 18:10_

But I think once we fix https://github.com/astral-sh/ty/issues/501 and remove this temporary approach, that should fall out anyway, so not worth worrying about now.

---
