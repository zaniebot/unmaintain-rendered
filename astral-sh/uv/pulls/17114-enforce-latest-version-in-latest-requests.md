```yaml
number: 17114
title: "Enforce latest-version in `@latest` requests"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tool
created_at: 2025-12-12T22:43:34Z
updated_at: 2025-12-13T15:39:03Z
url: https://github.com/astral-sh/uv/pull/17114
synced_at: 2026-01-12T16:12:37Z
```

# Enforce latest-version in `@latest` requests

---

_@charliermarsh_

## Summary

Given `uv tool install {name}@latest`, we make revalidation requests for `{name}`, but we don't actually add a "latest" constraint when resolving -- we just assume that since the package is unpinned, and we're fetching the latest available versions, the resolver will select the latest version. 

However, imagine a package in which the latest version requires Python 3.13 or later, but prior versions support Python 3.9 and up. If we happen to select Python 3.9 ahead of resolution, and the user requests `{name}@latest`, we would backtrack to the non-latest version due to the Python mismatch.

This PR modifies `uv tool install` and `uv tool run` to first determine the latest version, then provide it as a constraint when resolving.


---

_Label `bug` added by @charliermarsh on 2025-12-12 22:43_

---

_Review requested from @zanieb by @charliermarsh on 2025-12-12 22:43_

---

_Marked ready for review by @charliermarsh on 2025-12-12 22:43_

---

_@charliermarsh reviewed on 2025-12-12 22:44_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:709 on 2025-12-12 22:44_

(Looking for a nicer way to do this.)

---

_Converted to draft by @charliermarsh on 2025-12-12 22:49_

---

_Marked ready for review by @charliermarsh on 2025-12-12 23:07_

---

_@zanieb approved on 2025-12-13 15:38_

---

_Merged by @charliermarsh on 2025-12-13 15:39_

---

_Closed by @charliermarsh on 2025-12-13 15:39_

---

_Branch deleted on 2025-12-13 15:39_

---
