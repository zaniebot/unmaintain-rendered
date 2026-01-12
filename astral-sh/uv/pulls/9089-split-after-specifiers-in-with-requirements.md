```yaml
number: 9089
title: "Split after specifiers in `--with` requirements"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/w
created_at: 2024-11-13T16:09:56Z
updated_at: 2024-11-13T16:20:42Z
url: https://github.com/astral-sh/uv/pull/9089
synced_at: 2026-01-12T16:08:38Z
```

# Split after specifiers in `--with` requirements

---

_@charliermarsh_

## Summary

Part of me wants to revert support for `--with "flask, requests"`, but the multiple specifiers case actually isn't ambiguous, and handling it is better than shipping a breaking change in a patch release.

Closes https://github.com/astral-sh/uv/issues/9081.


---

_Label `bug` added by @charliermarsh on 2024-11-13 16:10_

---

_Merged by @charliermarsh on 2024-11-13 16:20_

---

_Closed by @charliermarsh on 2024-11-13 16:20_

---

_Branch deleted on 2024-11-13 16:20_

---
