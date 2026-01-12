```yaml
number: 5380
title: Allow symlinks to files in scripts directory
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/sym-script
created_at: 2024-07-23T22:11:18Z
updated_at: 2024-07-23T22:24:44Z
url: https://github.com/astral-sh/uv/pull/5380
synced_at: 2026-01-12T16:06:47Z
```

# Allow symlinks to files in scripts directory

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/5359.

## Test Plan

Unfortunately, the only packages I know of that use this are Ruff and uv, and both are too heavy to install in a recurring test, so:

`uv tool install hatch==1.12.0 --with uv==0.2.27 --force --link-mode=symlink`


---

_Label `bug` added by @charliermarsh on 2024-07-23 22:16_

---

_Marked ready for review by @charliermarsh on 2024-07-23 22:16_

---

_Merged by @charliermarsh on 2024-07-23 22:24_

---

_Closed by @charliermarsh on 2024-07-23 22:24_

---

_Branch deleted on 2024-07-23 22:24_

---
