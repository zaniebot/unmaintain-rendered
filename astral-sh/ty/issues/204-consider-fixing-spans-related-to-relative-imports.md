```yaml
number: 204
title: consider fixing spans related to relative imports
type: issue
state: open
author: BurntSushi
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2025-02-05T18:45:30Z
updated_at: 2025-11-18T16:10:22Z
url: https://github.com/astral-sh/ty/issues/204
synced_at: 2026-01-12T15:54:22Z
```

# consider fixing spans related to relative imports

---

_@BurntSushi_

### Description

This issue originally sprouted from here: https://github.com/astral-sh/ruff/pull/15976#discussion_r1943452993

Consider this Python program:

```py
from ...zqzqzqzq import hi
```

Running red-knot at present gives you this:

```
$ run-red-knot -q pr1 -- check
error: lint:unresolved-import
 --> /home/andrew/astral/ruff/play/diags/play/test.py:1:9
  |
1 | from ...zqzqzqzq import hi
  |         ^^^^^^^^ Cannot resolve import `...zqzqzqzq`
  |
```

The range here is a little inconsistent with the actual message. The range is only on the module name, but the message includes the preceding `...`. Indeed, if we futz with the Python program a bit more, we can somewhat observe what's going wrong:

```py
from . . . zqzqzqzq import hi
```

Gives this:

```
$ run-red-knot -q pr1 -- check
error: lint:unresolved-import
 --> /home/andrew/astral/ruff/play/diags/play/test.py:1:12
  |
1 | from . . . zqzqzqzq import hi
  |            ^^^^^^^^ Cannot resolve import `...zqzqzqzq`
  |
```

Here, the diagnostic message is showing something that does not match the actual source code.

I believe the underlying problem is the AST definition of a `from import`:

https://github.com/astral-sh/ruff/blob/a84b27e679137697bdb994e9c5f79d366fb4a945/crates/ruff_python_ast/src/nodes.rs#L291-L298

Namely, it normalizes the level based on the preceding dots. But in doing so, this drops the range associated with the dots.

---

_Label `bug` added by @BurntSushi on 2025-02-05 18:45_

---

_Comment by @BurntSushi on 2025-02-05 18:46_

An alternative is to change the diagnostic message to make it in sync with the range. The range isn't arguably _wrong_ per se, but I would guess that including the dots is more semantically correct? (Because the dots may be what leads to a module import being unresolvable.)

---

_Label `diagnostics` added by @BurntSushi on 2025-02-05 18:46_

---

_Renamed from "[red-knot] consider fixing spans related to relative imports" to "consider fixing spans related to relative imports" by @MichaReiser on 2025-05-07 15:26_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:01_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
