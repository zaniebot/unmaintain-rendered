```yaml
number: 2117
title: Re-enable boundness analysis for implicit instance attributes
type: issue
state: open
author: sharkdp
labels:
  - control flow
  - attribute access
assignees: []
created_at: 2025-12-19T17:30:57Z
updated_at: 2025-12-20T00:32:30Z
url: https://github.com/astral-sh/ty/issues/2117
synced_at: 2026-01-10T01:56:41Z
```

# Re-enable boundness analysis for implicit instance attributes

---

_Issue opened by @sharkdp on 2025-12-19 17:30_

In https://github.com/astral-sh/ruff/pull/20128, we disabled boundness analysis for *implicit* instance attributes:

```py
class C:
    def __init__(self):
        if False:   
            self.x = 1

C().x  # would previously show an error, with this PR we pretend the attribute exists
```

It is possible that we can simply re-enable this now that we have a better strategy for dealing with these cyclic type inference problems.

We probably don't want a full-blown revert of that PR. I like `TypeQualifiers::IMPLICIT_INSTANCE_ATTRIBUTE` much better than `TypeQualifiers::POSSIBLY_UNBOUND_IMPLICIT_ATTRIBUTE`, so maybe we can prevent bringing the latter back.

For some reason, I don't see any regression tests for https://github.com/astral-sh/ty/issues/758 in that PR, so we should probably add them when we resolve this issue.

---

_Label `control flow` added by @sharkdp on 2025-12-19 17:30_

---

_Label `attribute access` added by @sharkdp on 2025-12-19 17:30_

---

_Added to milestone `Stable` by @sharkdp on 2025-12-19 17:31_

---

_Comment by @mtshiba on 2025-12-20 00:32_

https://github.com/astral-sh/ruff/pull/21822

I think my PR is close to what's described in this issue.

I'm leaving it as a draft because I found the following false positive:

https://github.com/astral-sh/ruff/pull/21822/files#diff-19ac4a4d9337821d0bbfb544ab4e3fda87df59c6ea06065b77d3fcf5cf682ec2R650-R666

```py
from typing import Callable

class C:
    def __init__(self, cond: Callable[[], bool]) -> None:
        i = 0
        while cond():
            i += 1

        # TODO: this should be `int`
        reveal_type(i)  # revealed: Literal[0, 1]
        if i < 100:
            return
        self.x = 1

# TODO: this should be a possibly-unresolved-attribute error, not unresolved-attribute
# error: [unresolved-attribute]
reveal_type(C(lambda: False).x)  # revealed: Unknown
```

Resolving this requires accurate control flow tracking (https://github.com/astral-sh/ty/issues/232).

---
