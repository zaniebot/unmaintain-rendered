```yaml
number: 2281
title: No unresolved-reference error when definition exists but is not reachable
type: issue
state: open
author: carljm
labels:
  - control flow
  - unreachable-code
assignees: []
created_at: 2025-12-30T21:38:11Z
updated_at: 2025-12-30T21:38:51Z
url: https://github.com/astral-sh/ty/issues/2281
synced_at: 2026-01-10T01:56:41Z
```

# No unresolved-reference error when definition exists but is not reachable

---

_Issue opened by @carljm on 2025-12-30 21:38_

In this code, we (of course) emit an `unresolved-reference` on the use of `x` in the last line:

```py
def foo():
    x  # error: [unresolved-reference]
```

But in this code (checked under Python > 3.10), we do not, but we should:

```py
import sys

if sys.version_info < (3, 10):
    x = 1

def foo():
    x
```

The root cause is our generalized handling of reachability, where we check "is there a path from a definition to a use" without distinguishing whether it is the definition or the use that is unreachable.

---

_Added to milestone `Stable` by @carljm on 2025-12-30 21:38_

---

_Label `control flow` added by @carljm on 2025-12-30 21:38_

---

_Label `unreachable-code` added by @carljm on 2025-12-30 21:38_

---

_Comment by @carljm on 2025-12-30 21:38_

This has a similar root-cause to #2272 

---
