```yaml
number: 7916
title: Fix documentation (projects guide) regarding adding a git dependency
type: pull_request
state: merged
author: VolkerH
labels:
  - documentation
assignees: []
merged: true
base: main
head: volkerh/doc-fix-guides-projects-add-git
created_at: 2024-10-04T08:35:10Z
updated_at: 2024-10-04T10:42:23Z
url: https://github.com/astral-sh/uv/pull/7916
synced_at: 2026-01-10T12:53:59Z
```

# Fix documentation (projects guide) regarding adding a git dependency

---

_Pull request opened by @VolkerH on 2024-10-04 08:35_

## Summary

This is a trivial, one line documentation change that fixes the following documentation bug.

The current documentation suggests this for adding a git dependency

```
# Add a git dependency

uv add requests --git https://github.com/psf/requests
```

Executing what is suggested with `uv` version `0.4.18` results in this error message

```
uv add requests --git https://github.com/psf/requests
error: unexpected argument '--git' found
```

The working approach is to add the git depency like this:

```
uv add git+https://github.com/psf/requests
```

## Test Plan

I manually tested the command suggested currently in the guide against my change.
 

---

_@charliermarsh approved on 2024-10-04 10:42_

---

_Label `documentation` added by @charliermarsh on 2024-10-04 10:42_

---

_Merged by @charliermarsh on 2024-10-04 10:42_

---

_Closed by @charliermarsh on 2024-10-04 10:42_

---

_Comment by @charliermarsh on 2024-10-04 10:42_

Thx!

---
