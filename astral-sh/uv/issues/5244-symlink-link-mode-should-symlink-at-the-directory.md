```yaml
number: 5244
title: Symlink link mode should symlink at the directory level
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-07-20T11:15:38Z
updated_at: 2024-07-29T01:09:00Z
url: https://github.com/astral-sh/uv/issues/5244
synced_at: 2026-01-12T15:58:55Z
```

# Symlink link mode should symlink at the directory level

---

_@charliermarsh_

Rather than the file level. We should make the method look more like the “clone” variant rather than the “hard link” variant”.

See: https://github.com/astral-sh/uv/issues/5147#issuecomment-2240922522

---

_Label `enhancement` added by @charliermarsh on 2024-07-20 11:15_

---

_Label `help wanted` added by @charliermarsh on 2024-07-20 11:15_

---

_Comment by @charliermarsh on 2024-07-20 13:10_

Reconsidering this: I’m not sure that it’s the right approach, because we’ll then pollute the cache with compiled .pyc files.

---

_Closed by @charliermarsh on 2024-07-29 01:09_

---
