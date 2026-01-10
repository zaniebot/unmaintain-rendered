```yaml
number: 21025
title: The opposite of ICN003
type: issue
state: open
author: jsh9
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-21T23:48:15Z
updated_at: 2025-12-06T21:30:40Z
url: https://github.com/astral-sh/ruff/issues/21025
synced_at: 2026-01-10T11:10:00Z
```

# The opposite of ICN003

---

_Issue opened by @jsh9 on 2025-10-21 23:48_

### Summary

Hi, `ICN003` says this is good and this is bad:

* Good
  * `import pandas as pd`
* Bad
  * `from pandas import ...`

Can you add a rule to check the opposite?

Example:

* Good
  * `from typing import Callable, Any`
* Bad
  * `import typing` and then `typing.Callable`



---

_Comment by @ntBre on 2025-10-22 13:09_

Thanks for the suggestion! There's a partially related request in https://github.com/astral-sh/ruff/issues/14070, but that's the closest I found.

---

_Label `rule` added by @ntBre on 2025-10-22 13:09_

---

_Label `needs-decision` added by @ntBre on 2025-10-22 13:09_

---

_Comment by @jsh9 on 2025-10-22 20:43_

Thanks!

Another example where a linter like this could benefit:

* Good
  * `from pathlib import Path`
* Bad
  * `import pathlib` and then `pathlib.Path(......)`

---

_Comment by @MichaReiser on 2025-10-23 06:17_

If we implement this, it's more likely to implement it as a configuration option than as a rule because we don't want conflicting rules (this can lead to infinite fixes or cases where Ruff needs to guess and disable one of the two if both are enabled)

---

_Comment by @CAM-Gerlach on 2025-12-06 21:30_

Would that only allow using one of them on any given project? If so, that wouldn't be much of an improvement, at least for us, since as is common we want to require imports `from` some modules, e.g. `typing` and `pathlib` while banning them from others, e.g. NumPy, Pandas, Matplotlib. Wouldn't it be sufficient to error if `banned-from` is disjoint on the two checks if both are enabled (and neither is the empty set), which would prevent any conflict?

---
