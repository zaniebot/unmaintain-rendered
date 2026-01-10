```yaml
number: 1607
title: Avoid propagating top-level options to sub-resolutions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rec
created_at: 2024-02-17T18:31:09Z
updated_at: 2024-02-17T18:53:39Z
url: https://github.com/astral-sh/uv/pull/1607
synced_at: 2026-01-10T15:33:24Z
```

# Avoid propagating top-level options to sub-resolutions

---

_Pull request opened by @charliermarsh on 2024-02-17 18:31_

## Summary

It's incorrect to pass the resolution and dependency mode down to the `BuildDispatch`, since it means that we'll use `--no-deps` when building source distributions. If you set resolution to `lowest`, it also means we end up using (e.g.) the lowest version of `wheel`, which also doesn't make sense.

It's fine to pass `--exclude-newer`.

Closes https://github.com/astral-sh/uv/issues/1355.

Closes https://github.com/astral-sh/uv/issues/1563.


---

_Label `bug` added by @charliermarsh on 2024-02-17 18:48_

---

_Merged by @charliermarsh on 2024-02-17 18:53_

---

_Closed by @charliermarsh on 2024-02-17 18:53_

---

_Branch deleted on 2024-02-17 18:53_

---
