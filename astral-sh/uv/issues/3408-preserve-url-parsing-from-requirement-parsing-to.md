```yaml
number: 3408
title: "Preserve url parsing from `Requirement` parsing to `Dist`"
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2024-05-06T14:14:50Z
updated_at: 2024-05-14T01:23:29Z
url: https://github.com/astral-sh/uv/issues/3408
synced_at: 2026-01-12T15:58:43Z
```

# Preserve url parsing from `Requirement` parsing to `Dist`

---

_@konstin_

Part of #3407:
* Decide which type to store in `PubGrubPackage` and `Urls` and add them. For the first step, we should retain the `VerbatimUrl` and only add the parsed field, like `Requirement` does.
* Use these fields in `Dist::from_url`.


---

_Label `enhancement` added by @konstin on 2024-05-06 14:14_

---

_Assigned to @konstin by @konstin on 2024-05-06 14:14_

---

_Renamed from "Preserve urls from `Requirement` parsing to `Dist`" to "Preserve url parsing from `Requirement` parsing to `Dist`" by @konstin on 2024-05-06 14:17_

---

_Closed by @charliermarsh on 2024-05-14 01:23_

---
