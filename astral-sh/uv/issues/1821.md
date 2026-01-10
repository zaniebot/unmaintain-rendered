```yaml
number: 1821
title: Raise error on duplicate editables
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2024-02-21T17:39:00Z
updated_at: 2024-02-22T02:28:00Z
url: https://github.com/astral-sh/uv/issues/1821
synced_at: 2026-01-10T05:40:32Z
```

# Raise error on duplicate editables

---

_Issue opened by @charliermarsh on 2024-02-21 17:39_

If two editables resolve to the same package name, we silently drop one of them.

Try copying `black_editable` as `duplicate_editable`, then resolving:

```txt
-e ./scripts/editable-installs/black_editable
-e ./scripts/editable-installs/duplicate_editable
```

Which yields:

```txt
-e ./scripts/editable-installs/duplicate_editable
```


---

_Label `error messages` added by @charliermarsh on 2024-02-21 17:39_

---

_Comment by @charliermarsh on 2024-02-21 17:46_

(`pip sync` and `pip install` correctly fail.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-21 18:13_

---

_Closed by @charliermarsh on 2024-02-22 02:28_

---

_Closed by @charliermarsh on 2024-02-22 02:28_

---
