```yaml
number: 13303
title: "Converting `__file__` to a named path variable triggers E402"
type: issue
state: open
author: ncoghlan
labels:
  - type-inference
assignees: []
created_at: 2024-09-10T06:05:47Z
updated_at: 2024-09-10T17:49:02Z
url: https://github.com/astral-sh/ruff/issues/13303
synced_at: 2026-01-10T11:09:55Z
```

# Converting `__file__` to a named path variable triggers E402

---

_Issue opened by @ncoghlan on 2024-09-10 06:05_

While the `E402` detection makes some allowances for `sys.path` manipulation, it still doesn't like it if `__file__` gets assigned to a variable, as in code like:

```
_THIS_DIR = Path(__file__).parent
_REPO_DIR = _THIS_DIR.parent

if find_spec("some_project_module") is None:
    sys.path.append(str(_REPO_DIR / "src"))
from some_project_module import relevant_api
```

This inline version is correctly detected as permitted `sys.path` manipulation:
```
if find_spec("some_project_module") is None:
    sys.path.append(str(Path(__file__).parent.parent / "src"))
from some_project_module import relevant_api
```

While `#noqa: E402` makes the error go away in the first case, it doesn't seem right that it triggers a lint error while the more cryptic inline version gets a free pass. At the same time, tracing and allowing `__file__` manipulation would presumably be difficult, so `wontfix` may be the right response here.

---

_Label `type-inference` added by @MichaReiser on 2024-09-10 17:48_

---

_Comment by @MichaReiser on 2024-09-10 17:49_

Thanks for opening this. We may bel able to do better here once Ruff uses type inference. 

---
