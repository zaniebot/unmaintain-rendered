```yaml
number: 5486
title: "Support `resolve_call_path` resolution for same-file definitions"
type: issue
state: closed
author: charliermarsh
labels:
  - core
assignees: []
created_at: 2023-07-03T17:30:49Z
updated_at: 2023-11-09T03:38:08Z
url: https://github.com/astral-sh/ruff/issues/5486
synced_at: 2026-01-12T15:54:45Z
```

# Support `resolve_call_path` resolution for same-file definitions

---

_@charliermarsh_

If a user specifies a `classmethod` decorator or other setting based on a call path, we only respect that call path when the decorator is _imported_.

For example, if the file is `foo.py`, and user defines a method like:

```python
def bar():
  ...
```

Then if the user lists `"foo.bar"` as a call path for a call path-based setting, we _only_ respect it in files that `import foo` or `from foo import bar` etc. -- we wouldn't respect it in `foo.py` itself.


---

_Label `core` added by @charliermarsh on 2023-07-03 17:30_

---

_Closed by @charliermarsh on 2023-11-09 03:38_

---
