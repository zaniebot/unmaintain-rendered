---
number: 20656
title: "`non-pep695-generic-class` (`UP046`) - false negative on old `TypeVar` with `default` argument"
type: issue
state: closed
author: DetachHead
labels:
  - rule
  - preview
assignees: []
created_at: 2025-10-01T00:09:42Z
updated_at: 2025-10-15T14:51:57Z
url: https://github.com/astral-sh/ruff/issues/20656
synced_at: 2026-01-07T13:12:16-06:00
---

# `non-pep695-generic-class` (`UP046`) - false negative on old `TypeVar` with `default` argument

---

_Issue opened by @DetachHead on 2025-10-01 00:09_

### Summary

```py
from typing import TypeVar, Generic

T = TypeVar("T", default=int)
U = TypeVar("U")


class Foo(Generic[T]): ... # no error
class Bar(Generic[U]): ... # error
```

https://play.ruff.rs/31611e2c-077c-4291-bdeb-dd663a0f43f7

this behavior is mentioned in the [known problems section](https://docs.astral.sh/ruff/rules/non-pep695-generic-class/#known-problems) but i assume this was written before this feature was widely supported by type checkers, and i couldn't find an existing issue for this.

### Version

0.13.2

---

_Comment by @ntBre on 2025-10-01 00:55_

Yeah, this is still a [TODO](https://github.com/astral-sh/ruff/blob/bedfc6fd063e08f4d1e76ae8a780162404079298/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs#L321) from when I was first working on the rule. It's tracked in https://github.com/astral-sh/ruff/issues/15642 but only mentioned in the body, so it's probably a bit hard to find.

I'll make this a sub-issue rather than a duplicate for visibility. Contributions welcome!

We'll need to make this a preview change since the rule has been stabilized in the meantime.

---

_Label `rule` added by @ntBre on 2025-10-01 00:56_

---

_Label `preview` added by @ntBre on 2025-10-01 00:56_

---

_Referenced in [astral-sh/ruff#20660](../../astral-sh/ruff/pulls/20660.md) on 2025-10-01 03:49_

---

_Closed by @ntBre on 2025-10-15 14:51_

---
