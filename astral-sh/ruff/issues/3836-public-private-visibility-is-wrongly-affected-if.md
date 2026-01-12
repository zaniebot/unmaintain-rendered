```yaml
number: 3836
title: public/private visibility is wrongly affected if there is an unrelated parent directory that starts with _
type: issue
state: closed
author: Hnasar
labels:
  - bug
assignees: []
created_at: 2023-03-31T22:11:00Z
updated_at: 2023-04-03T15:38:49Z
url: https://github.com/astral-sh/ruff/issues/3836
synced_at: 2026-01-12T15:54:44Z
```

# public/private visibility is wrongly affected if there is an unrelated parent directory that starts with _

---

_@Hnasar_

With an empty file, `foo.py`, if you place it in a folder like `_bar/baz/quux`, even though the directories are not packages, ruff thinks that `foo.py` is a private module. This affects the pydocstyle "D" rules https://beta.ruff.rs/docs/rules/#pydocstyle-d

patch to fix: #3835

---

_Label `bug` added by @charliermarsh on 2023-04-01 14:24_

---

_Closed by @charliermarsh on 2023-04-03 15:38_

---
