```yaml
number: 3397
title: "Accept list of sources with markers in `uv.tool.sources`"
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - help wanted
  - projects
assignees: []
created_at: 2024-05-06T08:11:09Z
updated_at: 2024-09-30T21:16:45Z
url: https://github.com/astral-sh/uv/issues/3397
synced_at: 2026-01-10T04:45:09Z
```

# Accept list of sources with markers in `uv.tool.sources`

---

_Issue opened by @konstin on 2024-05-06 08:11_

`tool.uv.sources` should accept a list of dicts instead of a single dict, too, to support splitting for different python versions (detailed design TBD):

```toml
[project]
dependencies = [
    "idna>=2.8; python_version > '3.11'",
    "idna<2.8; python_version <= '3.11'"
]

[tool.uv.sources]
idna = [
  { url = "https://example.org/idna-2.8.zip, python = "<3.11" },
  { url = "https://example.org/idna-2.7.zip, python = ">=3.11" },
]
```

---

_Label `preview` added by @konstin on 2024-05-06 08:11_

---

_Renamed from "uv source: Accept list instead of dict" to "Uv sources: Accept list instead of dict" by @konstin on 2024-05-06 08:11_

---

_Label `projects` added by @zanieb on 2024-06-27 21:44_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Label `enhancement` added by @zanieb on 2024-08-22 16:26_

---

_Renamed from "Uv sources: Accept list instead of dict" to "Accept list of sources with markers in `uv.tool.sources`" by @zanieb on 2024-08-23 15:42_

---

_Label `help wanted` added by @charliermarsh on 2024-09-10 13:47_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-27 18:29_

---

_Comment by @charliermarsh on 2024-09-27 23:27_

In review here: https://github.com/astral-sh/uv/pull/7745

---

_Closed by @charliermarsh on 2024-09-30 21:16_

---
