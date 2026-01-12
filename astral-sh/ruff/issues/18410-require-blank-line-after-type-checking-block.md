```yaml
number: 18410
title: Require blank line after type-checking block
type: issue
state: open
author: injust
labels:
  - formatter
  - style
  - needs-design
assignees: []
created_at: 2025-06-01T08:01:09Z
updated_at: 2025-06-02T09:15:41Z
url: https://github.com/astral-sh/ruff/issues/18410
synced_at: 2026-01-12T15:54:56Z
```

# Require blank line after type-checking block

---

_@injust_

### Summary

I am used to the Ruff formatter requiring 1-2 blank lines after imports. However, after enabling the [flake8-type-checking (TC)](https://docs.astral.sh/ruff/rules/#flake8-type-checking-tc) rules (which move imports into a type-checking block), the Ruff formatter does not add a blank line after the `if TYPE_CHECKING: ...` block.

Example: https://play.ruff.rs/cad6eeef-b192-426b-b97c-68d7b73f48b2?secondary=Format

This is the formatted original code:

```python
from __future__ import annotations

from pathlib import Path

path: Path
```

This is the formatted code with the TC003 fix:

```python
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from pathlib import Path
path: Path
```

---

_Label `formatter` added by @ntBre on 2025-06-01 14:24_

---

_Label `style` added by @MichaReiser on 2025-06-02 09:14_

---

_Comment by @MichaReiser on 2025-06-02 09:15_

More generally: Should the formatter enforce a blank line after conditional import blocks. This can be:

* `if TYPE_CHECKING` (may have an else branch)
* other conditional imports, e.g. dependent on a python version
* conditional imports using `try...except`

What's the heuristic to detect such import blocks? 

---

_Label `needs-design` added by @MichaReiser on 2025-06-02 09:15_

---
