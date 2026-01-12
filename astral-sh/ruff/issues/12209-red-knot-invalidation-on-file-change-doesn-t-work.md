```yaml
number: 12209
title: "[red-knot] invalidation on file change doesn't work in CLI with relative paths"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-07-05T16:34:40Z
updated_at: 2024-07-09T07:52:14Z
url: https://github.com/astral-sh/ruff/issues/12209
synced_at: 2026-01-12T15:54:51Z
```

# [red-knot] invalidation on file change doesn't work in CLI with relative paths

---

_@carljm_


Given these files:

```
# main.py
from typing import override
from base import B

foo = override

class C(B):
    @foo
    def method(self): pass

# base.py
class B: pass

# typing.py
def override(func):
    return func
```

Running `cargo run --bin red_knot path/to/that/dir/main.py` correctly gives a bad-override diagnostic.

If we remove the `@foo` line in `main.py` and re-run the CLI from start, it correctly does not give a diagnostic.

But in either case if we add or remove the `@foo` line while the CLI is still running, it appears to re-execute some queries, but it fails to correctly account for the change (the diagnostic remains present or absent as it was on the first run, disregarding the change.)

---

_Label `red-knot` added by @carljm on 2024-07-05 16:34_

---

_Comment by @carljm on 2024-07-05 16:37_

From the traces after the file change, it looks like it's just not recognizing the file has been modified at all; there are some `file_system` queries but no semantic index of `main.py` or anything else.

---

_Comment by @carljm on 2024-07-05 17:33_

It looks like this is a path normalization issue. Invalidation works correctly if I run `cargo run --bin red_knot /absolute/path/to/the/dir/main.py`, but fails if I run `cargo run --bin red_knot ../path/to/the/dir/main.py`.

---

_Renamed from "[red-knot] invalidation on file change doesn't work in CLI" to "[red-knot] invalidation on file change doesn't work in CLI with relative paths" by @carljm on 2024-07-05 17:35_

---

_Comment by @MichaReiser on 2024-07-05 19:43_

Possibly related https://github.com/astral-sh/ruff/issues/11907

---

_Closed by @MichaReiser on 2024-07-09 07:52_

---
