```yaml
number: 345
title: Respect direct URLs in puffin installer
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/support-direct-url
created_at: 2023-11-06T19:21:38Z
updated_at: 2023-11-07T14:11:29Z
url: https://github.com/astral-sh/uv/pull/345
synced_at: 2026-01-12T16:03:53Z
```

# Respect direct URLs in puffin installer

---

_@charliermarsh_

We now write the `direct_url.json` when installing, and _skip_ installing if we find a package installed via the direct URL that the user is requesting.

A lot of TODOs, especially around cleaning up the `Source` abstraction and its relationship to `DirectUrl`. I'm gonna keep working on these today, but this works and makes the requirements clear.

Closes #332.

---

_Label `enhancement` added by @charliermarsh on 2023-11-06 19:21_

---

_Review requested from @konstin by @charliermarsh on 2023-11-06 19:26_

---

_@konstin approved on 2023-11-07 09:40_

---

_Merged by @charliermarsh on 2023-11-07 14:11_

---

_Closed by @charliermarsh on 2023-11-07 14:11_

---

_Branch deleted on 2023-11-07 14:11_

---
