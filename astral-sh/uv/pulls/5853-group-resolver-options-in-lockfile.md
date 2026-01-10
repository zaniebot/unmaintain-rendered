```yaml
number: 5853
title: Group resolver options in lockfile
type: pull_request
state: merged
author: konstin
labels:
  - preview
  - lock
assignees: []
merged: true
base: main
head: konsti/group-resolver-options
created_at: 2024-08-07T12:29:16Z
updated_at: 2024-08-07T18:12:00Z
url: https://github.com/astral-sh/uv/pull/5853
synced_at: 2026-01-10T13:31:54Z
```

# Group resolver options in lockfile

---

_Pull request opened by @konstin on 2024-08-07 12:29_

There are three options that determine resolver behavior:

* resolution mode
* prerelease mode
* exclude newer

They are different from the other top level options: If they mismatch, we recreate the resolution. To distinguish them from the rest of the lockfile, we group them under an `[options]` header.

1/3 for #4893

---

_Label `preview` added by @konstin on 2024-08-07 12:29_

---

_Label `lock` added by @konstin on 2024-08-07 12:29_

---

_@charliermarsh approved on 2024-08-07 13:06_

---

_Merged by @charliermarsh on 2024-08-07 18:11_

---

_Closed by @charliermarsh on 2024-08-07 18:11_

---

_Branch deleted on 2024-08-07 18:12_

---
