```yaml
number: 410
title: "When installing, check that the installation fits with the `.dist-info/METADATA`"
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2023-11-13T12:20:20Z
updated_at: 2023-12-13T08:13:45Z
url: https://github.com/astral-sh/uv/issues/410
synced_at: 2026-01-10T05:40:31Z
```

# When installing, check that the installation fits with the `.dist-info/METADATA`

---

_Issue opened by @konstin on 2023-11-13 12:20_

When installing, we should for each wheel we install check:
 * All requires dist entries are fulfilled (part of the `InstallPlan`)
 * The requires python entry matches the current python version

I expect this will catch some bugs

---

_Label `enhancement` added by @konstin on 2023-11-13 12:20_

---

_Added to milestone `Future` by @charliermarsh on 2023-11-13 14:33_

---

_Comment by @charliermarsh on 2023-11-14 04:35_

This will also be required for #129, since if a package is installed, we should look up its dependencies from the virtualenv so make sure they too get installed.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-13 03:11_

---

_Closed by @konstin on 2023-12-13 08:13_

---
