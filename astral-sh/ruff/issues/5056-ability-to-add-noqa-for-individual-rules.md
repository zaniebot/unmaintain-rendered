```yaml
number: 5056
title: Ability to add noqa for individual rules
type: issue
state: closed
author: gaborbernat
labels:
  - question
assignees: []
created_at: 2023-06-13T16:40:45Z
updated_at: 2023-06-13T17:11:01Z
url: https://github.com/astral-sh/ruff/issues/5056
synced_at: 2026-01-12T15:54:45Z
```

# Ability to add noqa for individual rules

---

_@gaborbernat_

Would be nice if `--add-noqa` could take an optional argument to only add those ignores for a given rule, and not globally.

---

_Renamed from "Ability to automatically add overrides" to "Ability to add noqa for individual rules" by @gaborbernat on 2023-06-13 16:44_

---

_Comment by @charliermarsh on 2023-06-13 16:58_

I believe this does work, or at least it's intended to work. `ruff check foo.py --add-noqa --select F841` will only add `noqa` directives for `F841`.

---

_Label `question` added by @charliermarsh on 2023-06-13 16:58_

---

_Comment by @gaborbernat on 2023-06-13 17:11_

Thanks!

---

_Closed by @gaborbernat on 2023-06-13 17:11_

---
