```yaml
number: 13379
title: Retain dot-separated wheel tags during cache prune
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cache
assignees: []
merged: true
base: main
head: charlie/prune-cache
created_at: 2025-05-10T18:28:16Z
updated_at: 2025-05-10T18:39:12Z
url: https://github.com/astral-sh/uv/pull/13379
synced_at: 2026-01-12T16:10:40Z
```

# Retain dot-separated wheel tags during cache prune

---

_@charliermarsh_

## Summary

If a set of wheel tags includes a dot, this code is treating the part _after_ the dot as an extension, and thereby failing to detect that the entry is a symlink to an archive (and thereby removing the archive).

This is all an optimization, so this code just makes it a little targeted: we skip specific known extensions, rather than anything with any extension.

Closes https://github.com/astral-sh/uv/issues/13270.


---

_Label `bug` added by @charliermarsh on 2025-05-10 18:28_

---

_Label `cache` added by @charliermarsh on 2025-05-10 18:28_

---

_Marked ready for review by @charliermarsh on 2025-05-10 18:29_

---

_Merged by @charliermarsh on 2025-05-10 18:39_

---

_Closed by @charliermarsh on 2025-05-10 18:39_

---

_Branch deleted on 2025-05-10 18:39_

---
