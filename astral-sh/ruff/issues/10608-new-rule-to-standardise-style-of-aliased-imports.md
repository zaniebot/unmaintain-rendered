---
number: 10608
title: New rule to standardise style of aliased imports
type: issue
state: open
author: ngnpope
labels:
  - rule
assignees: []
created_at: 2024-03-26T10:12:56Z
updated_at: 2024-03-26T10:21:29Z
url: https://github.com/astral-sh/ruff/issues/10608
synced_at: 2026-01-10T01:22:50Z
---

# New rule to standardise style of aliased imports

---

_Issue opened by @ngnpope on 2024-03-26 10:12_

So, I've had a good look through all the available rules and found nothing that quite matches this.

I could have an aliased import in this style:

```python
import pyarrow.parquet as pq
```

Or in this style:

```python
from pyarrow import parquet as pq
```

It would be nice to select one form or the other. I imagine a setting would be a good idea as people have differing preferences. I think this should stay away from having per-package/module configuration to allow going both ways in different cases - it'll just over-complicate things.

This would probably fit nicely under the [`ICN`](https://docs.astral.sh/ruff/rules/#flake8-import-conventions-icn) set of rules?

FWIW, this is also quite close to [`PLR0402`](https://docs.astral.sh/ruff/rules/manual-from-import/), but that's only aimed at avoiding `import x.y as y` cases.


---

_Label `rule` added by @AlexWaygood on 2024-03-26 10:21_

---

_Referenced in [astral-sh/ruff#10599](../../astral-sh/ruff/issues/10599.md) on 2024-03-26 10:23_

---
