---
number: 1910
title: "ty: ignore[unresolved-import] fails for multi-line import"
type: issue
state: open
author: riklopfer
labels:
  - suppression
assignees: []
created_at: 2025-12-15T22:16:46Z
updated_at: 2025-12-31T15:42:43Z
url: https://github.com/astral-sh/ty/issues/1910
synced_at: 2026-01-10T01:51:14Z
---

# ty: ignore[unresolved-import] fails for multi-line import

---

_Issue opened by @riklopfer on 2025-12-15 22:16_

### Summary

Expected this to work per https://docs.astral.sh/ty/suppression/#ty-suppression-comments

```python
def main():
    # ignore works
    from java.util import LinkedList, ArrayList  # ty: ignore[unresolved-import]

    # ingore works
    from java.util import (  # ty: ignore[unresolved-import]
        LinkedList,
        ArrayList,
    )

    # ingore fails
    from java.util import (
        LinkedList,
        ArrayList,
    )  # ty: ignore[unresolved-import]

```

### Version

ty 0.0.1-alpha.34 (ef3d48ac4 2025-12-12)

---

_Label `suppression` added by @carljm on 2025-12-16 01:18_

---

_Comment by @MichaReiser on 2025-12-16 07:41_

Thanks.

This is another instance where it would be beneficial to have an optional `suppression_range` that can be wider than the actual diagnostic range.

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:42_

---
