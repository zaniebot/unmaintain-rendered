```yaml
number: 8907
title: Remove source distribution filename from cache
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
  - cache
assignees: []
merged: true
base: main
head: charlie/length-i
created_at: 2024-11-08T00:25:37Z
updated_at: 2024-11-08T00:50:07Z
url: https://github.com/astral-sh/uv/pull/8907
synced_at: 2026-01-12T16:08:33Z
```

# Remove source distribution filename from cache

---

_@charliermarsh_

## Summary

In the example outlined in https://github.com/astral-sh/uv/issues/8884, this removes an unnecessary `jupyter_contrib_nbextensions-0.7.0.tar.gz` segment (replacing it with `src`), thereby saving 39 characters and getting that build working on my Windows machine.

This should _not_ require a version bump because we already have logic in place to "heal" partial cache entries that lack an unzipped distribution.

Closes https://github.com/astral-sh/uv/issues/8884.

Closes https://github.com/astral-sh/uv/issues/7376.


---

_Label `bug` added by @charliermarsh on 2024-11-08 00:26_

---

_Label `windows` added by @charliermarsh on 2024-11-08 00:26_

---

_Label `cache` added by @charliermarsh on 2024-11-08 00:26_

---

_Merged by @charliermarsh on 2024-11-08 00:50_

---

_Closed by @charliermarsh on 2024-11-08 00:50_

---

_Branch deleted on 2024-11-08 00:50_

---
