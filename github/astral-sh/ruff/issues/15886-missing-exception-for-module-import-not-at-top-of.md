---
number: 15886
title: "Missing exception for `module-import-not-at-top-of-file-e402`"
type: issue
state: closed
author: janosh
labels:
  - help wanted
assignees: []
created_at: 2025-02-02T21:57:24Z
updated_at: 2025-02-06T17:29:59Z
url: https://github.com/astral-sh/ruff/issues/15886
synced_at: 2026-01-07T13:12:16-06:00
---

# Missing exception for `module-import-not-at-top-of-file-e402`

---

_Issue opened by @janosh on 2025-02-02 21:57_

### Description

the following two codes are practically equivalent but the 2nd one gets a `E402` error in `ruff` v0.9.4
```py
import os
import sys

sys.path.append(os.path.dirname(__file__))  # sys.path.append gets special exception when appearing above imports

from package import module
```

```py
import os
import sys

sys.path += [os.path.dirname(__file__)] # get E402 here

from package import module
```

i think the bottom code should be treated same as the top, i.e. not raise an error

---

_Comment by @InSyncWithFoo on 2025-02-03 06:59_

This seems to be an excellent good first issue candidate. The changes necessary would be to modify [`is_sys_path_modification()`](https://github.com/InSyncWithFoo/ruff/blob/fa46ba2306c62a2d6bb3c1abcd7ba0afb26d9421/crates/ruff_python_semantic/src/analyze/imports.rs#L12) and [`Checker::visit_stmt()`](https://github.com/InSyncWithFoo/ruff/blob/fa46ba2306c62a2d6bb3c1abcd7ba0afb26d9421/crates/ruff_linter/src/checkers/ast/mod.rs#L455), or, in other words, mostly what #14474 did.

---

_Label `help wanted` added by @MichaReiser on 2025-02-03 07:32_

---

_Comment by @VascoSch92 on 2025-02-03 11:57_

Can I give a try?

---

_Comment by @MichaReiser on 2025-02-03 14:14_

Sure

---

_Assigned to @VascoSch92 by @MichaReiser on 2025-02-03 14:14_

---

_Referenced in [astral-sh/ruff#15980](../../astral-sh/ruff/pulls/15980.md) on 2025-02-05 20:31_

---

_Comment by @VascoSch92 on 2025-02-06 16:57_

I think this issue is closed ;-) 

---

_Closed by @MichaReiser on 2025-02-06 17:29_

---

_Comment by @MichaReiser on 2025-02-06 17:29_

thanks for the ping!

---
