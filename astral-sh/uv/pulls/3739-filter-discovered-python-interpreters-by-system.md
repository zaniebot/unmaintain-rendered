```yaml
number: 3739
title: Filter discovered Python interpreters by system preference
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/filter-system-preferences
created_at: 2024-05-22T15:45:42Z
updated_at: 2024-05-22T16:22:11Z
url: https://github.com/astral-sh/uv/pull/3739
synced_at: 2026-01-12T16:05:49Z
```

# Filter discovered Python interpreters by system preference

---

_@zanieb_

Previously, we enforced `SystemPython` outside of the interpreter discovery exclusively with source selection. Now, we perform additional filtering of interpreters depending on if they are a virtual environment. This should not change any existing behavior, but will make it much easier to have consistent behavior in ambiguous cases like https://github.com/astral-sh/uv/pull/3736#discussion_r1610072262 where a source could provide either a system interpreter or virtual environment interpreter.

---

_Label `internal` added by @zanieb on 2024-05-22 15:45_

---

_@charliermarsh approved on 2024-05-22 16:17_

---

_Comment by @charliermarsh on 2024-05-22 16:18_

Is it the case today that running with `--system` from within a virtualenv will _not_ discover the venv Python, even as last resort? I can't remember.

---

_Comment by @zanieb on 2024-05-22 16:21_

@charliermarsh yeah that's the case, although I could see situations where we may want to do something else in the future

---

_Merged by @zanieb on 2024-05-22 16:22_

---

_Closed by @zanieb on 2024-05-22 16:22_

---

_Branch deleted on 2024-05-22 16:22_

---
