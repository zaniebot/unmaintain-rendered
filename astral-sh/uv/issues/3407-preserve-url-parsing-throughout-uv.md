```yaml
number: 3407
title: Preserve url parsing throughout uv
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2024-05-06T14:13:30Z
updated_at: 2024-07-09T11:27:58Z
url: https://github.com/astral-sh/uv/issues/3407
synced_at: 2026-01-10T05:31:37Z
```

# Preserve url parsing throughout uv

---

_Issue opened by @konstin on 2024-05-06 14:13_

Currently, we have a rich `Requirement` type, storing both the url component and the verbatim input.  We than lose this parsed information through `Urls`, `to_pubgrub` and `PubGrubPackage` storing only a single `VerbatimUrl`. We eventually parse this url again in `ResolutionGraph::from_state` -> `Dist::from_url` again, but then store only the `VerbatimUrl` on the `Dist` again. When using the `Dist`, we parse the url again.

Instead, we should store the parsed url in `Urls` and the `PubGrubPackage`, and later in the `Dist` variants. This can be split into subitems:
* #3408
* #3409
* #3410

---

_Label `enhancement` added by @konstin on 2024-05-06 14:13_

---

_Assigned to @konstin by @konstin on 2024-05-06 14:14_

---

_Renamed from "Preserve urls through uv" to "Preserve url parsing throughout uv" by @konstin on 2024-05-06 14:19_

---

_Unassigned @konstin by @konstin on 2024-06-20 09:07_

---

_Closed by @konstin on 2024-07-09 11:27_

---
