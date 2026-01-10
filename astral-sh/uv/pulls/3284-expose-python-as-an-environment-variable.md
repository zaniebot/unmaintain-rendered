```yaml
number: 3284
title: "Expose `--python` as an environment variable"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/use-python
created_at: 2024-04-26T23:27:28Z
updated_at: 2024-04-30T03:32:41Z
url: https://github.com/astral-sh/uv/pull/3284
synced_at: 2026-01-10T14:37:54Z
```

# Expose `--python` as an environment variable

---

_Pull request opened by @charliermarsh on 2024-04-26 23:27_

## Summary

This was requested offline, and seems reasonable to me.

---

_Label `configuration` added by @charliermarsh on 2024-04-26 23:27_

---

_Marked ready for review by @charliermarsh on 2024-04-26 23:27_

---

_Comment by @zanieb on 2024-04-29 14:40_

Should we just go with `UV_PYTHON` to match our usual naming scheme? Discussed offline that `UV_USE_PYTHON` sort of implies a boolean option.

---

_Comment by @charliermarsh on 2024-04-29 14:41_

Yeah works for me.

---

_Merged by @charliermarsh on 2024-04-30 03:32_

---

_Closed by @charliermarsh on 2024-04-30 03:32_

---

_Branch deleted on 2024-04-30 03:32_

---
