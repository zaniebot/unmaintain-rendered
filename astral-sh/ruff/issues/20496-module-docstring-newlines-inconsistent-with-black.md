```yaml
number: 20496
title: module_docstring_newlines inconsistent with black
type: issue
state: closed
author: Redoubts
labels:
  - question
  - formatter
assignees: []
created_at: 2025-09-21T19:03:13Z
updated_at: 2025-09-22T19:18:58Z
url: https://github.com/astral-sh/ruff/issues/20496
synced_at: 2026-01-10T11:09:59Z
```

# module_docstring_newlines inconsistent with black

---

_Issue opened by @Redoubts on 2025-09-21 19:03_

### Summary

Not sure if this is on you or black, but consider

```
#!/python
"""
docstring
"""
from __future__ import annotations

import os
```

if I use `black` on this file, it stays unchanged. If I use `ruff format`, a newline is added after the docstring. If I remove the shebang, black will add the newline.

Who's right?

### Version

ruff 0.13.1 vs black, 25.9.0

---

_Comment by @MichaReiser on 2025-09-22 07:30_

It could be that Black fails to recognize the docstring as a module-level docstring and instead treats it as a normal statement expression, containing a string. In which case, no empty line should be added before the import (see https://play.ruff.rs/c286e261-c5fc-4816-9233-ef6f6dfbd6a9).

Either way, I don't see why a shebang should change the number of empty lines between the docstring and import statement, and consider Ruff's behavior correct.

For reference, @Redoubts also reported this on black https://github.com/psf/black/issues/4762

---

_Closed by @MichaReiser on 2025-09-22 07:30_

---

_Label `formatter` added by @MichaReiser on 2025-09-22 07:30_

---

_Label `question` added by @MichaReiser on 2025-09-22 07:30_

---

_Comment by @MeGaGiGaGon on 2025-09-22 19:18_

Yep, this is a bug in black, the comment causes it to not be recognized as a module docstring. Fix in psf/black#4764

---
