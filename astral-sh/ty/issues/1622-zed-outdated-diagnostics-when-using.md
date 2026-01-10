```yaml
number: 1622
title: "Zed: Outdated diagnostics when using `diagnosticMode: \"openFiles\"`"
type: issue
state: open
author: MichaReiser
labels:
  - editor
assignees: []
created_at: 2025-11-24T17:35:50Z
updated_at: 2025-12-31T15:36:24Z
url: https://github.com/astral-sh/ty/issues/1622
synced_at: 2026-01-10T01:56:40Z
```

# Zed: Outdated diagnostics when using `diagnosticMode: "openFiles"`

---

_Issue opened by @MichaReiser on 2025-11-24 17:35_


Create a file `lib.py`:

```py
class Project:
    name: str

```

Create a second file `main.py`:

```py
from lib import Project

p = Project()

print(p.name)
```


Now, change `lib.py` to 

```py
class Project:
    name: str

```

There should now be a diagnostic for `print(p.name)` but Zed fails to fetch the diagnostics for `main.py`. 

Note, that ty uses `relatedInformation: true` when registering for pull diagnostics.

I reported this issue upstream, see https://github.com/zed-industries/zed/issues/31259#issuecomment-3571961820

---

_Label `server` added by @MichaReiser on 2025-11-24 17:35_

---

_Label `server` removed by @MichaReiser on 2025-12-04 13:54_

---

_Label `editor` added by @MichaReiser on 2025-12-04 13:54_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:36_

---
